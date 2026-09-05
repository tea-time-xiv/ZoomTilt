# Changelog

Each released version gets a section here. The publish pipeline copies the section for the
version being released into the GitHub release notes and into the Tea Time plugin repo, which
is what the in-game changelog (`/xlplugins` -> Changelog) shows. Dalamud renders that text as
plain text, so keep it to short `-` bullets: no tables, no links, no bold.

The version is the `<Version>` property of `ZoomTilt/ZoomTilt.csproj`, released as the tag
`v<Version>`. Releases before `1.0.0.6` predate this file; see the
[releases](https://github.com/tea-time-xiv/ZoomTilt/releases) for those.

## Unreleased

Nothing yet.

## 1.0.0.7

- Fixed a crash that could take down the game when the camera was not available,
  such as at the title screen or while changing zone.
- Fixed the third person camera angle being written from an invalid value while
  zoom is locked, which could leave the setting corrupted.
- Stopped leaving the settings window drawing hooked up after the plugin unloads.

## 1.0.0.6

- Updated for Dalamud API level 15.
- Moved to the Tea Time plugin repository.
