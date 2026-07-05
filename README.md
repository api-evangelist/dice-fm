# DICE (dice-fm)

DICE is a live music and events ticketing platform ([dice.fm](https://dice.fm)) connecting fans to gigs, club nights, festivals, and experiences through a fan-facing mobile app, plus a partner platform (MIO) for promoters, venues, and festivals. For partners, DICE exposes a documented but **access-gated** GraphQL API - the **Ticket Holders API** at `https://partners-endpoint.dice.fm/graphql` - that lets downstream systems query events, tickets and ticket holders, orders and sales, returns and transfers, fans, and venues.

## Access Model (Read This First)

The DICE partner API is **real and documented**, but it is **not an open self-service developer program**:

- **Partner-gated.** API access is a benefit of being an onboarded DICE partner (promoter, venue, or festival) that sells tickets through DICE. There is no public "sign up and get a key" flow.
- **Tokens come from MIO.** Partners select events and generate an API token inside the MIO platform ([mio.dice.fm](https://mio.dice.fm/)); the token is passed as a Bearer `Authorization` header.
- **GraphQL, single endpoint.** The surface is one GraphQL endpoint (`POST https://partners-endpoint.dice.fm/graphql`), not a set of REST paths. Schema docs are public at [partners-endpoint.dice.fm/graphql/docs](https://partners-endpoint.dice.fm/graphql/docs/index.html).
- **Endpoints modeled, not live-tested.** The logical APIs below are modeled from DICE's public GraphQL schema documentation and example queries. Field and query names are taken from those docs; they were not exercised against a live token, because access is partner-gated. See `review.yml` (`endpointsModeled: true`).

## APIs.json

[https://raw.githubusercontent.com/api-evangelist/dice-fm/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/dice-fm/refs/heads/main/apis.yml)

## Tags

- Ticketing
- Live Music
- Events
- Tickets
- GraphQL
- Entertainment
- Partner API

## Timestamps

- **Created:** 2026-07-05
- **Modified:** 2026-07-05

## APIs

All logical APIs below are surfaced through the single **Ticket Holders GraphQL API** (`https://partners-endpoint.dice.fm/graphql`), authenticated with a MIO-issued Bearer token. Queries use the authenticated `viewer` root and `node(id)` object lookups, with cursor-based pagination (`first`/`after`/`before`/`last` and `PageInfo`).

### DICE Events API

Query a partner's events - name, state, start/end datetimes, currency, URL, artists, genres, ticket types, price tiers, and total ticket allocation - via the `viewer.events` connection or by `node(id)`.

### DICE Tickets and Ticket Holders API

Retrieve who currently holds a ticket for access management and attendance tracking - ticket code, ticket type, seat, full price, fees, commission and DICE commission, the holder (fan), claim timestamp, and extras with separate access barcodes. Exposed as the `tickets` connection on an event with filters (for example `ticketTypeId`).

### DICE Orders and Sales API

Query orders and sales to understand event finances and feed BI - purchase timestamp, quantity, sales channel, full price, commission and DICE commission, fee breakdown, purchasing fan, and the tickets on each order. Supports date-range filters (for example `purchasedAt gte`).

### DICE Returns and Transfers API

Track ticket returns (`Return`: ticket, order, returned-at, reason) and transfers (`TicketTransfer`: tickets, orders, transferred-at) between fans, with date-range filtering and pagination.

### DICE Venues API

Read venue details attached to events - name, type, age limit, address fields, coordinates, and timezone.

## Common Properties

- [GitHub Organization](https://github.com/dicefm)
- [LinkedIn](https://www.linkedin.com/company/dice-fm)
- [Website](https://dice.fm)
- [Documentation](https://partners-endpoint.dice.fm/graphql/docs/index.html)
- [Portal (MIO)](https://mio.dice.fm/)
- [Sign Up (Partners)](https://dice.fm/partners)
- [Plans](plans/dice-fm-plans-pricing.yml)
- [Rate Limits](rate-limits/dice-fm-rate-limits.yml)
- [Fin Ops](finops/dice-fm-finops.yml)

## Pricing / Commercial Model

The Ticket Holders API has **no separate published per-call fee**; access is included with a DICE partner agreement. DICE's underlying revenue model is ticketing - a booking fee / commission per ticket sold, surfaced in the API as the `commission` and `diceCommission` fields on tickets and orders. Exact partner terms are negotiated and not publicly reconciled here.

## WebSocket / Real-Time

DICE does **not** publish a documented public WebSocket API. The partner API is request/response GraphQL over HTTPS with no documented subscriptions or `wss://` transport. See `review.yml`.

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
