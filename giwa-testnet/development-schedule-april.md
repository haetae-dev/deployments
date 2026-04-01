# Haetae Finance — 4월 개발 일정 (4/1 ~ 4/30)

**목표**: 4월 말 Giwa 테스트넷 데모 — 실제 지갑 연결 → DEX Pool deposit/withdraw → Vault yield farming 동작

**팀**:

- **BC** (K님): 스마트 컨트랙트 (Vault strategy 배포, Oracle/Swapper 설정). 대부분 기존 StrategyVelodromeGaugeV2 배포 스크립트 실행 수준 — 새 컨트랙트 작성 거의 없음. W2 집중, 이후 테스트/검증 위주.
- **BE** (규현님): beefy-api 백엔드 (Giwa 체인 등록, vault 설정, APY 계산)
- **FE** (보석): Haetae 프론트엔드 (실제 체인 연동, 지갑 연결, beefy-api 연동, 트랜잭션 플로우)
- **Tech PM** (보석): 전체 조율, 일정 관리

---

## W1: 4/1 ~ 4/4 (완료 + 세팅)

### BC

- [x] Aerodrome DEX fork → Giwa 테스트넷 배포 완료 (4개 Pool + 4개 Gauge + 유동성)
- [ ] 배포된 컨트랙트 주소 확인 → https://github.com/haetae-dev/deployments

### BE

- [ ] **beefy-api에 Giwa 체인 등록** — `yarn add:chain giwa 91342 https://sepolia-rpc.giwa.io https://sepolia-explorer.giwa.io/ ETH` 실행
- [ ] address-book에 Giwa 주소 입력 (beefyfinance.ts, tokens.ts)
- [ ] beefy-api 로컬 실행 확인

### FE

- [x] haetae chains.ts Giwa RPC 업데이트 완료
- [ ] 해태 랜딩페이지 정리
- [ ] haetae ↔ beefy-api 연동 포인트 정리 (어떤 API endpoint 호출하는지, 데이터 흐름 매핑)
- [ ] haetae ↔ on-chain 직접 호출 포인트 정리 (viem read/write 어디서 하는지)

---

## W2: 4/7 ~ 4/11 (BC: Vault Strategy 배포 / BE: API 연동)

### BC — Vault Strategy 배포

- [ ] `StrategyVelodromeGaugeV2` 배포 스크립트 작성 (`scripts/vault/deploy-giwa-aerodrome.js`)
  - Pool: WETH/USDC volatile → Gauge 연동
  - Router: `0x4e646080c0E8b2a823E2891c7462F9dfD2d91e0A`
  - Reward: AERO `0x842f982FE047045247d732DEE5F71EC86dc81A91`
- [ ] BeefyVaultV7 proxy 배포 (VaultFactory 통해 clone)
- [ ] Strategy → Vault 연결 + Gauge deposit 테스트
- [ ] **최소 2개 Vault 배포**: WETH/USDC, DAI/USDC (stable)
- [ ] BeefyOracle에 Aerodrome pool 가격 피드 등록
- [ ] BeefySwapper에 Aerodrome Router swap 경로 설정

### BE — API 설정

- [ ] `constants.ts`에 Giwa RPC, chain ID 추가
- [ ] `chains.ts`에 giwaChain 정의
- [ ] `web3Helpers.ts`에 Multicall 주소 추가
- [ ] Vault config JSON 작성 (`data/giwa/beefyCowVaults.json`)
- [ ] APY 계산 모듈 기본 세팅 (`api/stats/giwa/`)
- [ ] **API 엔드포인트 테스트**: `/vaults`, `/apy`, `/tvl` 에서 Giwa vault 데이터 확인

### FE

- [ ] 지갑 연결 구현 (mock → real wallet, wagmi/viem)
- [ ] Giwa 테스트넷 네트워크 추가 (chain switch)
- [ ] beefy-api 엔드포인트 연동 (vault 목록, APY, TVL fetch)
- [ ] on-chain read 연동 (balanceOf, allowance, vault share 등)

---

## W3: 4/14 ~ 4/18 (FE: 실제 연동 / BC: 멀티체인 준비)

