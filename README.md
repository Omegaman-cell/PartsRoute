# PartsRoute

**Every part. One complete cost.**

Current demo repository: [Omegaman-cell/PartsRoute](https://github.com/Omegaman-cell/PartsRoute). Start with the [local video-demo handoff](docs/LOCAL-DEMO.md) to run the full app on another computer.

Compare a small electronics bill of materials across Robu and Robocraze. The calculator accounts for pack sizes, published ordering rules, price breaks, stock, item tax and one shipping charge per retailer basket. Exact product requirements must agree before an offer is eligible.

## Current local version

The application has a Next.js frontend and API, a PostgreSQL job queue, and a separate Node worker. The supported catalogue contains HC-SR04 sensors, DHT11 breakout modules and 400-point breadboards. Use one to three distinct components and change their quantities; arbitrary product discovery and BOM import are outside this version.

The sample is fully functional and explicitly synthetic. Live collection works with privately configured providers. On 22 October 2026, a new live three-line quote displayed complete product coverage in **5.313 seconds** and a matched-cart shipping estimate in **7.464 seconds**. It did **not** produce a verified delivered total within ten seconds. Robu timed out and Robocraze's estimator did not disclose shipping tax. The ten-second goal is now an optimization target rather than a release blocker. See [measured performance](docs/PERFORMANCE.md).

## Run locally

Use Node 22.12+ and the pinned pnpm version from package.json.

```sh
pnpm install --frozen-lockfile
pnpm local:setup
```

Fill TinyFish and Gemini keys in the generated, ignored `.env.local`. The setup creates a private team access token. Point `DATABASE_URL` at an existing PostgreSQL database, or initialize an installed PostgreSQL runtime:

```sh
pnpm local:setup --postgres-bin "C:/path/to/PostgreSQL/bin"
pnpm local:start
```

Open http://127.0.0.1:3000. The launcher starts the configured local database, applies migrations, builds production assets and starts web and worker. Keep its terminal open. It refuses to duplicate an existing PartsRoute web service. See [local setup and troubleshooting](docs/LIVE-SETUP.md) for separate processes and Docker.

## Use the app

- **Sample calculation**: calculate synthetic baskets without provider keys or a database; add/remove supported components, edit requirements, compare alternatives and inspect matching details.
- **Live quote**: enter the private team token, check connection, then collect fresh data. API keys stay on the server.
- **View saved observations**: a separate read-only comparison; no new provider calls. Source times and missing observations remain visible.
- **Evidence**: original collected content, normalized fields, linked datasheet excerpts, provider usage and recoverable browser run IDs.

A delivered recommendation requires an exact basket, matching quantities, known taxes, destination serviceability and a verified complete total. Unknown shipping is never treated as free. Partial results are useful evidence, not a verified recommendation.

When all item costs and shipping charges are known, an **items + shipping estimate** is displayed separately, with unconfirmed shipping tax explicitly labelled. It does not change verification, rankings or claimed savings.

Fresh collection defaults to six new jobs per team token per rolling hour. Set `LIVE_QUOTES_PER_HOUR=0` in the private local environment to disable this hourly limit for testing. The queue still permits at most three active jobs. Sample calculations and saved observations do not consume hourly slots.

## Collection design

TinyFish Fetch retrieves product pages and policies in two concurrent retailer batches. Each URL has a bounded timeout and its errors remain distinct. The original responses are stored before extraction. Narrow deterministic parsers read known templates; Gemini performs schema-validated extraction when source facts are insufficient. There is no automatic paid-model fallback.

For Robocraze, the documented Shopify Ajax product API supplies current SKU, price and explicit quantity rules. These must agree with the listing. A retailer-linked PDF datasheet can supply explicit physical-header and operating-voltage specifications. No prices, variant IDs, pin counts or shipping rates are hardcoded in the live adapters.

A new anonymous guest cart checks exact SKUs, quantities and line totals before requesting pincode-specific shipping estimates. It never clears existing contents, submits checkout, enters personal details, pays or places an order. Cart cookies/tokens are excluded from saved API evidence. An estimator without tax evidence stays estimated. TinyFish Agent can verify shortlisted baskets concurrently with slower retailer collection. Ambiguous run creation is not retried; saved run IDs support recovery.

The worker uses expiring leases with attempt fencing. Lost ownership cancels in-flight local requests and rejects stale result/evidence writes. The collection window is bounded; expiry preserves partial facts and pending remote run IDs. A timed-out HTTP request does not prove that a remote operation stopped.

## Verify

```sh
pnpm test
pnpm typecheck
pnpm test:postgres       # dedicated TEST_DATABASE_URL required
pnpm build
pnpm build:pages
pnpm test:http          # production web server running
pnpm doctor
pnpm doctor --providers # opt-in real provider quota
pnpm benchmark:live-budget --follow # opt-in new live job; strict 10s verdict
```

[Validation](docs/VALIDATION.md) distinguishes mocked tests, real PostgreSQL checks, browser observations and live timing. The CI workflow runs unit/type/database/build/HTTP checks without provider credentials. Live calls remain explicit.

## Hackathon demo

Use the [demo walkthrough](docs/DEMO-GUIDE.md). Demonstrate the sample arithmetic first, then the real collection and its evidence. The latest version runs locally; no paid hosting was provisioned. A static Pages build cannot run the database or worker. Historical screenshots and tunnel addresses in older research documents are not current hosting guarantees.

Cloud deployment is deferred in favor of recording the complete local app. The [hosting notes](docs/HOSTING.md) retain the full-stack deployment plan for later. No reduced-functionality Sites version was published.

**Live limitations:** complete shipping-tax evidence and reliable collection from both retailers remain dependent on retailer access. A full fresh verified quote under ten seconds has not been achieved; that optimization is deferred for this local delivery. Paid hosting, arbitrary search, interviews and a demo video are not claimed.

Built by **Team REsynced**, NSUT, for Kritical Hacks. Original repository: [PartsRoute](https://github.com/Daksh909-design/PartsRoute---Every-Part-One-Complete-Cost.).
