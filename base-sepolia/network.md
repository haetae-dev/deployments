# Base Sepolia

| Item         | Value                                          |
| ------------ | ---------------------------------------------- |
| Chain ID     | `84532`                                        |
| RPC          | `https://sepolia.base.org`                     |
| Explorer     | `https://sepolia.basescan.org`                 |
| Explorer API | `https://api-sepolia.basescan.org/api`         |
| Faucet (web) | `https://www.alchemy.com/faucets/base-sepolia` |
| Native Token | ETH                                            |
| Stack        | OP Stack (Sepolia)                             |

## Notes

- This is a reference deployment. Base Sepolia data is not yet wired into `haetae-finance-api` (`/vaults/base_sepolia` currently returns `chainId not found`).
- The Aerodrome fork and Beefy vault infrastructure were deployed separately from the production Base mainnet Aerodrome; see [aerodrome.md](./aerodrome.md) and [beefy.md](./beefy.md).
- Deployed test tokens are used for USDC/DAI rather than canonical Base Sepolia assets — see [tokens.md](./tokens.md).

## Deployer

| Role                                            | Address                                      |
| ----------------------------------------------- | -------------------------------------------- |
| Deployer / Team / FeeManager / EmergencyCouncil | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |
