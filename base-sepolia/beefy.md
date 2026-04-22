# Beefy (Vault Infrastructure) — Base Sepolia

Deployed: 2026-04 (confirmed on-chain and from local deployment artifacts)

## Infrastructure Contracts

| Contract | Address |
|----------|---------|
| Vault owner (Timelock) | `0x5602729d1507cF0FBDAb479220A8166655331905` |
| Vault Factory | `0x51E4561Dd264ee2DF47490acbf80BB2735A4bB37` |
| BeefyFeeConfig (proxy) | `0xCB6C3B45A0557e36E3C432481dA29b32db930E19` |
| BeefySwapper | `0xb0D539E96e99653a8E7A8D7bfB32c54A853cFC24` |
| Deployer/Keeper (historical deployment account) | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |

## Aerodrome vaults deployed

### Vault — WETH/USDC (Aerodrome volatile)

| Item | Value |
|------|------|
| Vault (proxy) | `0xF9bBf3EC1f0C90Fd7feDCd188A7Bfa73F9A6CD6a` |
| Strategy | `0xd71A532E728b2307a8E54CBf313e1046918cb62D` |
| Want (LP) | `0xD6ed234835501Fc468758C7e6AF8918d8758cF9D` |
| Gauge | `0x6c83D4614433fF9BB437bBB9B19464E60955Bda5` |
| Name | Moo Aerodrome Base Sepolia WETH-USDC |
| Symbol | mooAerodromeBaseSepoliaWETH-USDC |
| approvalDelay | 21600 (6h) |
| lpToken0 / lpToken1 | USDC / WETH |
| harvestOnDeposit | true |

### Vault — DAI/USDC (Aerodrome stable)

| Item | Value |
|------|------|
| Vault (proxy) | `0x9913adc053B53053e9Ad9E28E2d9e58237D9Fa87` |
| Strategy | `0x19a032866501e7F94CC0ed85791F03772E64149C` |
| Want (LP) | `0x201F470f314bb9f3eE701663b42be3aB976792cB` |
| Gauge | `0x1c9297E7A99392402A33347aCc9e999fcFe9c786` |
| Name | Moo Aerodrome Base Sepolia sDAI-USDC |
| Symbol | mooAerodromeBaseSepoliasDAI-USDC |
| approvalDelay | 21600 (6h) |
| lpToken0 / lpToken1 | USDC / DAI |
| harvestOnDeposit | true |

## On-chain verification summary

- `vault.strategy()` matches the listed strategy on both vaults
- `strategy.want()` matches the corresponding LP pool on both strategies
- `strategy.gauge()` matches the corresponding gauge on both strategies
- `vault.owner()` matches the Base Sepolia timelock `0x5602729d1507cF0FBDAb479220A8166655331905`
- `strategy.harvestOnDeposit()` is `true` on both strategies

## Remaining follow-ups

- Publish these addresses in FE/API downstream repos
- Add `haetae-finance-api` Base Sepolia datasets/endpoints (`/vaults/base_sepolia`, `/tokens/base_sepolia`, `/apy`, `/tvl`, `/lps`)
- If production/public usage is planned, clarify whether test-token USDC/DAI should be replaced with canonical Base Sepolia assets
