# fantaseer — public client mirror

**fantaseer** turns a Twitch stream into a fantasy league.
Viewers draft lineups and score when their picks show up in the streamer's actual gameplay, call live pick'ems on what happens next, and climb leaderboards that roll both into one ranking.
It ships as a SvelteKit web app and an embedded Twitch extension — currently brewed around Hearthstone Battlegrounds — with a companion [.NET client and Hearthstone Deck Tracker plugin](https://github.com/Foobasaur/Fantaseer.Client) on the desktop side.

## What this repository is

This is the **public mirror of the Fantaseer client codebase**: the browser-facing side of a private project, published for the explorative code explorer.
It is meant to be read, not deployed — the UI the extension and web app render, the game rules they score with, and the shape of the API they talk to are all here in the open.

A quick map:

```text
src/lib/core/games/HS/   Hearthstone domain: CDN card API, fantasy draft and pickaroo rules, their Svelte UIs
src/lib/core/twitch/     Twitch integration: extension helper service, auth flow, PubSub client
src/lib/svelted/         app glue and the shared UI kit
src/lib/utilz/           small standalone helpers (logging, http, caching, string/number tools)
src/routes/              SvelteKit pages plus the API surface (thin handlers over a withheld server layer)
tests/                   seed and live-populate suites for the data pipeline
compose.yaml             local Postgres for the data tooling
```

## The exclusions list

The mirror is published with a deliberate exclusions list — the serving side of the project stays home.
Withheld today:

- `src/lib/server/` — the layer every `+server.ts` route imports from: the extension backend (EBS) game logic, the Twitch OAuth and Helix proxying, the database schema the tests reference, the server types.
- `.scripts/` — build and release tooling, including the custom Twitch-extension adapter that `svelte.config.js` references.
- Infrastructure and data configs — the SST deployment setup and the Drizzle config and migrations behind the `db:*` scripts.
- Environment files — even the `.env.example` that `.gitignore` optimistically carves an exception for.

The practical consequence is that **this repo does not build or run as-is**, and that is by design: imports into `$lib/server/*` resolve in the private tree, not here.

## A list that can shrink

For now, holding the serving side back is simply the safer, more secure posture — auth flows, backend internals, and infrastructure layout make poor public defaults for a live service.
But the exclusions list is a default, not doctrine.
As the project matures and demand shows up, entries are expected to graduate out of it one by one, shrinking the gap between mirror and source.
If something withheld is exactly the thing you wanted to study, open an issue — expressed interest is the mechanism by which the list shrinks.

## Stack

SvelteKit 2 on Svelte 5 runes, TypeScript, Vite, Tailwind CSS 4 with daisyUI, Vitest, Drizzle ORM over Postgres, SST for deployment, and the Twitch extension platform.

## Around the project

- [Fantaseer.Client](https://github.com/Foobasaur/Fantaseer.Client) — the .NET client and the Hearthstone Deck Tracker plugin, installable from its releases page.
- [Foobasaur on GitHub](https://github.com/Foobasaur) — everything else as it surfaces.

## License

[Apache License 2.0](LICENSE).
