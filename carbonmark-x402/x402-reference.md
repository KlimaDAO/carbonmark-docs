<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/5_x402-ref.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# x402 Reference

Contract addresses, input tokens, amount rules, fees, endpoints, and the full error code reference for the x402 Endpoint on Base mainnet.

This page is shared reference material for both the [build-your-own](./retire-carbon-with-x402.md) and [gasless relay](./gasless-retirement-paid-relay.md) paths.

## Endpoints

<!-- generated:endpoints -->
<!-- Do not edit by hand: npm run docs:render -->

| Action | How to call | Moves funds? | What it does |
| --- | --- | --- | --- |
| `discover` | `GET /api/discover` or `POST /api` | No | Lists carbon classes, credits, reference USDC/tonne prices, supported input tokens |
| `quote` | `GET /api/quote` or `POST /api` | No | Live on-chain price for a tonnage (retirement cost + protocol fee + slippage buffer) |
| `prepare/retire` | `GET /api/prepare/retire` or `POST /api` | No (you broadcast) | Unsigned `[approve, retire]` batch for self-submit |
| `prepare-auth` | `GET /api/prepare-auth` or `POST /api` | No | EIP-712 `typedData` + ready `actionsRetireRequest` for the relay path |
| `actions/retire` | `POST /api` | Yes (relayed) | Executor submits the retirement; requires signed `authPayload` |
| `certificate` | `GET /api/certificate` or `POST /api` | No | Resolves Carbonmark certificate URL(s) for a `txHash` |

<!-- /generated:endpoints -->

