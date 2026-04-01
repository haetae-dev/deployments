# Giwa Testnet

| Item | Value |
|------|-------|
| Chain ID | `91342` |
| RPC | `https://sepolia-rpc.giwa.io` |
| WebSocket RPC | `https://sepolia-rpc-flashblocks.giwa.io` |
| Explorer | `https://sepolia-explorer.giwa.io` |
| Explorer API | `https://sepolia-explorer.giwa.io/api` (no key needed) |
| Faucet (web) | `https://faucet.giwa.io/` (1hr cooldown) |
| Native Token | ETH |
| Stack | OP Stack (Sepolia) |

## Faucet (Nodit API, 24hr cooldown)

```bash
curl -X POST https://console-api.lambda256.io/be/v2.0/faucet/charge \
  -H "Content-Type: application/json" \
  -H "X-API-KEY: w~ui5kSJVtlkd9lV1hf4GzBv4tE-nM8R" \
  -d '{"address":"<YOUR_ADDRESS>","protocol":"giwa","network":"sepolia"}'
```

## Deployer

| Role | Address |
|------|---------|
| Deployer / Team / FeeManager / EmergencyCouncil | `0x5D2cd3e72fb4b08C80D729feE66dA4755E071147` |
