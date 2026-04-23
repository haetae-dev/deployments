# Beefy (Vault Infrastructure) — Base Sepolia

Deployed: 2026-04 (confirmed on-chain and from local deployment artifacts)

## Infrastructure Contracts

| Contract                                        | Address                                      |
| ----------------------------------------------- | -------------------------------------------- |
| Vault owner (Timelock)                          | `0x5602729d1507cF0FBDAb479220A8166655331905` |
| Vault Factory                                   | `0x51E4561Dd264ee2DF47490acbf80BB2735A4bB37` |
| Multicall (Beefy)                               | `0xb3F76b75623A859eeC180E33b0f886B19C72bA51` |
| BeefyV2AppMulticall                             | `0xb174A2816929ab5B7a126A7Ea424B65Df438b24B` |
| BeefyFeeConfig (proxy)                          | `0xCB6C3B45A0557e36E3C432481dA29b32db930E19` |
| BeefyOracle                                     | `0xf9364d8d5D0ee34a1c1Acf2c40B9bc212aF3D70F` |
| FixedOracle                                     | `0x970317143394a34Bc7ff1d80a44a32eA5E89a1EA` |
| BeefySwapper                                    | `0xb0D539E96e99653a8E7A8D7bfB32c54A853cFC24` |
| Zap                                             | `0xc0fA3412Fd58F5C0CD20588438438bB4f981644b` |
| Deployer/Keeper (historical deployment account) | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |

> **Multicall (Beefy)** — `contracts/BIFI/utils/Multicall.sol` (generic aggregate). Beefy address-book(`beefyfinance.ts`)의 `multicall` 필드 대상. 2026-04-23 배포(tx `0x4ab9d67a4bb8a090cac8f0e4b79e7c032f5335e0e58ffeb43f814899d9a72a91`).
>
> **BeefyV2AppMulticall** — `contracts/BIFI/infra/BeefyV2AppMulticall.sol`. Beefy API `fetchAmmPrices.ts`의 `MULTICALLS` 맵 대상(volatile LP 가격 산출). 2026-04-23 배포.

## Oracle Prices (FixedOracle sub-oracle)

