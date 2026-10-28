# Skydropx plugin for Grok Build

Connect Grok Build to [Skydropx](https://www.skydropx.com), the shipping platform for
e-commerce in Mexico. Quote with the carriers on your account, buy and track labels,
schedule pickups, manage orders from your connected stores and use your address book,
all through the hosted Skydropx MCP server.

## Installation

In Grok Build, open `/plugin`, search for **Skydropx**, and install.

On first connection, Grok opens the Skydropx sign-in in your browser. Authorize with the
Skydropx account you want Grok to operate. No API key is needed and nothing should be
pasted into chat. A Skydropx account (https://app.skydropx.com) is required; the service
is available for Mexico accounts.

## What you get

- **MCP server** `skydropx` at `https://mcp.skydropx.com/mcp` (Streamable HTTP, OAuth 2.1).
- **Skill** `skydropx`: how to use the tools well (quote before buying, poll async quotes,
  confirm every paid or irreversible action, check saved addresses before asking).

### Tools

| Area | Tools |
|---|---|
| Quote & rates | `quote_shipment`, `get_quotation`, `rate_shipment`, `list_carrier_services`, `list_extra_charges`, `lookup_postal_code`, `verify_address_by_carriers` |
| Shipments | `create_shipment`, `get_shipment`, `list_shipments`, `track_shipment`, `cancel_shipment`, `protect_shipment`, `get_events_since`, `push_tracking_event` |
| Pickups | `schedule_pickup`, `reschedule_pickup`, `reprogram_pickup`, `get_pickup`, `list_pickups`, `pickup_coverage`, `list_office_points` |
| Store orders | `list_orders`, `get_order`, `create_order`, `update_order`, `create_shipment_for_order`, `list_order_labels`, `list_products`, `list_merchant_stores`, `get_merchant_store`, `update_merchant_store`, `list_merchant_store_carriers` |
| Address book & catalogs | `list_address_templates`, `get_address_template`, `create_address_template`, `update_address_template`, `delete_address_template`, `list_packagings`, `list_consignment_notes` |
| Checkout rules & imports | `list_conditional_tariffs`, `create_conditional_tariff`, `create_external_shipment`, `import_external_shipments`, `get_external_shipment_import` |
| Account | `whoami`, `get_credits`, `list_transactions`, `disconnect` |

Every tool carries a `title` and MCP annotations (`readOnlyHint`, `destructiveHint`), so
Grok can tell a lookup from a purchase before calling it. Any action that costs money or
is irreversible (buy a label, cancel, schedule a pickup, edit orders or templates) first
returns `status: confirmation_required` and only executes when called again with
`_confirm: true` after the user agrees.

## Example prompts

- "Quote a 2 kg, 30x20x15 cm package from 06600 to 64000 and tell me the cheapest option arriving within 2 days."
- "Buy the FedEx option that arrives in 2 days and send me the label."
- "Where is my latest FedEx shipment?"
- "Create the label for my store's most recent order using the cheapest rate."
- "Schedule a pickup for tomorrow morning at my warehouse."
- "What's my balance and my latest transactions?"

## Authentication and network

The plugin connects only to `https://mcp.skydropx.com`. Authentication is OAuth 2.1
authorization code with PKCE (S256) and dynamic client registration; Grok handles the flow.

Network endpoints:

- `https://mcp.skydropx.com/mcp`: hosted MCP (Streamable HTTP)
- `https://mcp.skydropx.com/oauth/authorize`, `/oauth/token`, `/oauth/register`: OAuth 2.1 + DCR
- `https://mcp.skydropx.com/.well-known/oauth-authorization-server`, `/.well-known/oauth-protected-resource`: discovery metadata
- `https://app.skydropx.com`: human sign-in and consent screen during authorization

Credentials: a Skydropx account. Scopes `read` (default) and `write`; without `write`,
purchase and mutation tools answer `insufficient_scope`. The MCP server exchanges the grant
for Skydropx tokens scoped to that account and sends them as `Authorization: Bearer`
to the Skydropx API. Skydropx does not train models on your data.

## Links

- Docs: https://mcp.skydropx.com/docs
- Privacy: https://mcp.skydropx.com/legal/privacy
- Terms: https://mcp.skydropx.com/legal/terms
- Security: https://mcp.skydropx.com/legal/security
- Support: hola@skydropx.com

## License

The plugin files in this directory are MIT licensed (see `LICENSE`). The hosted
Skydropx MCP service is governed by the Skydropx terms at https://mcp.skydropx.com/legal/terms.
