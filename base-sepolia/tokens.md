# Test Tokens — Base Sepolia

Deployed: 2026-04 (confirmed from local Aerodrome deployment outputs and on-chain bytecode)

## Tokens used by the deployed Base Sepolia pools

| Token | Address | Decimals | Notes |
|-------|---------|----------|-------|
| WETH | `0x4200000000000000000000000000000000000006` | 18 | Base Sepolia WETH predeploy |
| USDC | `0x0F3Ae09e986f6E57812d662484043F5971151BD0` | 6 | Test token used in deployed Aerodrome pools |
| DAI | `0x5c226913e80c3558d9a900dc88C08D231eD722D6` | 18 | Test token used in deployed Aerodrome pools |
| AERO | `0xDe3e9B5bef343fb1991B378d64836e6fD48aD8f7` | 18 | Reward token |

## Important note

This Base Sepolia deployment does **not** use the public Circle Base Sepolia USDC token. Any downstream integration (frontend, API, docs, demo scripts) must use the addresses above until the environment is explicitly migrated.

## Source artifacts

- `script/constants/output/DeployCore-BaseSepoliaHaetae.json`
- `script/constants/output/DeployTestTokens-BaseSepoliaHaetae.json`
- `script/constants/output/DeployGaugesAndPools-BaseSepoliaHaetae.json`
