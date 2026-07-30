<!--
  DO NOT EDIT. Published automatically from Carbonmark/x402-klima-RA-new/api/docs/3_relay.md.
  Changes made here will be overwritten by the next docs sync.
  Edit the source file and open a PR there instead.
-->

# Gasless retirement (paid relay)

Retire carbon without holding native ETH, without a prior token approval, and without a Base Account — sign a single token authorization and a Klima executor submits the transaction for you.

The relay path lets any wallet or agent retire carbon by signing **one** standard EIP-712 token authorization. A Klima executor submits the on-chain transaction and is reimbursed for gas out of your signed budget. For the path where you submit the transaction yourself, see [Retire carbon with x402](./retire-carbon-with-x402.md).

## What you sign

You sign the **token authorization, not the retirement**. The only signature is a standard token authorization:

* [EIP-3009](https://eips.ethereum.org/EIPS/eip-3009) `TransferWithAuthorization` for USDC, or
* [EIP-2612](https://eips.ethereum.org/EIPS/eip-2612) `Permit` for kVCM

This is exactly what any x402-style payment signs. Client signing is therefore plain `eth_signTypedData_v4`; there is no custom typed data to assemble.

On the **USDC** path the authorization's `nonce` is not random: it is `keccak256(retirement, salt)` — the exact credit, token id, amount, and full attribution struct being authorized, plus a fresh 32-byte `salt` the server mints and returns inside `actionsRetireRequest`. Because `nonce` is a signed EIP-3009 field, your signature covers **what gets retired and who is credited**, not just the spend value. `actions/retire` rebuilds the retirement, re-hashes it with the submitted `salt`, and returns `400 params_mismatch` on a mismatch.

**Post `actionsRetireRequest` back verbatim, `salt` included** (USDC). Without `salt` the authorization cannot be verified (`400 invalid_auth_payload`). The kVCM (Permit) path carries no commitment and no salt — Permit's nonce is the token's own counter.

## Before you begin

You will need:

* A wallet holding an accepted input token (USDC or kVCM) — **no ETH required**
* A client capable of `eth_signTypedData_v4`

## Attribution (required)

A relayed retirement must name its beneficiary. Pass one of:

* `details.beneficiaryAddress` — the party the retirement is for, or
* `beneficiaryIsPayer: true` — credit the paying wallet deliberately

Omitting both returns `400 attribution_required`. Attribution is permanently indexed on-chain and cannot be changed after confirmation.

## The flow

{% stepper %}

{% step %}
#### Prepare the authorization

`POST /api` with `prepare-auth`. The server resolves and prices the retirement and returns `typedData` (the EIP-712 object to sign) and `actionsRetireRequest` (a ready-to-send request body, including `salt` on the USDC path). Check `onChainDetails` before signing — it is exactly what the authorization commits to.
{% endstep %}

{% step %}
#### Sign the typed data

Sign `typedData` in your wallet with `signTypedData`. This is the only signing step.
{% endstep %}

{% step %}
#### Submit the retirement

`POST /api` with `actions/retire`, sending the request body plus your signature. A Klima executor relays the transaction on-chain.
{% endstep %}

{% step %}
#### Resolve the certificate

`POST /api` with `certificate` and the `txHash` to get the public proof. Poll if the transaction is still pending. A `settled` `actions/retire` response already includes certificate URL(s).
{% endstep %}

{% endstepper %}

```
1. POST /api  prepare-auth   → server resolves + prices, returns:
                                 • typedData            (EIP-712 object to sign)
                                 • actionsRetireRequest (ready-to-send body,
                                     incl. salt on the USDC path)
2. wallet     signTypedData(typedData)        ← the ONLY signing step
3. POST /api  actions/retire (body + signature) → executor relays on-chain
4. POST /api  certificate    { txHash }       → public proof (poll if pending)
```

{% hint style="info" %}
`prepare-auth` is the `200` alias of the `402` challenge that `actions/retire` returns when it is posted **without** an `authPayload`. Either entry point gives you the same `typedData`.
{% endhint %}

## How the signed budget works

The signed budget (`authValue`) covers the retirement, the protocol fee, and the executor's gas reimbursement, with a slippage buffer. The signer needs only an input-token balance (USDC or kVCM) — no ETH. Send the `actionsRetireRequest` body **verbatim** so that `from`, `to`, and (on USDC) `salt` match the signed authorization.

## `actions/retire` responses

| Status | Meaning |
| --- | --- |
| `settled` | Mined and indexed — `retirements[]` carries the certificate URL(s). |
| `pending_index` | Mined or broadcast, but the subgraph has not caught up — poll `/certificate` with the tx hash. |
| `400 attribution_required` | No beneficiary named — set `details.beneficiaryAddress` or `beneficiaryIsPayer: true`. |
| `400 insufficient_authorized_value` | Your signed `value` no longer covers retirement + fee + gas (price or gas moved). Re-run `prepare-auth` and re-sign. |
| `400 invalid_auth_payload` | `from` ≠ request `from`, `to` ≠ settlement contract, wrong payload shape, or USDC path missing `salt`. Use the `actionsRetireRequest` body verbatim. |
| `400 params_mismatch` | Submitted retirement does not match the authorized USDC nonce commitment. Re-post verbatim or re-prepare and re-sign. |
| `422 transaction_reverted` | Mined but reverted on-chain; no retirement recorded. |

See [x402 reference](./x402-reference.md) for the full error registry.

## Reference clients

* **SDK** — zero-dependency TypeScript client [`sdk/klima-retire.ts`](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/blob/main/sdk/klima-retire.ts). One `retire()` call runs prepare-auth → sign → submit → poll certificate. Pass `beneficiaryIsPayer: true` or `details.beneficiaryAddress`.
* **Examples** — runnable scripts under [`examples/`](https://github.com/KlimaDAO/Klima-Protocol-x402-MCP-documentation/tree/main/examples) (`retire/retire-sdk.ts`, `retire/paid-retire.ts`, `retire/retire-raw.ts`).

## Notes

* You sign a token authorization, not the retirement itself — and on USDC that authorization binds the retirement via `nonce` + `salt`.
* The signer needs only an input-token balance — no ETH.
* Name a beneficiary (`details.beneficiaryAddress` or `beneficiaryIsPayer: true`) before signing.
* Re-run `prepare-auth` and re-sign if you hit `insufficient_authorized_value`; do not blind-retry.
* Send the `actionsRetireRequest` body verbatim (including `salt` on USDC) to keep the signature valid.
* Retirement is irreversible once the transaction confirms.
