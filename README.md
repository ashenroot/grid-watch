# grid-watch

A single-file dashboard for US bulk-power-grid situational awareness: hourly
demand and day-ahead forecast error, generation mix, interchange, and
deviation flags for every balancing authority in the Lower 48 — plus a
deep-dive tab for whichever state you configure, joining its generating
fleet, active weather alerts, reported customer outages, and out-of-service
generators into one view.

It is one HTML file. Open it from disk, serve it from anywhere, or host it
behind a URL — there is no build step, no framework and no server component
in the page itself. Most of it talks straight to the EIA Open Data API, the
National Weather Service, and ORNL's outage portal; the handful of feeds a
browser cannot reach directly come from a small relay you run yourself.

```
grid-watch.html      the whole dashboard
```

## Getting it running

1. **Open `grid-watch.html`.** Nothing is pre-configured — a fresh copy has
   no state, no EIA key, and no relay set.
2. **Setup tab.** Enter a free EIA API key (get one at
   [eia.gov/opendata](https://www.eia.gov/opendata/) — read-only,
   rate-limited, over public data) and pick the state you want the state
   tab and state outage tab to cover. That's enough for the national view,
   the state view, and the outage tabs.
3. **Stand up the relay**, if you also want the KEV catalog, the DOE 202(c)
   register, NERC advisories, or the briefing-digest comparison. See
   [watch-relay](https://github.com/ashenroot/watch-relay) — it is the same
   relay [comms-watch](https://github.com/ashenroot/comms-watch) uses, so
   one host serves both dashboards.

## Configuration, and why it is split in three

The Setup tab writes to three separate browser storage keys:

```
grid-watch/state/v1          which state the state tab and outage tab cover
grid-watch/eia-key/v1        your EIA API key
grid-watch/relay/v1          relay base URL
grid-watch/relay-token/v1    relay digest-write token
```

The key is split from the state for the same reason the relay token is
split from the relay URL: the state is something you'd happily paste into a
bug report, the key and the token are not. Nothing here is sent anywhere
except to the services the page itself names — the EIA key goes only to
`api.eia.gov`, and the relay token only to the relay you configured.

## About the state tab

Every state's hourly layer is shown through whichever balancing authority
carries most of its generating capacity — its own, if it has one, or
whichever multi-state BA does, in which case the tab says so and gives that
state's rough share. State-exact figures (the generating fleet, monthly
generation by fuel, retail sales) come from EIA's monthly and inventory
series instead, at a slower cadence than the hourly layer.

The state map plots every generator at its EIA-reported coordinates for any
state you choose. The drawn state *outline* behind those markers, however,
is bundled for Georgia only — for any other state the map view is fit to
the plants themselves rather than drawing a boundary this project does not
actually have. That is a real, stated limitation, not a bug.

## Keys

- **EIA Open Data API** — required for everything except the relay-backed
  feeds. Free, read-only, rate-limited. Get one at
  [eia.gov/opendata](https://www.eia.gov/opendata/).
- Nothing else is required to get started. The relay's own feeds (KEV, DOE
  202(c), NERC, briefing digests) are documented on the Setup tab.

If you fork this and publish it, check that you have not committed a key.
The `.gitignore` excludes exported config files for that reason — but the
EIA key itself was never in this file to begin with; it lives only in your
browser's storage, entered once on the Setup tab.

## Licence

MIT. See [LICENSE](LICENSE).
