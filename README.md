<p align="center">
  <img src="public/favicon.png" alt="HatMP icon" height="32" width="32">
</p>

# HatMP

HatMP is an adult video game featuring a gender-swapping protagonist. This game is available for Windows (.exe) and Android
(.apk); its website [hatmp.com](https://hatmp.com) only offers the downloads.

## :warning: :warning: Adult only :warning: :warning:

This game contains explicit scenes and is **prohibited for minors** (18 or 21 years old minimum, depending on the law of
your country of residence).

## :warning: :warning: ATTENTION :warning: :warning:

This version and the original version are still under development.

## Development

### Prerequisites

The project is built with [Tauri v2](https://v2.tauri.app), plus [pnpm](https://pnpm.io/).

- [Tauri v2 prerequisites](https://v2.tauri.app/start/prerequisites/): they vary depending on your machine (Windows,
  Linux or macOS).
- [Android prerequisites](https://v2.tauri.app/start/prerequisites/#android): required only to build the `.apk`.

### Getting started

```sh
pnpm install
pnpm run tauri:dev             # desktop app in development mode
pnpm run dev                   # web version only
pnpm test                      # unit tests
pnpm run build                 # production web build (dist/)
pnpm run tauri:build           # installer / executable for the current platform
pnpm run tauri:android:build   # Android package for the aarch64 target (.apk)
pnpm run tauri:build:linux     # Linux packages (.deb, .AppImage, .rpm)
```

### End-to-end tests (`Cypress`)

The e2e specs live in `cypress/e2e/` (`*.cy.{js,ts}`) and run against the Vite dev server at `http://127.0.0.1:5173`
(override it with `CYPRESS_BASE_URL`). The Cypress binary is downloaded by `pnpm install`; start the dev server first:

```sh
pnpm run dev                 # in a first terminal
pnpm run test:e2e            # headless run
pnpm run test:e2e:open       # interactive runner
```

### Error monitoring (`Sentry`)

The project uses [Sentry](https://sentry.io) to get readable stack traces in production.

- **Dependencies**: `@sentry/browser` and `tauri-plugin-sentry-api` (runtime); `@sentry/vite-plugin` and `@sentry/cli` (build).
- **Source maps**: production builds (`NODE_ENV=production`) generate `hidden` source maps (`build.sourcemap` in
  `vite.config.ts`). The `sentryVitePlugin` uploads them to Sentry during `pnpm run build`, so minified frames are mapped
  back to the original source. The plugin is disabled in development.
- **Authentication**: the upload needs a Sentry auth token. Create one in Sentry (*Settings → Developer Settings → Auth
  Tokens*) and provide it **without committing it**, either through the `SENTRY_AUTH_TOKEN` environment variable or a
  local `.env.sentry-build-plugin` file (already ignored by git):

  ```sh
  # .env.sentry-build-plugin
  SENTRY_AUTH_TOKEN=<your-token>
  ```

Without a token, the build still works but the source maps are not uploaded (a warning is printed).

### Website (`Cloudflare Workers`)

The presentation page of [hatmp.com](https://hatmp.com) lives in `website/` (plain HTML, CSS and JS, no build step). It
only offers the downloads: the game cannot be played on the website.

```sh
pnpm exec wrangler login    # once, to authenticate with Cloudflare
pnpm exec wrangler dev      # local preview (http://localhost:8787)
pnpm exec wrangler deploy   # publish to Cloudflare
```

The download buttons use GitHub's "latest release" links, so every release must attach its files under these exact names:

| Platform | File |
| --- | --- |
| Windows | `HatMP-windows-x64-setup.exe` |
| Android | `HatMP-android-arm64.apk` |

## Game assets

The game assets (`resources/`) are **not stored in git**; they are hosted on Cloudflare R2.

## Translation

There may be some translation errors, so please feel free to let me know by opening an issue. In addition to English, I provide French, German, Italian, Portuguese and Spanish.

## Contributing

Issues and pull requests are welcome.

## Credits and license

The original game was developed by **FF5** ([Patreon](https://www.patreon.com/cw/FF5)). This version is developed with
the original developer's permission.

The original game does not provide any license, so **all rights remain with their respective authors**. No open-source
license is granted for the original content (code, story, artwork). Do not redistribute it without the authors' consent.

Only my code and the logo are licensed, under the [Apache License 2.0](LICENSE) (see also [NOTICE](NOTICE)); each of my
files is marked accordingly.
