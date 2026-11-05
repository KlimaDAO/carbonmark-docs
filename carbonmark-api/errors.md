---
description: >-
  Understand error responses from the Carbonmark API and how to recover from
  them.
---

# Errors

The API uses standard HTTP status codes. A `2xx` status means the request succeeded. A `4xx` status means the request could not be completed as sent. A `5xx` status means something went wrong on Carbonmark's side or a dependency is temporarily unavailable.

## Error response formats

Most errors use this format:

```json
{
  "error": "Quote has expired"
}
```

Requests from accounts on [prepaid billing](billing-and-production-access.md) can also return errors with a stable `code` and a `recovery` object:

```json
{
  "error": "Insufficient available API credits.",
  "code": "PREPAID_INSUFFICIENT_BALANCE",
  "recovery": {
    "action": "top_up",
    "requested_amount": 25.5,
    "available_balance": 10,
    "next_available_at": null
  }
}
```

Check for `code` to tell the two formats apart. Use `code`, not the `error` text, in your error handling; the text may change.

## The recovery object

`recovery.action` tells you what to do next:

| Action            | What to do                                                                       |
| ----------------- | -------------------------------------------------------------------------------- |
| `retry`           | Fix the request, or wait, and send it again.                                     |
| `top_up`          | Add funds on the Billing page, then retry.                                       |
| `contact_sales`   | The request is above a plan limit. Request a limit increase on the Billing page. |
| `contact_support` | The account needs attention. Check the Billing page and contact support.         |

Limit and balance errors can include these fields:

<table data-search="false"><thead><tr><th>Field</th><th>Meaning</th></tr></thead><tbody><tr><td><code>requested_amount</code></td><td>The amount the request needed, in USD.</td></tr><tr><td><code>available_balance</code></td><td>Your available balance, in USD.</td></tr><tr><td><code>limit</code></td><td>The limit that was reached, in USD.</td></tr><tr><td><code>used</code></td><td>How much of the limit has been used, in USD.</td></tr><tr><td><code>remaining</code></td><td>How much of the limit is left, in USD.</td></tr><tr><td><code>window_hours</code></td><td>The length of the limit's rolling period, in hours.</td></tr><tr><td><code>next_available_at</code></td><td>When enough capacity returns for this request (ISO 8601, UTC), or <code>null</code> if it will not return on its own.</td></tr></tbody></table>

## Prepaid error codes

These codes are returned by `POST /quotes` and `POST /orders` for prepaid accounts.

| Code                           | HTTP status | Meaning                                                              | Recovery                                                          |
| ------------------------------ | ----------- | -------------------------------------------------------------------- | ----------------------------------------------------------------- |
| `PREPAID_INSUFFICIENT_BALANCE` | 402         | Your available balance does not cover the order.                     | `top_up`                                                          |
| `PREPAID_ACCOUNT_SPEND_LIMIT`  | 409         | The order would exceed your account's spend limit.                   | Wait until `next_available_at`, or request a limit increase.      |
| `PREPAID_KEY_SPEND_LIMIT`      | 409         | The order would exceed this key's weekly spend limit.                | Wait until `next_available_at`, or use a key with a higher limit. |
| `PREPAID_QUOTE_INVALID`        | 400         | The quote has expired or has already been used.                      | Create a new quote.                                               |
| `PREPAID_ORDER_INVALID`        | 400         | The order request is invalid.                                        | Check the request body.                                           |
| `IDEMPOTENCY_KEY_REQUIRED`     | 400         | The `Idempotency-Key` header is empty or longer than 200 characters. | Send a valid key.                                                 |
| `IDEMPOTENCY_CONFLICT`         | 409         | This idempotency key was already used with a different request.      | Use a new key for a new order.                                    |
| `PREPAID_RESTRICTED`           | 403         | Your account is restricted.                                          | `contact_support`                                                 |
| `EXECUTION_UNAVAILABLE`        | 503         | There is not enough liquidity to fill the order right now.           | Retry later, or try a smaller quantity or another listing.        |
| `PREPAID_UNAVAILABLE`          | 503         | Prepaid billing is temporarily unavailable.                          | Retry later.                                                      |

## Retrying orders safely

For production keys on prepaid accounts, `POST /orders` is idempotent. If a request times out or you do not receive a response, send the same request again with the same `Idempotency-Key`. You will get back the original order instead of a second one, and you are charged only once.

```bash
curl --request POST \
  --url https://v20.api.carbonmark.com/orders \
  --header "Authorization: Bearer YOUR_API_KEY" \
  --header "Content-Type: application/json" \
  --header "Idempotency-Key: order-2026-11-03-0001" \
  --data '{ "quote_uuid": "...", "beneficiary_name": "...", "retirement_message": "..." }'
```

* The key can be any string of 1 to 200 characters. Use a new key for each new order.
* If you leave the header out, the `quote_uuid` is used as the key.
* Reusing a key with a different `quote_uuid`, beneficiary, retirement message, or consumption metadata returns `IDEMPOTENCY_CONFLICT`.
