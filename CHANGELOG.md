# Changelog

All notable changes to ZoomTilt are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versions are the four-part `Major.Minor.Build.Revision` value of the `<Version>`
property in `ZoomTilt/ZoomTilt.csproj`, released as the git tag `v<Version>`.

Releases up to and including `v1.0.0.6` predate this file and are not
documented here; see the
[commit log](https://github.com/tea-time-xiv/ZoomTilt/commits/master) and
[releases](https://github.com/tea-time-xiv/ZoomTilt/releases) for those.

## [Unreleased]

Nothing yet.

## Cutting a release

Releases are automated by `.github/workflows/build.yml`. On a push to `master`
it reads `<Version>` from `ZoomTilt/ZoomTilt.csproj` and, if the tag
`v<Version>` does not already exist, builds `ZoomTilt.zip`, creates the release
under that tag, and pushes an updated entry to the
[tea-time-xiv/pluginmaster](https://github.com/tea-time-xiv/pluginmaster) repo.

So, to release:

1. Move the `Unreleased` entries into a new `## [x.y.z.w] - YYYY-MM-DD` section.
2. Bump `<Version>` in `ZoomTilt/ZoomTilt.csproj`. If the Dalamud API level
   changed, also update `DalamudApiLevel` in the workflow's
   `Update pluginmaster` step.
3. Merge to `master`. CI tags, releases, and updates the plugin repo.

Leaving `<Version>` unchanged is safe: CI sees the existing tag and skips the
release, so a docs-only merge does not publish anything.

Note: the `pluginmaster.json` at the repo root is a leftover from the upstream
`Tenrys/ZoomTilt` repo and is no longer what the plugin installer reads.

## Categories

Use these headings under each version, omitting any that are empty:

- **Added** — new features.
- **Changed** — changes to existing behaviour.
- **Deprecated** — soon-to-be-removed features.
- **Removed** — removed features.
- **Fixed** — bug fixes.
- **Security** — vulnerability fixes.
