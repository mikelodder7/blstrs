# Changelog

All notable changes to `blstrs_plus` will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.10.0] - 2026-11-02

### Security

- Removed `crypto-bigint` `0.5` ([RUSTSEC-2026-0387]) by dropping the `elliptic-curve` `0.13` dependency, and updated `crypto-bigint` `0.7` to `0.7.10`.

### Breaking

- Removed the `elliptic_curve_013` re-export and all `elliptic-curve` `0.13` trait impls (`Curve`, `PrimeCurve`, `FieldBytesEncoding`, `AffineCoordinates`, `MulByGenerator`, `LinearCombination`, `ScalarPrimitive`, `FromUintUnchecked`, `Reduce`, `Invert`, `IsHigh`). The `ff`/`group` `0.13` traits stay because `pairing` needs them.
- `hash`/`encode` now take an `X: hash2curve::ExpandMsg<U16>` from `hash2curve` `0.14`, for example `hash2curve::ExpandMsgXmd<sha2::Sha256>` with `sha2` `0.11`.
- `Scalar` conversions to and from `U256`/`U384`/`U512` now use `crypto-bigint` `0.7` types. `From<Scalar> for U384`/`U512` now produce the correct value, and `From<U384>`/`From<[u8; 48]>` reduce modulo the group order instead of truncating.
- The `elliptic-curve` `0.14` `AffineCoordinates::FieldRepr` for `G1Affine`/`G2Affine` is now `elliptic_curve::array::Array` instead of `generic_array::GenericArray`.

### Changed

- Added `hash2curve` `0.14` for `expand_message`, and bumped `sha2` to `0.11`.
- Updated `ctutils` to `0.4.7` ([RUSTSEC-2026-0385]) and `crossbeam-epoch` to `0.9.21` ([RUSTSEC-2026-0204]) in `Cargo.lock`.
- G1 and G2 now share one isogeny evaluation helper. Output is unchanged and checked against the RFC 9380 test vectors.

[RUSTSEC-2026-0385]: https://rustsec.org/advisories/RUSTSEC-2026-0385
[RUSTSEC-2026-0204]: https://rustsec.org/advisories/RUSTSEC-2026-0204
[RUSTSEC-2026-0387]: https://rustsec.org/advisories/RUSTSEC-2026-0387

## [0.9.0] - 2026-07-31

- Updated core cryptography trait dependencies to their latest compatible patch releases.
- Bumped `criterion` to `0.8.2`.
- Used `elliptic-curve-tools` for faster small- and medium-sized sums of products with `alloc`.
- Added `elliptic-curve`, `ff`, and `group` `0.14` compatibility while preserving the `0.13` traits required by `pairing`.
- Added arkworks compatibility behind the `ark` feature.
- Implemented `IsHigh` for the field and scalar types.

## [0.8.15] - 2024-02-16

- Match bls12_381_plus methods
- Update dependencies

## [0.8.9] - 2023-10-18

- Match bls12_381_plus methods

## [0.8.6] - 2023-10-18

- Fixed issue with 32-bit builds

## [0.1.0] - 2020-10-08

- Initial release
