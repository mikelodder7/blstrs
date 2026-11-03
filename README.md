# `blstrs_plus`

This crate provides an implementation of the BLS12-381 pairing-friendly elliptic curve construction with hash-to-curve and multi-exponentiation methods.

* **This implementation has not been reviewed or audited. Use at your own risk.**
* This implementation targets Rust `1.87` or later.
* This implementation does not require the Rust standard library.
* All operations are constant time unless explicitly noted.

## Features

* `bits` (on by default): Enables APIs for obtaining bit iterators for scalars.
* `groups` (on by default): Enables APIs for performing group arithmetic with G1, G2, and GT.
* `pairings` (on by default): Enables APIs for performing pairings.
* `alloc` (on by default): Enables APIs that require an allocator.
* `ark`: Enables compatibility with the arkworks ecosystem and the Rust standard library.
* `expose-fields`: Exposes the internal field modules; this also enables `groups`.
* `std`: Enables APIs that require the Rust standard library.

## Trait compatibility

* `elliptic-curve`, `ff`, and `group` `0.14` traits are implemented for `Scalar`, the G1/G2 types, and the `Bls12381G1`/`Bls12381G2` curve markers.
* The `ff` and `group` `0.13` traits are also implemented, because `pairing` `0.23` needs them. They are re-exported as `ff_013` and `group_013`.
* `elliptic-curve` `0.13` is no longer supported as of `0.10.0`.

## Hashing to curve

Hash-to-curve follows RFC 9380 and takes any `hash2curve` `0.14` `ExpandMsg` at the 128-bit security level:

```rust
use blstrs_plus::{G1Projective, G2Projective, Scalar};
use hash2curve::ExpandMsgXmd;
use sha2::Sha256;

let msg = b"message";
let p = G1Projective::hash::<ExpandMsgXmd<Sha256>>(msg, b"BLS_SIG_BLS12381G1_XMD:SHA-256_SSWU_RO_NUL_");
let q = G2Projective::hash::<ExpandMsgXmd<Sha256>>(msg, b"BLS_SIG_BLS12381G2_XMD:SHA-256_SSWU_RO_NUL_");
let s = Scalar::hash::<ExpandMsgXmd<Sha256>>(msg, b"my-scalar-dst");
```

This needs `hash2curve = "0.14"` and `sha2 = "0.11"` in your `Cargo.toml`. The DST must be non-empty, or these functions panic.

## Testing

Test for non-wasm32 targets:

```sh
cargo test --all-features
```

Test wasm32 targets:

```sh
cargo test --target wasm32-unknown-unknown
```

## License

Licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
