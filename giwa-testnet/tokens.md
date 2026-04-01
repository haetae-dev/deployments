# Test Tokens — Giwa Testnet

Deployed: 2026-04-01

| Token | Address | Decimals | Notes |
|-------|---------|----------|-------|
| WETH | `0x4200000000000000000000000000000000000006` | 18 | OP Stack predeploy (not custom deployed) |
| MockUSDC | `0x54eC8566041194E7D6c541AD1565970077365BC7` | 6 | Public `mint(address,uint256)` |
| MockDAI | `0x98846C8323B26E788021Ff6017CB15fb787b2537` | 18 | Public `mint(address,uint256)` |

## Minting Test Tokens

```bash
# Mint 10,000 USDC to your address
cast send 0x54eC8566041194E7D6c541AD1565970077365BC7 \
  "mint(address,uint256)" <YOUR_ADDRESS> 10000000000 \
  --private-key <YOUR_KEY> --rpc-url https://sepolia-rpc.giwa.io

# Mint 10,000 DAI to your address
cast send 0x98846C8323B26E788021Ff6017CB15fb787b2537 \
  "mint(address,uint256)" <YOUR_ADDRESS> 10000000000000000000000 \
  --private-key <YOUR_KEY> --rpc-url https://sepolia-rpc.giwa.io
```
