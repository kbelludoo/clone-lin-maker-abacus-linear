# clone-lin-maker-abacus-linear

Experimental **LIN** clone of MakerDAO / Sky `LinearDecrease.price` — Dutch-auction linear decay (`price = rmul(top, mul(tau - dur, RAY) / tau)`, `0` when `dur >= tau`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/abaci.sol` sha256 `a4c3dc0368d66a5bf84e586f0cf76e2e80fb8b336c00348a97cc0d4ddcb016ca` blob `6cbd666267fc9f534ae52a45926eec5aa44b3bc5`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_abacus_linear.lin`](src/lin_maker_abacus_linear.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_abacus_linear_c11.c`](test/oracles/maker_abacus_linear_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_abacus_linear_kernel.c`](test/oracles/maker_abacus_linear_kernel.c)
- **Proofs (lin-open only):** `examples/maker_abacus_linear/` and `make maker-abacus-linear-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 ray. Not a production abacus replacement.

Named goldens: PAR `top=1e9 dur=0 tau=100` → **1000000000**;
HALF `dur=50` → **500000000**;
LAST `dur=99` → **10000000**;
EXPIRED `dur>=tau` → **0**;
FLOOR `top=7 dur=2 tau=5` → **4**;
ODD `123456789,1,3600` → **123422495**;
`tau=0` fail-closes;
price is not wards-auth; rpow/cut/step/Clipper.take/Dog.bark are absent;
OVERFLOW `1e10*1e9` fail-closes while naive i64 wraps.

## Reproduce proofs (in lin-open)

```
make maker-abacus-linear-proof
```

## Not claimed

Uint256 ray / mainnet LinearDecrease.price / StairstepExponentialDecrease rpow parity.
LIN replacing Maker, Sky, clipper, or the EVM. TVL or dollar savings. zk soundness.
Hidden `tau` storage, `wards`/`file`, `block.timestamp`, or Clipper.take.
