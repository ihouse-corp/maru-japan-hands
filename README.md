# MARU Japan Hands

Human services in Japan for AI agents, operated by iHouse Co., Ltd. in Osaka, Japan.

- Website: https://hands.senpinai.com/
- MCP endpoint: https://maru.senpinai.com/mcp
- Transport: Streamable HTTP. Connecting and listing tools require no sign-in or API key.
- Service documentation: https://maru.senpinai.com/llms.txt
- Privacy: https://hands.senpinai.com/privacy
- Terms: https://hands.senpinai.com/terms
- Seller details: https://hands.senpinai.com/legal
- Support: https://hands.senpinai.com/contact

This repository contains connection configuration and documentation, not the hosted server implementation. Its documentation license does not license the hosted implementation.

## Connect

Use the public MCP URL in a client supporting Streamable HTTP. For clients with a JSON MCP configuration:

```json
{"mcpServers":{"maru-japan-hands":{"type":"streamable-http","url":"https://maru.senpinai.com/mcp"}}}
```

Gemini CLI configuration is included in `gemini-extension.json`. Cline uses `streamableHttp` in place of `streamable-http`; see `llms-install.md`.

## Services and pricing

Japanese phone calls, restaurant and golf reservations, interpreting, shopping and shipping, on-site checks, queue/pickup and other supported services in Japan. Read `list_services` for current fixed prices, limits and exclusions. Human services are paid according to the existing service price list; connecting to the MCP endpoint does not purchase a service. Quotes and payment are separate steps. Do not create a request or payment request without the user's authorization.

## Tools

The public endpoint returned these 11 tools on 2026-11-17. This includes the wallet integration; it is not just the eight human-service and restaurant tools exposed through Ask Japan.

| Tool | Description |
|---|---|
| `list_services` | List the services MARU Japan Hands provides in Japan (Japanese phone calls, restaurant and golf reservations, shopping proxy, on-site checks, interpreting, Osaka stays, home search and property viewings, app development), with prices in JPY, limits and what is not offered. |
| `check_availability` | Check whether a new request can be taken now. Remote work (calls, online reservations) has a daily limit; on-the-ground work takes one job at a time. Returns open or full and when it reopens. |
| `request_service` | Send a request for a service in Japan. Fixed-price services (phone_call JPY 1,500, restaurant_booking JPY 3,000, interpreting JPY 5,000 per 30 min) return the quote and payment details immediately. Other services get a fixed quote from a person within 2 hours (JST 09:00-21:00); requests sent outside hours are handled from 09:00. No email is needed: follow up with get_request_status. Creating a request is free and does not move any money. Full refund if we cannot do the task or you cancel before work starts. |
| `get_request_status` | Get the status of a request: received, quoted (price and how to pay), paid, in_progress, done (with result), cancelled or refunded. |
| `quote_purchase` | Instant cost estimate to buy an item sold in Japan and ship it: item price (from the shop page the user or agent found), our fee (10%, minimum JPY 2,000) and shipping estimated from Japan Post's official EMS table. Read-only: it does not order or charge anything. |
| `get_menu` | Get a MARU Order restaurant's menu: categories, items with tax-included JPY prices, options (size, toppings), allergens and sold-out flags. Use list shop slugs from the maru://order/shops resource. |
| `check_open` | Check whether a MARU Order restaurant is taking takeout orders now, its hours and whether it accepts JPYC with a discount. |
| `place_takeout_order` | Place a takeout order. Returns an order number and a link. The customer pays at pickup at the counter, or opens the link to pay with JPYC (discounted). This tool never charges or moves money. |
| `maru_wallet_connect` | Connect this AI to the user's MARU wallet (Japanese yen stablecoin, JPYC). The user creates a one-time pairing code in the MARU app (Security > Let AI pay > Add an AI) and tells it to you. Returns an agent token and a 4-symbol fingerprint: tell the user the fingerprint and ask them to approve this AI in the MARU app with Face ID. Until they approve, the token does nothing. Keep the token for later calls; never show it to anyone else. |
| `maru_wallet_request_payment` | File a payment request in the user's MARU wallet. This does NOT pay. The user sees the amount and recipient in the MARU app and approves with Face ID or rejects. Use maru_wallet_request_status to see the outcome. Never tell the user a payment was made until the status is 'done'. |
| `maru_wallet_request_status` | Status of a request made with maru_wallet_request_payment: pending (waiting for the owner), done (paid; includes the transaction link), rejected, or failed. |

Wallet connection/payment requests are separate from human-service requests. Payment approval requires the wallet owner's confirmation. Listing tools for directory verification does not execute any wallet operation.

## Safe verification

Use MCP `initialize` and `tools/list`. For service information, use `list_services`. Do not invoke `request_service`, `place_takeout_order`, or wallet tools as an installation test.
