# Aerodrome DEX — Giwa Testnet

Deployed: 2026-04-01
Source: Fork of [aerodrome-finance/contracts](https://github.com/aerodrome-finance/contracts)

## Core Contracts

| Contract | Address |
|----------|---------|
| AERO (token) | `0x842f982FE047045247d732DEE5F71EC86dc81A91` |
| Router | `0x4e646080c0E8b2a823E2891c7462F9dfD2d91e0A` |
| PoolFactory | `0xfaB97D477A8D57d4ce4136dfd9f191E1836efCb4` |
| Voter | `0x12545bAcf2D3B4d743520c64D922D9E0f308F689` |
| VotingEscrow | `0x8fccA4DCbfD627D393ccD7f5922BBDfaaEac1E90` |
| Minter | `0xe9E47a3E22486AB26f518A719F891c54Ca41150E` |
| GaugeFactory | `0xCb7480296Bf24bEBD77B6776B35A212af7C7Eeb9` |
| FactoryRegistry | `0xA033D7E195040928257AB7932C665Dc8f2c680D4` |
| VotingRewardsFactory | `0xD850BA79D0882C449593de2dcf6B9AB1dAB46677` |
| ManagedRewardsFactory | `0x95304D74ccD3a99B7115a40ef9Ad0f47B1588B6d` |
| Forwarder | `0x69004ED5aE8B233bb3201762A56485863172dcB5` |
| ArtProxy | `0x221cB4Bb0351425F46dD0828FF865df6166E7920` |
| Distributor | `0xB13dC26E12655973EBb62eeE0655E569c2f6dEB7` |
| AirdropDistributor | `0xA37DD3Ee4531f1C38affA4d9E0DAB5047DdC64c7` |

## Pools & Gauges

| Pool | Type | Tokens | Pool Address | Gauge Address |
|------|------|--------|-------------|---------------|
| WETH/USDC | volatile | WETH + USDC | `0x69208dba95aaFB30FBa6A5c2E2180c0FC86CE615` | `0x9Df510a82dd834094c6b08e32935e3A8C60dd3Fe` |
| DAI/USDC | stable | DAI + USDC | `0x6ec763498F105E2E28B5cD6EF36f112a41753d36` | `0x0904B398FE1a3179c23a832474A616749D48aD32` |
| AERO/WETH | volatile | AERO + WETH | `0x79852B83b3b128e2bEa35E2D83CAEA030027915C` | `0x91C244d445BF75A6Df14CfB98A98f2EFdE35aFa3` |
| AERO/USDC | volatile | AERO + USDC | `0x8DEaE79448Aef9a00A7615CdF5d9EEDbCb508AaD` | `0x2DeA461929EBaf26cc0824aF996bD8eC49d543e9` |

## Verification Commands

```bash
# Pool count (should return 4)
cast call 0xfaB97D477A8D57d4ce4136dfd9f191E1836efCb4 "allPoolsLength()(uint256)" --rpc-url https://sepolia-rpc.giwa.io

# Swap quote: 1 USDC → WETH
cast call 0x4e646080c0E8b2a823E2891c7462F9dfD2d91e0A \
  "getAmountsOut(uint256,(address,address,bool,address)[])(uint256[])" \
  1000000 \
  "[(0x54eC8566041194E7D6c541AD1565970077365BC7,0x4200000000000000000000000000000000000006,false,0xfaB97D477A8D57d4ce4136dfd9f191E1836efCb4)]" \
  --rpc-url https://sepolia-rpc.giwa.io

# Pool reserves
cast call 0x69208dba95aaFB30FBa6A5c2E2180c0FC86CE615 "getReserves()(uint256,uint256,uint256)" --rpc-url https://sepolia-rpc.giwa.io
```

## Beefy Integration

| Beefy Parameter | Value |
|-----------------|-------|
| `solidlyRouter` | `0x4e646080c0E8b2a823E2891c7462F9dfD2d91e0A` (Router) |
| `_rewardPool` | Gauge address (per pool, see table above) |
| `_rewards[0]` | `0x842f982FE047045247d732DEE5F71EC86dc81A91` (AERO) |
| `want` | Pool address = LP token (see table above) |