| Token | Price   | 비고                                                        |
| ----- | ------- | ----------------------------------------------------------- |
| USDC  | $1.0    | stable anchor                                               |
| DAI   | $1.0    | stable anchor                                               |
| WETH  | $3000   | testnet placeholder                                         |
| AERO  | $0.0045 | AMM 실거래 가격 반영 (2026-04-23, issue #19 사전 방지 조치) |

BeefyOracle staleness = 21600s (6h).

AERO 가격 이력:

- 초기 배포 (2026-04 vault 배포 시점): `$0.1` placeholder
- **수정 (2026-04-23)**: `$0.0045` — AMM 실거래 가격 반영, Cowllector gauge seeding 전 SwapFailed 사전 방지
  - tx: `0xbb942256ebea36a00507dad07f5f3455693edcad5a1f717019bcc410a1dc4107`

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

## BeefySwapper configuration

| 항목               | 값                                                          |
| ------------------ | ----------------------------------------------------------- |
| Oracle             | `0xf9364d8d5D0ee34a1c1Acf2c40B9bc212aF3D70F` (BeefyOracle)  |
| Owner              | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` (Deployer)     |
| Routes (AERO→WETH) | Aerodrome Router `0x2DD7fBB5…8de5`, volatile single-hop     |
| Routes (AERO→USDC) | Aerodrome Router `0x2DD7fBB5…8de5`, volatile single-hop     |
| Routes (AERO→DAI)  | AERO→USDC volatile, USDC→DAI stable (two-hop via Aerodrome) |
| **Slippage**       | `0.99e18` (1% 허용, DEX 표준)                               |

Slippage 이력:

- 초기 배포 (2026-04): `0.095e18` (9.5% 허용, Giwa pre-fix 상태와 동일)
- **수정 (2026-04-23)**: `0.095e18 → 0.99e18` — SwapFailed 재발 방지 및 저 minOut 리스크 완화 + Cowllector gauge seeding 대비 (PROD 전환 전 non-zero min seeding·Oracle 자동동기화 등 추가 MEV hardening 필요)
  - tx: `0x96df0e2bf6543608653d4b9c48ec6c86e06a56bf77d0f9fbc453d4cc9291da9d`

## Harvest SwapFailed 사전 방지 (2026-04-23, issue #19)

Cowllector가 Base Sepolia gauge에 `notifyRewardAmount`로 AERO rewards를 시드하는 시점부터 Giwa `SwapFailed(0xcac88ea9)` (deployments #13)와 동일 패턴 재발이 확정적이었던 상태를 사전 해소.

**상태 (교정 전 · 2026-04-23 사전점검 결과):**

1. FixedOracle AERO 가격 `$0.1` ↔ AMM 실거래 `~$0.005~$0.006` → 20배 괴리
2. BeefySwapper slippage `0.095e18` (9.5% 허용) — Giwa pre-fix 상태와 동일
3. AERO/WETH pool `(0.002 WETH / 1,000 AERO)`, AERO/USDC pool `(5 USDC / 1,000 AERO)` 유동성 극도로 얇음 (harvest swap price impact 만으로도 fail 가능)

**조치 (Giwa PR #14 패턴 동일):**

- Keeper WETH 확보: 0.001 WETH → 0.071 WETH (ETH 0.07 wrap) — tx `0x423e57f759d206dd3610ac3ed66b08ba3898141756cec7d4f4f380f91f98ab06`
- Oracle AERO 가격: `$0.1` → `$0.0045` — tx `0xbb942256ebea36a00507dad07f5f3455693edcad5a1f717019bcc410a1dc4107`
  - 메모: `setOracle` 내부에서 `_getFreshPrice` 호출되지만 `staleness=6h` gate에 걸리면 `latestPrice` cache는 유지. Truth는 `subOracle[AERO].data`, 그 다음 harvest/`getFreshPrice` 호출 시 cache 자동 갱신.
- AERO/WETH pool 시딩: desired `36,000 AERO + 0.068 WETH`, Aerodrome Router가 current ratio 유지 위해 실제 `+34,000 AERO + 0.068 WETH`만 흡수 (잔여 2,000 AERO Keeper 반환) → 총 0.07 WETH / 35,000 AERO — tx `0x9d29fa08efb993f6486123f9936144431c5193344ba3e0a0dbf0a7f7dcd5301a`
- AERO/USDC pool 시딩: `+200,000 AERO + 1,000 USDC` (current ratio 200 AERO/USDC와 정확 일치) → 총 1,005 USDC / 201,000 AERO — tx `0xa8b0a956fa07a6ed56c9298ff572c186b31609b84c132bfbbd92a8078ee53b57`
- BeefySwapper slippage: `0.095e18` → `0.99e18` — tx `0x96df0e2bf6543608653d4b9c48ec6c86e06a56bf77d0f9fbc453d4cc9291da9d`

**검증:**

- `BeefyOracle.subOracle(AERO).data` = `0x000…ffcb9e57d4000` = `4.5e15` ($0.0045) 저장 확인
- `BeefyOracle.getFreshPrice(AERO)` callStatic = `(4.5e15, true)`
- `BeefySwapper.getAmountOut(AERO, WETH, 1000 AERO)` = `0.0015 WETH` (Oracle 매칭 정확)
- `BeefySwapper.getAmountOut(AERO, USDC, 1000 AERO)` = `4.5 USDC` (Oracle 매칭 정확)
- AMM 실제 출력 (x\*y=k, 0.3% fee): AERO/WETH `~0.00194 WETH` / AERO/USDC `~4.97 USDC` → minOut (slippage × Oracle) 대비 충분한 margin
- 양 Strategy `callStatic.harvest()` 양측 no revert (rewards=0 상태이므로 swap 경로 미트리거, 시드 후 실행 시 상기 margin으로 통과 예상)

스크립트: `haetae-dev/haetae-finance-contracts` `scripts/manage/base-sepolia/wrap-weth.ts`, `update-aero-price.ts`, `seed-aero-pools.ts`, `set-swapper-slippage.ts`

## Next Steps (complete)

- [x] Deploy Beefy vault strategy for Aerodrome LP pools (StrategyVelodromeGaugeV2) — WETH/USDC + DAI/USDC
- [x] Register Aerodrome pools in BeefyOracle — FixedOracle fallback 사용
- [x] Configure BeefySwapper with Aerodrome Router — AERO→WETH/USDC/DAI
- [x] BeefySwapper slippage 위험값 보정 (`0.095e18` → `0.99e18`) — issue #19
- [x] AERO Oracle 가격 AMM 실거래 기준 교정 ($0.1 → $0.0045) — issue #19
- [x] AERO/WETH + AERO/USDC pool 유동성 시딩 — issue #19

## Remaining follow-ups

- Publish these addresses in FE/API downstream repos
- Add `haetae-finance-api` Base Sepolia datasets/endpoints (`/vaults/base_sepolia`, `/tokens/base_sepolia`, `/apy`, `/tvl`, `/lps`)
- If production/public usage is planned, clarify whether test-token USDC/DAI should be replaced with canonical Base Sepolia assets
- FixedOracle AERO 가격 주기적 동기화 cron/hook (pool AMM 변동 시 재발 방지)
- Oracle sub-oracle을 FixedOracle → BeefyOracleSolidly로 전환 (Aerodrome pool observations 성숙 후, PROD 전환 전)
- Strategy ownership을 StratTimelock 계정으로 이전 (PROD 전환 전)
