# x402 Reference

Contract addresses, input tokens, amount rules, fees, and the full error code reference for the x402 Endpoint on Base mainnet.

This page is shared reference material for both the [build-your-own](https://claude.ai/chat/retire-carbon-with-x402.md) and [gasless relay](https://claude.ai/chat/gasless-retirement-paid-relay.md) paths.

### Inputs and contracts (Base mainnet)

| Contract              | Address                                                                  |
| --------------------- | ------------------------------------------------------------------------ |
| Input token: USDC     | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`                             |
| Input token: kVCM     | `0x00fbac94fec8d4089d3fe979f39454f48c71a65d`                             |
| Retirement Aggregator | `0xda0a793d7c32ab80bcdab7f8c725c96db22464f4`                             |
| Klima Protocol AAM    | `0x1C24239309398220883207681602BfF4D10fbde1`                             |
| Settlement Contract   | Read from the `prepare` response (`to` / `approvalInstructions.spender`) |

### Amount rules

* Amounts are decimal tonne strings, with a minimum of `0.001` t (1 kg).
* Toucan Puro credits retire in **whole tonnes only**.
* Amounts above a credit's available liquidity are rejected.

### Fees

API calls are free. Each retirement bakes in a protocol fee, computed and collected on-chain by the Settlement Contract:

* The fee is `max(floor, feeBps% of cost)`.
* The floor is denominated in USDC (converted to kVCM via the pool when paying in kVCM).
* It is always included in `quote.fee` and folded into `quote.total`.
* The contract spends exactly `retirementCost + fee` and refunds any unused slippage budget in the same transaction.

### Error reference

Failures return JSON with an `error` code plus actionable fields.

| Status + error                | Meaning                                                                                                           |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `400 schema_validation`       | Malformed or unknown parameter — `issues[]` names it.                                                             |
| `400 unsupported_chain_id`    | Base mainnet only — use `chainId=8453`. (Base Sepolia `84532` hits this; other numbers fail `schema_validation`.) |
| `400 unsupported_input_token` | Use USDC or kVCM.                                                                                                 |
| `400 vintage_not_found`       | Pick from `availableVintages`, or omit `vintage`.                                                                 |
| `400 puro_details_required`   | Supply the Puro `details` fields listed in `missing`, then re-prepare.                                            |
| `404 no_candidates`           | Nothing retirable for that class or credit — re-check `/discover`.                                                |
| `404 retirement_not_found`    | Certificate not indexed yet (retry) or a bad `index`.                                                             |
| `422 amount_not_whole_tonnes` | Puro: request whole tonnes (`nearestDownTonnes` / `nearestUpTonnes` provided).                                    |
| `422 insufficient_liquidity`  | Reduce `amount` (`bestAvailableAtomic` is the max coverable) or pick another credit.                              |
| `422 amount_below_increment`  | Amount converts to zero retirable units — increase it.                                                            |
| `422 contract_revert`         | Decoded on-chain revert — follow `decoded.retryAdvice`; do not blind-retry.                                       |

For the relay-specific statuses returned by `actions/retire`, see [Gasless retirement (paid relay)](https://claude.ai/chat/gasless-retirement-paid-relay.md).

### Resources

* [Paid-retire reference script](https://github.com/KlimaDAO/x402-retire/blob/main/api/scripts/paid-retire.ts) — runnable end-to-end relay client (USDC + kVCM)
* [Klima Retirement Aggregator USAGE](https://github.com/KlimaDAO/retirement-aggregator/blob/main/USAGE.md) — direct on-chain access for callers who do not want the HTTP layer
* [Discovery manifest](https://x402.klimalabs.com/.well-known/x402.json) — public `x402.json` for agent directories
