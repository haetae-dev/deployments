# Beefy (Vault Infrastructure) — Giwa Testnet

Deployed: 2026-03 (prior to Aerodrome)

## Infrastructure Contracts

| Contract | Address |
|----------|---------|
| Vault Owner (Timelock) | `0x0F3Ae09e986f6E57812d662484043F5971151BD0` |
| Strategy Owner (Timelock) | `0xAD87Bc1b336B80bBAad3EEfcEfaF9736b79EBB51` |
| Multicall | `0xDe3e9B5bef343fb1991B378d64836e6fD48aD8f7` |
| BeefyFeeConfig (proxy) | `0xD5844cBe16A3f3ab16Ee4885f7161903CaaC6Ca4` |
| BeefyFeeConfig (impl) | `0x44dfDc4E7260cE5a5896A104eAED08e05E8E5EFA` |
| Vault V7 (impl) | `0xFfB6E811E68b55B0dD3bA2748230A6082EFeb29D` |
| Vault Factory | `0x834657FB872fDAda9c77454D02329Be2135283C6` |
| BeefyOracle | `0x73F4dEb0973158a89B7553D282fA33783eAD9883` |
| BeefySwapper | `0x72464736E898d8Ac2742f0C1C2b25c4ef7e3301c` |
| Deployer/Keeper/Treasury | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |

## Fee Configuration

| Parameter | Value |
|-----------|-------|
| Total Fee | 9.5% |
| Call Fee | 0.05% |
| Strategist Fee | 0.5% |

## Next Steps

- [ ] Deploy Beefy vault strategy for Aerodrome LP pools (StrategyVelodromeGaugeV2)
- [ ] Register Aerodrome pools in BeefyOracle
- [ ] Configure BeefySwapper with Aerodrome Router