All HTTP calls are free. Use **GET** with query parameters or **POST** JSON to `/api` with an `action` field. Both return the same responses. Live schemas and per-action `errorCodes` are in the [discovery manifest](https://x402.klimalabs.com/.well-known/x402.json).

## Versioning

The API is versioned by host.

| Host | Serves |
| --- | --- |
| `x402.klimalabs.com` | the **latest** release. A breaking release moves it to the new major. |
| `v1.x402.klimalabs.com` | the **v1 major** and every future 1.x minor and patch. Never moves to v2. |
| `v0.x402.klimalabs.com` | the previous major, kept reachable for callers that have not migrated. |

These docs describe v1. Pin `v1.x402.klimalabs.com` if you want the bare host's future major bump not to reach you. `/.well-known/x402.json` and `/.well-known/x402-changelog.json` both publish `apiVersion` and a `versionedHosts` object, and the changelog carries a `migration` string on every breaking release.

## Inputs and contracts (Base mainnet)

<!-- generated:contracts -->
<!-- Do not edit by hand: npm run docs:render -->

| Contract | Address |
| --- | --- |
| Input token: USDC | `0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913` |
| Input token: kVCM | `0x00fbac94fec8d4089d3fe979f39454f48c71a65d` |
| Retirement Aggregator | `0xda0a793d7c32ab80bcdab7f8c725c96db22464f4` |
| Klima Protocol AAM | `0x1C24239309398220883207681602BfF4D10fbde1` |
| Settlement Contract | `0x290e98e95dacE244c73376C0c39A4D53b22E34B6` |

<!-- /generated:contracts -->

Prefer reading the Settlement Contract from each `prepare` / `prepare-auth` response (`to` / `approvalInstructions.spender`) rather than hard-coding it. The address above is the current Base mainnet value from `api/src/config.ts`.

## Amount rules

* Amounts are decimal tonne strings, with a minimum of `0.001` t (1 kg).
* Toucan Puro credits retire in **whole tonnes only**.
* Amounts above a credit's available liquidity are rejected.

## Fees

API calls are free. Each retirement bakes in a protocol fee, computed and collected on-chain by the Settlement Contract:

* The fee is `max(floor, feeBps% of cost)`.
* The floor is denominated in USDC (converted to kVCM via the pool when paying in kVCM).
* It is always included in `quote.fee` and folded into `quote.total`.
* The contract spends exactly `retirementCost + fee` and refunds any unused slippage budget in the same transaction.

## Error reference

Failures return JSON with an `error` code plus actionable fields:

```json
{ "error": "<code>", "message": "…", "x402FacilitatorVersion": 2 }
```

plus code-specific context (`issues` on `schema_validation`, `expectedNonce` / `receivedNonce` on `params_mismatch`, and so on). **Match on `error`, never on `message`** — codes are stable, wording is not.

`retryable: yes` means the identical request can succeed later untouched. `retryable: no` means fix the request first.

<!-- generated:error-codes -->
<!-- Do not edit by hand: npm run docs:render -->

| Code | HTTP | Group | Retryable | Meaning and remedy |
| --- | --- | --- | --- | --- |
| `invalid_json` | 400 | request | no | The request body was not parseable JSON. Send a JSON object with a `content-type: application/json` header. Note the API takes a single POST body, not form-encoded fields. |
| `unknown_action` | 400 | request | no | The body's `action` field is missing or is not one of the supported actions. Set `action` to one of the values in `supported` (echoed in the error body), or GET the endpoint root for the action index. |
| `not_found` | 404 | request | no | No route exists at the requested path. This API is a single POST multiplexer: POST the endpoint URL with an `action` field rather than using per-action paths. The 404 body carries the endpoint and action list. |
| `document_not_found` | 404 | request | no | No documentation document with the requested `id` (from `/api/docs?id=…`). Use one of the ids in the error's `available` list, or fetch the index at /api/docs. |
| `schema_validation` | 400 | request | no | The body failed schema validation. `issues` carries the offending path and reason. Bodies are strict at the top level and inside `details`, so an unrecognized key is an error rather than being silently dropped. Read `issues[].path` and `issues[].keys`. For `unrecognized_keys`, check the key belongs where you put it — attribution fields go inside `details`, not at the top level. |
| `internal_error` | 500 | request | yes | An unhandled server-side failure. Retry with backoff. If it persists, report it via the contact in /.well-known/security.txt with the request body. |
| `unsupported_chain_id` | 400 | resolution | no | `chainId` is not a supported network. Use 8453 (Base mainnet) or 84532 (Base Sepolia). |
| `unsupported_input_token` | 400 | resolution | no | `inputToken` is not an accepted payment token on this chain. Use the USDC or kVCM address for the chain — see the manifest, or the addresses in the endpoint documentation. |
| `invalid_input_token` | 400 | resolution | no | `inputToken` passed validation but matches neither settlement path (EIP-3009 USDC nor EIP-2612 kVCM), so no relay function applies. Use the chain's USDC or kVCM address. |
| `no_candidates` | 404 | resolution | yes | No credit in the carbon class matched the request filters, or the class holds no credits. Call `discover` to list live classes and credits, then retry with a `carbonClass`/`creditToken` from that response. Retryable because class inventory changes. |
| `vintage_not_found` | 400 | resolution | no | No credit in the class carries the requested `vintage`. Pick one of the years in the error's `availableVintages`, or omit `vintage` to let the server choose a liquid credit. |
| `insufficient_liquidity` | 422 | amount | yes | The pool cannot fill the requested amount at any price right now. Reduce `amount`, choose another credit or class, or retry later. Retryable because pool depth changes block to block. |
| `amount_not_whole_tonnes` | 422 | amount | no | The credit's registry (Puro) retires in whole tonnes only, and `amount` has a fractional part. Send an integer `amount` (e.g. "2", not "2.5"). |
| `amount_below_increment` | 422 | amount | no | `amount` is smaller than the credit's minimum retirement unit. Raise `amount` to at least the minimum reported in the error body. |
| `puro_details_required` | 400 | amount | no | The credit is Puro-issued, whose registry requires consumption metadata that the request omitted. Add the fields named in the error body to `details`: `beneficiaryLocation`, `consumptionCountryCode`, `consumptionPeriodStart`, `consumptionPeriodEnd`. |
| `payment_required` | 402 | authorization | no | Not a failure: the x402 challenge returned when `actions/retire` is posted without an `authPayload`. The body carries the EIP-712 `typedData` to sign and a ready-to-send `actionsRetireRequest`. Identical in shape to a `prepare-auth` 200. Sign `typedData` with the payer wallet, set `authPayload.signature` (or `v`/`r`/`s`), and POST `actionsRetireRequest` back — verbatim, including `salt` on the USDC path. |
| `attribution_required` | 400 | authorization | no | A relayed retirement named no beneficiary. The beneficiary is indexed on-chain as a permanent grouping key and cannot be changed once the retirement confirms, so it is not defaulted silently. Set `details.beneficiaryAddress` to the party the retirement is for, or set `beneficiaryIsPayer: true` to credit the paying wallet deliberately. |
| `invalid_auth_payload` | 400 | authorization | no | The authorization is structurally wrong for this request: `authPayload.from` is not the request `from`, `authPayload.to` is not the settlement contract, the payload shape doesn't match the input token's scheme (EIP-3009 for USDC, EIP-2612 for kVCM), or a USDC payload arrived without its top-level `salt`. Post the `actionsRetireRequest` from `prepare-auth` (or the 402 challenge) verbatim, adding only the signature. Do not rebuild the payload by hand. |
| `insufficient_authorized_value` | 400 | authorization | no | The signed `authPayload.value` no longer covers retirement + protocol fee + executor gas, usually because price or gas moved after signing. Relaying it would revert on-chain. Re-run `prepare-auth` (or re-request the 402 challenge) to size a fresh budget of at least `requiredMinimum`, then re-sign. The old authorization is unusable, not merely stale. |
| `params_mismatch` | 400 | authorization | no | The submitted retirement is not the one that was authorized. On the USDC path `authPayload.nonce` is keccak256 of the retirement plus `salt`, so the signature binds the credit, amount, and attribution — not just the spend value. The rebuilt struct hashed to something else. Re-post `actionsRetireRequest` verbatim including `creditToken`, `tokenId`, `details`, and `salt`, or re-run `prepare-auth` and re-sign. A salt is single-use; one from an earlier authorization will not reproduce the nonce. The error echoes `expectedNonce`, `receivedNonce`, and the `submitted` values to diff against. |
| `contract_revert` | 422 | settlement | yes | A contract call reverted during simulation, so nothing was broadcast and no funds moved. `selector` and `decoded.errorName` identify the revert; `contract`, `function`, and `args` give the call context. Read `decoded.errorName`. Liquidity and slippage reverts are worth retrying with a fresh quote; validation and permission reverts are not. |
| `transaction_reverted` | 422 | settlement | yes | The relayed transaction mined but reverted, typically from a state change between simulation and inclusion. No retirement was recorded. Inspect `transactionHash` on a block explorer, then re-run `prepare-auth` and re-sign. The old authorization's nonce may already be consumed. |
| `retirement_not_found` | 404 | settlement | yes | No indexed retirement for that transaction hash. Immediately after confirmation this means the subgraph has not caught up yet, not that the retirement failed. Poll every few seconds. If a retirement response returned `pending_index`, this is the expected interim state. |
| `gas_estimate_unavailable` | 503 | upstream | yes | The executor's gas reimbursement could not be priced, so the authorization budget cannot be sized. No retirement was attempted. Retry with backoff. Nothing was signed or spent, so the request can be repeated unchanged. |

<!-- /generated:error-codes -->

### Which action returns what

<!-- generated:error-codes-by-action -->
<!-- Do not edit by hand: npm run docs:render -->

| Action | Codes specific to it |
| --- | --- |
| `discover` | — |
| `quote` | `unsupported_chain_id`, `unsupported_input_token`, `no_candidates`, `vintage_not_found`, `insufficient_liquidity`, `amount_not_whole_tonnes`, `amount_below_increment`, `contract_revert` |
| `prepare/retire` | `unsupported_chain_id`, `unsupported_input_token`, `no_candidates`, `vintage_not_found`, `insufficient_liquidity`, `amount_not_whole_tonnes`, `amount_below_increment`, `puro_details_required`, `contract_revert` |
| `prepare-auth` | `unsupported_chain_id`, `unsupported_input_token`, `no_candidates`, `vintage_not_found`, `insufficient_liquidity`, `amount_not_whole_tonnes`, `amount_below_increment`, `puro_details_required`, `attribution_required`, `contract_revert`, `gas_estimate_unavailable` |
| `actions/retire` | `unsupported_chain_id`, `unsupported_input_token`, `invalid_input_token`, `no_candidates`, `vintage_not_found`, `insufficient_liquidity`, `amount_not_whole_tonnes`, `amount_below_increment`, `puro_details_required`, `payment_required`, `attribution_required`, `invalid_auth_payload`, `insufficient_authorized_value`, `params_mismatch`, `contract_revert`, `transaction_reverted`, `gas_estimate_unavailable` |
| `certificate` | `retirement_not_found` |

Every action can additionally return: `invalid_json`, `unknown_action`, `not_found`, `document_not_found`, `schema_validation`, `internal_error`.

<!-- /generated:error-codes-by-action -->

The same data is served live at [`/.well-known/x402-errors.json`](https://x402.klimalabs.com/.well-known/x402-errors.json) (GET alias `/api/errors`) — build error handling against that document rather than against this page.

For relay-specific `actions/retire` status values (`settled`, `pending_index`), see [Gasless retirement (paid relay)](./gasless-retirement-paid-relay.md).

## Resources

* [Paid-retire examples](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/tree/main/examples) — runnable end-to-end relay clients (USDC + kVCM)
* [TypeScript SDK](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/sdk/klima-retire.ts) — zero-dependency `retire()` client
* [Klima Retirement Aggregator USAGE](https://github.com/KlimaDAO/retirement-aggregator/blob/main/USAGE.md) — direct on-chain access for callers who do not want the HTTP layer
* [Discovery manifest](https://x402.klimalabs.com/.well-known/x402.json) — public `x402.json` for agent directories
* [Changelog](https://x402.klimalabs.com/.well-known/x402-changelog.json) — machine-readable release history
