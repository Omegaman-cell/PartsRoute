# Hackathon demo walkthrough

Run the local production app and worker using [local setup](LIVE-SETUP.md). Use the current local app, not a historical screenshot or an expired tunnel URL.

## Five-minute walkthrough

1. Start in **Sample calculation**. Explain that its prices are synthetic, chosen to demonstrate the purchasing calculation. The unchanged sample has INR 255 in items plus INR 40 shipping, totaling INR 295 through one retailer. Buying each item at its lowest listed price costs INR 225 in items plus INR 120 shipping, totaling INR 345.
2. Change the first quantity from two to three. The old result is marked stale; recalculate to obtain INR 370. Remove and add a component, then reset the sample. Duplicate component selectors are disabled; increase quantity instead.
3. Expand matching details. Exact model and required attributes matter: a DHT11 bare sensor is not the same requested product as a breakout module. A contradictory required specification cannot be approved away.
4. Switch to **Live quote**, enter the private team token and **Check connection**. Start a fresh quote. This creates new collection work. Show the timer, progressive product results and any unresolved checks. If shipping tax is unknown, the combined amount is explicitly an estimate, with known item tax included.
5. Open **Evidence**. Show the original retailer content, current SKU-specific API ordering rules, linked datasheet excerpt, collection time and saved TinyFish browser run ID. Resume a saved quote to demonstrate recovery, clearly identifying it as saved work.

## What the latest real test demonstrates

A fresh three-line collection matched all parts and established prices, item tax and order rules in 5.313 seconds. An exact guest-cart shipping estimate appeared at 7.464 seconds. These are measured live events, not a complete-quote pass. The estimator returned INR 49 shipping but did not disclose shipping tax or a final delivered total. Robu returned timeout errors. Terminal status was partial after 26.320 seconds.

Do not describe INR 248 in items plus the INR 49 shipping estimate as a verified delivered recommendation. The app can display INR 297 as an items + shipping estimate, with shipping tax unconfirmed; the verified final total remains unknown. Further ten-second optimization is deferred for this local delivery.

## Presenter preparation

- Keep the database, web server, worker and network running. Verify readiness immediately before presenting.
- Keep the private team token available locally. Never show provider keys or `.env.local` on the projector.
- Run one fresh collection before the demo to check provider availability; it consumes quota. Saved observations can explain recovery but are not a substitute for fresh timing.
- If a retailer blocks collection, show the truthful partial state and the working sample calculation. Do not call this a successful complete live comparison.

No public hosting, interviews, accuracy study, presentation deck or demo video is claimed. Those require separate work or evidence.
