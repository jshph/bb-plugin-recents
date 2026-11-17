> **Deprecated — development moved.** This repository has been replaced by [jshph/bb-extensions](https://github.com/jshph/bb-extensions), with the maintained plugin in [`plugins/recents`](https://github.com/jshph/bb-extensions/tree/main/plugins/recents). Use the new repository for updates and issues. This repository remains available for history, existing install URLs, and outstanding pull-request review. It has not been archived.

# Recents for BB

Recent threads on the new-thread page, with nested threads and live activity.

Recents is independent of Tandem. It follows the same one-plugin-per-repository
convention as `jshph/bb-plugin-live-voice`, with its own install, version and
release tags. BB also supports collections through `.bb/plugins.json`; a
collection is useful when several plugins intentionally share a repository.
Voice and homepage navigation have separate purposes, so these stay separate.

## Install

Requires a BB build with Plugin SDK **0.6.147** or later. The initial release
accompanies the homepage-section API changes in the `jshph/bb` fork; an older
BB build cannot install it. Those core changes must be applied first.

Install from the local plugin checkout:

```sh
bb plugin install .
```

Install from the public repository with
`bb plugin install https://github.com/jshph/bb-plugin-recents`.

The plugin renders below the composer on desktop and in the scroll area above
the composer on compact screens. Disabling it removes Recents entirely.

- Up to 15 visible rows ordered by attention, then creation time and ID.
- Children stay under their parent, including collapsed status and draft rollups.
- Provider icons, project/environment metadata, title mentions and thread menus
  use BB's public SDK.
- Creating a thread shows a pending row; the last successful creation is
  highlighted and its ancestors expand when the composer stays open.
- Navigating to a thread or receiving new unread activity reveals its ancestors.
- Collapse choices remain local to the browser. The original core storage key,
  `bb.mobileRecents.collapsedThreads`, is retained so existing choices survive.
- The original `data-root-compose-mobile-recents` and `data-overflow-fade`
  styling hooks remain available to themes such as Aura.

## CLI and SDK

```sh
bb recents list
bb recents list --json
```

The command prints at most 15 rows. It uses the same hierarchy and ordering as
the homepage, with every parent expanded because browser collapse state and
unsent drafts belong to that client. Thread data is available to plugins through
`bb.sdk.projects.sidebarBootstrap()` and `experimental_useSidebarThreads()`.
Thread actions use BB's existing thread commands and SDK methods.

## Development

```sh
npm ci
npm run typecheck
npm test
bb plugin build .
```

Development pins the published SDK 0.6.146. `src/homepage-contract.d.ts`
adds the two provisional homepage fields while the companion 0.6.147 contract
is awaiting release. It only augments declarations; runtime installation still
requires a host at 0.6.147 or later. Remove the augmentation and update the
development pin once the new SDK is published.

The plugin imports only public `@get-bb/plugin-sdk` entrypoints, React, Jotai
and clsx. It requires no private BB packages or filesystem paths. Source and
plugin assets ship in this repository; generated `dist/` is ignored.
