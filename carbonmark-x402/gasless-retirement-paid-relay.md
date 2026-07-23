# Gasless retirement (paid relay)

## Gasless retirement (paid relay)

Retire carbon without holding native ETH, without a prior token approval, and without a Base Account — sign a single token authorization and a Klima executor submits the transaction for you.

The relay path lets any wallet or agent retire carbon by signing **one** standard EIP-712 token authorization. A Klima executor submits the on-chain transaction and is reimbursed for gas out of your signed budget. For the path where you submit the transaction yourself, see [Retire carbon with x402](https://claude.ai/chat/retire-carbon-with-x402.md).

### What you sign

You sign the **token authorization, not the retirement**. The only signature is a standard token authorization:

* [EIP-3009](https://eips.ethereum.org/EIPS/eip-3009) `TransferWithAuthorization` for USDC, or
* [EIP-2612](https://eips.ethereum.org/EIPS/eip-2612) `Permit` for kVCM

This is exactly what any x402-style payment signs. The retirement parameters (credit, amount, beneficiary) are **not** part of the signed message; they travel in the request body and are bound to your signature only by the authorized spend `value`. Client signing is therefore plain `eth_signTypedData_v4`;there is no custom typed data to assemble.

### Before you begin

You will need:

* A wallet holding an accepted input token (USDC or kVCM) — **no ETH required**
* A client capable of `eth_signTypedData_v4`

### The flow

\{% stepper %\} \{% step %\}

#### Prepare the authorization

`POST /api` with `prepare-auth`. The server resolves and prices the retirement and returns `typedData` (the EIP-712 object to sign) and `actionsRetireRequest` (a ready-to-send request body).

\{% endstep %\}

\{% step %\}

#### Sign the typed data

Sign `typedData` in your wallet with `signTypedData`. This is the only signing step.

\{% endstep %\}

\{% step %\}

#### Submit the retirement

`POST /api` with `actions/retire`, sending the request body plus your signature. A Klima executor relays the transaction on-chain.

\{% endstep %\}

\{% step %\}

#### Resolve the certificate

`POST /api` with `certificate` and the `txHash` to get the public proof. Poll if the transaction is still pending. Once completed, the retire endpoint will also return the retirement certificate URL.

\{% endstep %\} \{% endstepper %\}

```
1. POST /api  prepare-auth   → server resolves + prices, returns:
                                 • typedData            (EIP-712 object to sign)
                                 • actionsRetireRequest (ready-to-send body)
2. wallet     signTypedData(typedData)        ← the ONLY signing step
3. POST /api  actions/retire (body + signature) → executor relays on-chain
4. POST /api  certificate    { txHash }       → public proof (poll if pending)
```

\{% hint style="info" %\} `prepare-auth` is the `200` alias of the `402` challenge that `actions/retire` returns when it is posted **without** an `authPayload`. Either entry point gives you the same `typedData`. \{% endhint %\}

### How the signed budget works

The signed budget (`authValue`) covers the retirement, the protocol fee, and the executor's gas reimbursement, with a slippage buffer. The signer needs only an input-token balance (USDC or kVCM) — no ETH. Send the `actionsRetireRequest` body verbatim so that `from` and `to` match the signed authorization.

### `actions/retire` responses

| Status                              | Meaning                                                                                                              |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `settled`                           | Mined and indexed — `retirements[]` carries the certificate URL(s).                                                  |
| `pending_index`                     | Mined or broadcast, but the subgraph has not caught up — poll `/certificate` with the tx hash.                       |
| `400 insufficient_authorized_value` | Your signed `value` no longer covers retirement + fee + gas (price or gas moved). Re-run `prepare-auth` and re-sign. |
| `400 invalid_auth_payload`          | `from` ≠ request `from`, or `to` ≠ settlement contract. Use the `actionsRetireRequest` body verbatim.                |
| `422 transaction_reverted`          | Mined but reverted on-chain; no retirement recorded.                                                                 |

### Reference client

A runnable end-to-end reference client covering both USDC and kVCM lives at [`api/scripts/paid-retire.ts`](https://github.com/KlimaDAO/x402-retire/blob/main/api/scripts/paid-retire.ts). It runs the full sequence: `prepare-auth` → sign → submit → certificate.

### Notes

* You sign a token authorization, not the retirement itself.
* The signer needs only an input-token balance — no ETH.
* Re-run `prepare-auth` and re-sign if you hit `insufficient_authorized_value`; do not blind-retry.
* Send the `actionsRetireRequest` body verbatim to keep the signature valid.
* Retirement is irreversible once the transaction confirms.
