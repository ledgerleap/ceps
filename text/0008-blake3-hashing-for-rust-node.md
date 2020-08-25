# Replace use of blake2 hashing with blake3

https://github.com/CasperLabs/ceps/pull/8

## Summary

[summary]: #summary

Replace use of Blake2 in the Rust node with Blake3.

## Motivation

[motivation]: #motivation

Use the blake3 crate for hashing as it is faster in benchmarks than blake2, potentially resulting in an overall performce increase.

## Guide-level explanation

[guide-level-explanation]: #guide-level-explanation

Since the Rust version of the node software is already under development, this wouldn't produce any noticeable difference for end users at first glance. Seeing as how Blake3 hashes are identical to Blake2b hashes, 256bit, are compatible with the same 64bit architecture, and are highly parallelizable, this would be totally an internal update. One would expect to notice an overall increase in speed after the update.

Rust implementation found at:
https://crates.io/crates/blake3 and https://docs.rs/blake3/0.3.6/blake3/

## Reference-level explanation

[reference-level-explanation]: #reference-level-explanation

According to the creators, a test was performed on an AWS c5.metal, 16KiB input, 1 thread, producing the following results compared one against the other:

	Blake2b - 1312 MiB/s
	Blake3  - 6866 MiB/s

These test results were pulled straight from the Blake3 team's repo. Reported to be performed on an Intel Cascade Lake-SP 8275CL modern processor.

## Drawbacks

[drawbacks]: #drawbacks

Could break connected logic if the Bouncycastle library is too heavily relied upon. However it may not be an issue in the new Rust codebase if different dependencies are present.

## Rationale and alternatives

[rationale-and-alternatives]: #rationale-and-alternatives

Rationale speaks for itself I believe, anything that could speed up the network without causing a problem or risks creating a new vector of attack is a positive update.
