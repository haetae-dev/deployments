# Aerodrome DEX — Base Sepolia

Deployed: 2026-04 (confirmed on-chain and from local deployment artifacts)
Source: Fork of [aerodrome-finance/contracts](https://github.com/aerodrome-finance/contracts)

## Core Contracts

| Contract | Address |
|----------|---------|
| AERO (token) | `0xDe3e9B5bef343fb1991B378d64836e6fD48aD8f7` |
| Router | `0x2DD7fBB5723B069a4B18e11581E1A053aECe8de5` |
| PoolFactory | `0x8edAe844E1C6214FC36cfD252B751149d2969CE9` |
| Voter | `0x98846C8323B26E788021Ff6017CB15fb787b2537` |
| VotingEscrow | `0x67B27452642c9c45749a28213c260a6E25C14B23` |
| Minter | `0x839623413a6906C7F554E0C330D62636F343C419` |
| GaugeFactory | `0x86fDCE50DAD80144801D217bB074084eA7DF67B8` |
| FactoryRegistry | `0x834657FB872fDAda9c77454D02329Be2135283C6` |
| VotingRewardsFactory | `0xD5844cBe16A3f3ab16Ee4885f7161903CaaC6Ca4` |
| ManagedRewardsFactory | `0xFfB6E811E68b55B0dD3bA2748230A6082EFeb29D` |
| Forwarder | `0x73F4dEb0973158a89B7553D282fA33783eAD9883` |
| ArtProxy | `0x72464736E898d8Ac2742f0C1C2b25c4ef7e3301c` |
| Distributor | `0x54eC8566041194E7D6c541AD1565970077365BC7` |
| AirdropDistributor | `0xfaB97D477A8D57d4ce4136dfd9f191E1836efCb4` |

## Pools & Gauges

| Pool | Type | Tokens | Pool Address | Gauge Address |
|------|------|--------|-------------|---------------|
| WETH/USDC | volatile | WETH + USDC | `0xD6ed234835501Fc468758C7e6AF8918d8758cF9D` | `0x6c83D4614433fF9BB437bBB9B19464E60955Bda5` |
| DAI/USDC | stable | DAI + USDC | `0x201F470f314bb9f3eE701663b42be3aB976792cB` | `0x1c9297E7A99392402A33347aCc9e999fcFe9c786` |
| AERO/WETH | volatile | AERO + WETH | `0x930Dd36139cCF43bbf6a08134e639039B6B10f30` | `0xf45a2651948075b241579aD2d0432BDb46F5f335` |
| AERO/USDC | volatile | AERO + USDC | `0x2CF0970C8CCbf5718bD8035009D2A9A1BB39d79A` | `0x7C3AB768A4d8BC779b0D8320cC6c045Fa733c598` |

## Notes

- This Base Sepolia environment uses deployed test tokens for USDC/DAI rather than the public Circle Base Sepolia USDC.
- Local deployment artifacts used for confirmation:
  - `script/constants/output/DeployCore-BaseSepoliaHaetae.json`
  - `script/constants/output/DeployGaugesAndPools-BaseSepoliaHaetae.json`
  - `script/constants/output/DeployTestTokens-BaseSepoliaHaetae.json`

## Beefy Integration

| Beefy Parameter | Value |
|-----------------|-------|
| `solidlyRouter` | `0x2DD7fBB5723B069a4B18e11581E1A053aECe8de5` (Router) |
| `_rewardPool` | Gauge address (per pool, see table above) |
| `_rewards[0]` | `0xDe3e9B5bef343fb1991B378d64836e6fD48aD8f7` (AERO) |
| `want` | Pool address = LP token (see table above) |
