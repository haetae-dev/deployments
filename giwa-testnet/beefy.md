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

## Aerodrome Vault 배포 (2026-04-13)

### Oracle 서브 컨트랙트 (신규 배포)

| Contract | Address |
|----------|---------|
| FixedOracle | `0xd71A532E728b2307a8E54CBf313e1046918cb62D` |

등록된 가격 (FixedOracle fallback — Aerodrome pool TWAP observations 부족):

| Token | Price (18 decimals) | Note |
|-------|---------------------|------|
| USDC  | $1.0   | stable anchor |
| DAI   | $1.0   | stable anchor |
| WETH  | $3000  | testnet placeholder |
| AERO  | $0.0045 | AMM 실거래 가격 반영 (2026-04-15, issue #13 해결) |

BeefyOracle staleness = 21600s (6h).

AERO 가격 이력:
- 초기 배포 (2026-04-13): `$0.1` placeholder
- **수정 (2026-04-15)**: `$0.0045` — AMM 실거래 가격(~$0.005~$0.006) 반영하여 harvest SwapFailed 해소
  - tx: `0x54c8a7d203a8d0415e4c9c4457eff12c159cb5f6958ab2ffffbabad334c1a253`

### Vault — WETH/USDC (Aerodrome volatile)

| 항목 | 값 |
|------|------|
| Vault (proxy) | `0x0a4f250Fd76A51cE3fAF4Ca578A253429AE86082` |
| Strategy | `0x2E0E4f013039DcaaD268032085643d079978531b` |
| Want (LP) | `0x69208dba95aaFB30FBa6A5c2E2180c0FC86CE615` |
| Gauge | `0x9Df510a82dd834094c6b08e32935e3A8C60dd3Fe` |
| Name | Moo Aerodrome Giwa WETH-USDC |
| Symbol | mooAerodromeGiwaWETH-USDC |
| approvalDelay | 21600 (6h) |
| lpToken0 / lpToken1 | WETH / USDC |
| harvestOnDeposit | true |

### Vault — DAI/USDC (Aerodrome stable)

| 항목 | 값 |
|------|------|
| Vault (proxy) | `0x4259d1Ed2F59c157e690e1F451FDa42bbAF0496a` |
| Strategy | `0xAa104671064bfd0b555b2F8aB2457f2a54e0005b` |
| Want (LP) | `0x6ec763498F105E2E28B5cD6EF36f112a41753d36` |
| Gauge | `0x0904B398FE1a3179c23a832474A616749D48aD32` |
| Name | Moo Aerodrome Giwa sDAI-USDC |
| Symbol | mooAerodromeGiwasDAI-USDC |
| approvalDelay | 21600 (6h) |
| lpToken0 / lpToken1 | USDC / DAI |
| harvestOnDeposit | true |

Vault 2개 모두 `owner = Vault Timelock (0x0F3A…1BD0)`. Strategy는 `owner = deployer` 유지 (테스트넷 빠른 튜닝 목적, PROD 전환 시 StratTimelock로 이전 예정).

### BeefySwapper 경로 (Aerodrome 4-field Route, selector `0xcac88ea9`)

| From → To | Router | Pools |
|-----------|--------|-------|
| AERO → WETH | Aerodrome Router | AERO/WETH volatile |
| AERO → USDC | Aerodrome Router | AERO/USDC volatile |
| AERO → DAI  | Aerodrome Router | AERO/USDC volatile → DAI/USDC stable (multi-hop) |

모든 경로: `amountIndex=4`, `minIndex=36`, `minAmountSign=0`.

### BeefySwapper slippage 보정 (중요 인프라 수정)

| 항목 | 값 |
|------|------|
| 기존 slippage | `0.095e18` |
| 공식 실측 | `minAmountOut = expected × slippage / 1 ether` ([BeefySwapper.sol:165](https://github.com/…)) |
| 해석 | 0.095 = "최소 출력 = 기대치의 9.5%만 보장" = 90.5% 슬리피지 허용 (치명적 위험) |
| 수정 slippage | `0.99e18` (1% 슬리피지만 허용, DEX 표준) |
| tx | `0x65e6a23f7fba4ad22ddbc6b01cc48fc1823e6b4d7d73db118ad23333b1eeb129` |

### Harvest SwapFailed 해결 (2026-04-15, issue #13)

Cowllector harvest가 `SwapFailed` revert로 2개 Strategy 모두 실패하던 문제 해결.

**원인:**
1. FixedOracle AERO 가격 `$0.1` ↔ AMM 실거래 `~$0.005~$0.006` → 20배 괴리
2. BeefySwapper slippage `0.99e18`로 교정된 후 `minAmountOut = amountIn × 0.99 × OraclePrice / targetOraclePrice`가 실제 AMM 출력의 **20배 요구** → revert 100%
3. 동시에 AERO/WETH pool (0.022 WETH / 11,000 AERO), AERO/USDC pool (5 USDC / 1,000 AERO) 유동성이 지나치게 얇아 1,732 AERO harvest swap 시 price impact로도 fail

**조치:**
- Oracle AERO 가격: `$0.1` → `$0.0045` — tx `0x54c8a7d2…c1a253`
- AERO/WETH pool 시딩: +25,000 AERO + 0.05 WETH → 총 36,000 AERO / 0.072 WETH — tx `0x400dd4ae…fb0e11fb`
- AERO/USDC pool 시딩: +200,000 AERO + 1,000 USDC → 총 201,000 AERO / 1,005 USDC — tx `0xf31d50f2…5df6ca7`
- harvest 실행 검증: WETH/USDC tx `0x159ee0e6…c2da6329`, DAI/USDC tx `0x3298ae38…61ff211b` (status=1, `lastHarvest` 갱신 확인)

스크립트: `haetae-dev/haetae-finance-contracts` (local beefy-contracts fork) `scripts/manage/giwa/update-aero-price.ts`, `scripts/manage/giwa/seed-aero-pools.ts`

### Next Steps (complete)

- [x] Deploy Beefy vault strategy for Aerodrome LP pools (StrategyVelodromeGaugeV2) — WETH/USDC + DAI/USDC
- [x] Register Aerodrome pools in BeefyOracle — FixedOracle fallback 사용
- [x] Configure BeefySwapper with Aerodrome Router — AERO→WETH/USDC/DAI
- [x] BeefySwapper slippage 위험값 보정 (0.095 → 0.99)
- [x] AERO Oracle 가격 AMM 실거래 기준 교정 ($0.1 → $0.0045) — issue #13

### 후속 과제 (PROD 전환 전)

- [ ] Strategy ownership을 StratTimelock (`0xAD87…EBB51`) 로 이전
- [ ] Oracle sub-oracle을 FixedOracle → BeefyOracleSolidly로 전환 (Aerodrome pool observations 성숙 후)
- [ ] FixedOracle AERO 가격 주기적 동기화 cron/hook (pool AMM 변동 시 재발 방지)
- [ ] Harvester bot (Cowllector) 설정값 주입: Strategy 주소 2개, HARVESTER_PK, vault config JSON
- [ ] haetae-finance-api 의 `/vaults/giwa_testnet`, `/apy`, `/tvl` 엔드포인트에 새 Vault 2개 반영 (BE-05 vault config)
- [ ] haetae FE(`src/config`) 에 Vault 2개 등록