### BC

- [ ] **멀티체인 1개 선정** (추천: Base Sepolia — OP Stack 동일, Aerodrome 원본 존재)
- [ ] 선정된 체인에 Beefy 인프라 배포
- [ ] 선정된 체인에 Vault 1개 이상 배포
- [ ] harvest() 수동 테스트 (Gauge reward → swap → LP → 재투자 사이클)

### BE

- [ ] beefy-v2 프론트 vault config 작성 (`config/vault/giwa.json`)
- [ ] 멀티체인 API 설정 추가
- [ ] Harvester bot 기본 설정 (수동 harvest 대체 가능)

### FE — 실제 트랜잭션 연동

- [ ] Home: 실제 vault 목록 표시 (API → on-chain)
- [ ] Deposit: Router approve → addLiquidity → Gauge deposit → Vault deposit 플로우
- [ ] Withdraw: Vault withdraw → Gauge withdraw → removeLiquidity 플로우
- [ ] 잔고 표시: 실제 지갑 balance, LP balance, vault share
- [ ] APY/TVL: API에서 가져와서 표시

---

## W4: 4/21 ~ 4/25 (통합 테스트 + 데모 준비)

### BC

- [ ] 멀티체인 Vault 배포 완료
- [ ] 전체 harvest 사이클 E2E 테스트 (deposit → time pass → harvest → compound 확인)
- [ ] edge case 테스트 (0 balance, max withdraw, 가스 부족 등)

### BE

- [ ] 전체 API 안정성 테스트
- [ ] 멀티체인 vault 데이터 통합 확인
- [ ] 에러 핸들링 (RPC 실패, 가격 피드 없음 등)

### FE

- [ ] Compare 화면: 실제 데이터로 자동복리 vs 수동 비교
- [ ] Detail 화면: 실제 전략 정보 표시
- [ ] 멀티체인 전환 UI (체인 선택 → vault 목록 변경)
- [ ] UI 폴리싱 + 반응형 확인
- [ ] E2E 전체 플로우 테스트

---

## W5: 4/28 ~ 4/30 (데모 준비 + 버퍼)

### 전체

- [ ] 데모 시나리오 작성 (스크립트)
- [ ] 데모 환경 세팅 (테스트넷 ETH 충분한지, 토큰 mint)
- [ ] 데모 녹화 또는 라이브 준비
- [ ] 버그 수정 버퍼

---

## BC 담당자 (K님) 즉시 시작 가능한 태스크

1. https://github.com/haetae-dev/deployments 확인 — Aerodrome 배포 주소
2. `beefy-contracts/contracts/BIFI/strategies/Velodrome/StrategyVelodromeGaugeV2.sol` 분석
3. Vault 배포 스크립트 작성 시작 (`scripts/vault/deploy-giwa-aerodrome.js`)
4. 참고: `hardhat.config.ts`에 Giwa 네트워크 이미 설정됨

## BE 담당자 (규현님) 즉시 시작 가능한 태스크

1. `beefy-api` 로컬 실행 환경 세팅
2. `yarn add:chain giwa 91342 https://sepolia-rpc.giwa.io https://sepolia-explorer.giwa.io/ ETH` 실행
3. address-book에 배포된 컨트랙트 주소 입력
4. 참고: https://github.com/haetae-dev/deployments/blob/main/giwa-testnet/beefy.md

---

## 리스크 & 의존성

| 리스크                                      | 영향      | 대응                                                     |
| ------------------------------------------- | --------- | -------------------------------------------------------- |
| Vault strategy 배포 실패 (interface 불일치) | W2 지연   | Aerodrome Gauge interface는 Velodrome과 동일 — 확인 완료 |
| 멀티체인 선정 지연                          | W3 지연   | 의사결정 W2 초까지 완료 필요                             |
| API 설정 복잡도 과소평가                    | W2-3 지연 | `add:chain` 스크립트로 기본 세팅 자동화 가능             |
| FE-BC 연동 시 ABI 불일치                    | W3 지연   | 배포 후 ABI를 즉시 FE에 공유                             |
