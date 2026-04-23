# Beefy (Vault Infrastructure) — Base Sepolia

Deployed: 2026-04 (confirmed on-chain and from local deployment artifacts)

## Infrastructure Contracts

| Contract                                        | Address                                      |
| ----------------------------------------------- | -------------------------------------------- |
| Vault owner (Timelock)                          | `0x5602729d1507cF0FBDAb479220A8166655331905` |
| Vault Factory                                   | `0x51E4561Dd264ee2DF47490acbf80BB2735A4bB37` |
| Multicall (Beefy)                               | `0xb3F76b75623A859eeC180E33b0f886B19C72bA51` |
| BeefyV2AppMulticall                             | `0xb174A2816929ab5B7a126A7Ea424B65Df438b24B` |
| BeefyPriceMulticall                             | `0xAF9a48f499b85d9aa2509AFb9aF50A0b26D06e48` |
| BeefyFeeConfig (proxy)                          | `0xCB6C3B45A0557e36E3C432481dA29b32db930E19` |
| BeefyOracle                                     | `0xf9364d8d5D0ee34a1c1Acf2c40B9bc212aF3D70F` |
| FixedOracle                                     | `0x970317143394a34Bc7ff1d80a44a32eA5E89a1EA` |
| BeefySwapper                                    | `0xb0D539E96e99653a8E7A8D7bfB32c54A853cFC24` |
| Zap                                             | `0xc0fA3412Fd58F5C0CD20588438438bB4f981644b` |
| Deployer/Keeper (historical deployment account) | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |

> **Multicall (Beefy)** — `contracts/BIFI/utils/Multicall.sol` (generic aggregate). Beefy address-book(`beefyfinance.ts`)의 `multicall` 필드 대상. 2026-04-23 배포(tx `0x4ab9d67a4bb8a090cac8f0e4b79e7c032f5335e0e58ffeb43f814899d9a72a91`). Bytecode ~1.6KB.
>
> **BeefyV2AppMulticall** — `contracts/BIFI/infra/BeefyV2AppMulticall.sol`. Beefy **App frontend**가 vault 정보(TVL/APY/balance)를 일괄 조회하기 위한 멀티콜 헬퍼. 2026-04-23 배포. Bytecode ~8.6KB.
>
> **BeefyPriceMulticall** — `contracts/BIFI/infra/BeefyPriceMulticall.sol`. Beefy **API** `fetchAmmPrices.ts`의 `MULTICALLS` 맵 대상(`getLpInfo(address[][])`로 volatile LP의 `totalSupply`·`token0/token1 balanceOf` 일괄 조회 → LP 단가 산출). 2026-04-23 배포(tx `0x91c62cc99e45f1e29236202eb3822f1acf190c2205417e241a8a91bed9935e50`). Bytecode ~1.4KB. Giwa 선례: `0x1a7f8ee57F9CfB1C1f7940e736e352E01DbA6a29`.
>
> ⚠️ `BeefyV2AppMulticall`과 `BeefyPriceMulticall`은 이름이 비슷하지만 **서로 다른 컨트랙트**다. API가 요구하는 것은 `getLpInfo` selector를 갖는 `BeefyPriceMulticall`이며, AppMulticall은 해당 selector가 없어 API에서 사용할 수 없다.

## Aerodrome vaults deployed

### Vault — WETH/USDC (Aerodrome volatile)

| Item                | Value                                        |
| ------------------- | -------------------------------------------- |
| Vault (proxy)       | `0xF9bBf3EC1f0C90Fd7feDCd188A7Bfa73F9A6CD6a` |
| Strategy            | `0xd71A532E728b2307a8E54CBf313e1046918cb62D` |
| Want (LP)           | `0xD6ed234835501Fc468758C7e6AF8918d8758cF9D` |
| Gauge               | `0x6c83D4614433fF9BB437bBB9B19464E60955Bda5` |
| Name                | Moo Aerodrome Base Sepolia WETH-USDC         |
| Symbol              | mooAerodromeBaseSepoliaWETH-USDC             |
| approvalDelay       | 21600 (6h)                                   |
| lpToken0 / lpToken1 | USDC / WETH                                  |
| harvestOnDeposit    | true                                         |

### Vault — DAI/USDC (Aerodrome stable)

| Item                | Value                                        |
| ------------------- | -------------------------------------------- |
| Vault (proxy)       | `0x9913adc053B53053e9Ad9E28E2d9e58237D9Fa87` |
| Strategy            | `0x19a032866501e7F94CC0ed85791F03772E64149C` |
| Want (LP)           | `0x201F470f314bb9f3eE701663b42be3aB976792cB` |
| Gauge               | `0x1c9297E7A99392402A33347aCc9e999fcFe9c786` |
| Name                | Moo Aerodrome Base Sepolia sDAI-USDC         |
| Symbol              | mooAerodromeBaseSepoliasDAI-USDC             |
| approvalDelay       | 21600 (6h)                                   |
| lpToken0 / lpToken1 | USDC / DAI                                   |
| harvestOnDeposit    | true                                         |

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
