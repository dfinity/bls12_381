# Unreleased

## Changed
- Based on bls12_381 0.9.2 (see the upstream release notes for 0.9.0, 0.9.1
  and 0.9.2 below). This includes breaking changes, notably:
  - MSRV is now 1.85.0, and the crate uses the 2024 edition.
  - Bumped dependencies to `ff 0.14`, `group 0.14`, `pairing 0.24.2` and
    `rand_core 0.10`. For example, `Field::random` and `Group::random` now
    take `&mut R` with `R: rand_core::Rng`, `group::Curve::AffineRepr` is now
    `Curve::Affine`, and the `PrimeCurveAffine` methods moved to
    `group::CurveAffine`.
  - `experimental`: Hashing to curves now follows Internet Draft v16 (hash
    outputs are unchanged), and its API has changed:
    `HashToCurve::{hash_to_curve, encode_to_curve}` and
    `HashToField::hash_to_field` take a `Message`, such as `[msg]`, instead of
    `impl AsRef<[u8]>` or `&[u8]`; `ExpandMessageState` and
    `InitExpandMessage` were merged into `ExpandMessage`; and `HashToField`
    has a new `XofOutputLength` associated type.
  - `experimental` now enables `groups`.
  - New `sqrt-table` feature flag, enabled by default (requires `alloc`). It
    speeds up `Scalar` square roots with precomputed tables whose memory
    accesses depend on the input. It needs atomic compare-and-swap, so
    thumbv6m-none-eabi builds need `default-features = false`.
- Avoid a variable-time division of secret scalar digits in upstream's new
  radix-3 scalar recoding (used by `G1Projective`/`G2Projective` scalar
  multiplication) on targets without native 64-bit division, such as
  thumbv6m-none-eabi and wasm32.
- On 32-bit ARM targets (e.g. Ledger devices), `Fp`/`Fp2` inversion, square
  roots and hashing to curves use square-and-multiply instead of upstream's new
  addition chains, which need several KiB of additional stack.

# ic_bls12_381 0.10.1
## Changed
- Work around an LLVM bug which caused very slow compilation when
  building this crate for wasm32-unknown-unknown in release mode

# ic_bls12_381 0.10
## Changed
- Update sha2/sha3/digest dependencies from 0.9 to 0.10

# ic_bls12_381 0.9.2
## Changed
- Yanked
- Update sha2/sha3/digest dependencies from 0.9 to 0.10

# ic_bls12_381 0.9.1
## Changed
- Avoid problems with position independent code on thumbv6m-none-eabi devices
- Smaller stack usage during final exponentiation

# ic_bls12_381 0.9.0
## Changed
- Based on bls12_381 0.8
- Smaller stack usage during final exponentiation

# ic_bls12_381 0.8.0
## Changed
- Based on bls12_381 0.7
- Support serialization of Gt elements
- Add zeroize support for Gt

# 0.9.2
## Changed
- Raised `pairing` minimum version to `0.24.2`, to provide a more efficient
  implementation of `pairing::MillerLoopResult::final_exponentiation_vartime`.
- Improved performance.

## Fixed
- The `groups` feature flag was broken by 0.9.1; it now works again without the
  `pairings` feature flag.

# 0.9.1
## Added
- `sqrt-table` feature flag (enabled by default), which enables large
  precomputed tables (stored on the heap) that speed up square root
  computation.
- `bls12_381::G1Projective::to_affine_vartime`
- `bls12_381::G2Projective::to_affine_vartime`

## Changed
- Raised `pairing` minimum version to `0.24.1`, to provide a more efficient
  implementation of `pairing::MultiMillerLoop::prepare_reusable_g2`.
- Improved performance.

# 0.9.0
## Changed
- MSRV is now 1.85.0.
- Bumped dependencies to `digest 0.10`, `ff 0.14`, `group 0.14`, `pairing 0.24`,
  `rand_core 0.10`.
- `experimental`: Migrated to Hashing to curves
  ([Internet Draft v16](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-hash-to-curve-16)).

# 0.8.0
## Changed
- Bumped dependencies to `ff 0.13`, `group 0.13`, `pairing 0.23`.

# 0.7.1
## Added
- `Mul` implementations for `{G1Affine, G1Projective, G2Affine, G2Projective}`
  with `Scalar` on the left-hand side. These enable code to be written matching
  equations that use the additive notation `[a] P`.
- Zeroization support for `Gt`.

# 0.7.0

## Changed
- MSRV bumped to `1.56.0`
- Bumped dependencies to `ff 0.12`, `group 0.12`, `pairing 0.22`.

# 0.6.1

## Changed
- G2 arithmetic is now 25-30% faster across the board.
- Pairings are now 10-15% faster.

# 0.6.0

## Fixed
- `bls12_381::Gt::default()` now returns `Gt::identity()` instead of a nonsensical value.

## Added
- Zeroization support for most public types, behind the `zeroize` feature flag.
- `bls12_381::MillerLoopResult` trait implementations:
  - `Default`
  - `AddAssign<MillerLoopResult>`
  - `AddAssign<&MillerLoopResult>`

## Changed
- Bumped dependencies to `ff 0.11`, `group 0.11`, `pairing 0.21`.

## Removed
- The deprecated `endo` feature flag.

# 0.5.0

## Added
- `bits` feature flag (on by default) that exposes an `ff::PrimeFieldBits` implementation
  on `bls12_381::Scalar`.
- `experimental` feature flag, for features have no backwards-compatibility guarantees and
  may change at any time.
- Hashing to curves ([Internet Draft v11](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-hash-to-curve-11)),
  behind the `experimental` feature flag.

## Changed
- Bumped dependencies to `ff 0.10`, `group 0.10`, `pairing 0.20`.
- MSRV is now 1.51.0.

# 0.4.0

## Changed
- Bumped dependencies to `ff 0.9`, `group 0.9`, `pairing 0.19`, `rand_core 0.6`.
- The `endo` feature is on by default. The `endo` feature flag itself is deprecated, and
  will be removed in a future minor release.
- MSRV is now 1.47.0.

# 0.3.1

# Fixed
* The crate now compiles for non-64-bit targets, such as the `wasm32-*` targets.

# 0.3.0

# Changed
* Migrated to `ff 0.8`, `group 0.8`, and `pairing 0.18`.
* MSRV is now 1.44.0.
* Switched to complete addition formulas for G1/G2.

# 0.2.0

This release adds implementations of the `ff`, `group`, and `pairing` traits (with the
latter two being gated by the `groups` and `pairings` feature flags respectively).
Additional trait implementations (for standard traits) have been added where the `ff`,
`group`, and `pairing` trait bounds require them.

## Added
* `bls12_381::Bls12`, a `pairing::Engine` for BLS12-381 pairing operations. It implements
  the following traits:
  * `pairing::{Engine, MultiMillerLoop}`
* New trait implementations for `bls12_381::G1Projective`:
  * `group::{Curve, Group, GroupEncoding, WnafGroup}`
  * `group::prime::{PrimeCurve, PrimeGroup}`
* New trait implementations for `bls12_381::G1Affine`:
  * `group::{GroupEncoding, UncompressedEncoding}`
  * `group::prime::PrimeCurveAffine`
  * `pairing::PairingCurveAffine`
* New trait implementations for `bls12_381::G2Projective`:
  * `group::{Curve, Group, GroupEncoding, WnafGroup}`
  * `group::prime::{PrimeCurve, PrimeGroup}`
* New trait implementations for `bls12_381::G2Affine`:
  * `group::{GroupEncoding, UncompressedEncoding}`
  * `group::prime::PrimeCurveAffine`
  * `pairing::PairingCurveAffine`
* New trait implementations for `bls12_381::Gt`:
  * `group::Group`
* New trait implementations for `bls12_381::MillerLoopResult`:
  * `pairing::MillerLoopResult`
* New trait implementations for `bls12_381::Scalar`:
  * `ff::{Field, PrimeField}`

# 0.1.1

Added `clear_cofactor` methods to `G1Projective` and `G2Projective`. If the crate feature `endo`
is enabled the G2 cofactor clearing will use the curve endomorphism technique described by
[Budroni-Pintore](https://ia.cr/2017/419). If the crate feature `endo` is _not_ enabled then
the code will simulate the effects of the Budroni-Pintore cofactor clearing in order to keep
the API consistent. In September 2020, when patents US7110538B2 and US7995752B2 expire, the
endo feature will be made default. However, for now it must be explicitly enabled.

# 0.1.0

Initial release.
