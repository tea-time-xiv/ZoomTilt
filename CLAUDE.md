# CLAUDE.md

## Identity

All git and GitHub work in this repository is done as the **tea-time-xiv**
account (`Tea Time`), never the default global `Frituur` identity.

Repo-local git config is already set to match:

| Key | Value |
| --- | --- |
| `user.name` | `Tea Time` |
| `user.email` | `tea-time-13371235@proton.me` |
| `user.signingkey` | `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIHFqHd1J/694PW/UAg1ZDORWqJR0H9M6syx8bwzjXbf1` (1Password: "GitHub - TT - Sign") |
| `gpg.format` | `ssh` |
| `commit.gpgsign` | `true` |

Rules:

- Never commit with `--no-gpg-sign`, and never fall back to the global
  identity to work around a signing failure. If the 1Password SSH agent
  refuses to sign, stop and ask the user to unlock 1Password.
- `gh` must run under the `tea-time-xiv` account (`gh auth status` to check).
  If it is authenticated as someone else, stop and ask rather than switching
  accounts unprompted.
- Do not change the repo-local `user.*` settings without being asked.

## Project

ZoomTilt is a Dalamud plugin for FFXIV that adjusts the 3rd person camera
angle based on camera zoom distance. This repo is a fork of
`Tenrys/ZoomTilt`; `origin` is `tea-time-xiv/ZoomTilt` and its default branch
is `master`.

- Plugin source lives in `ZoomTilt/`; entry point is `ZoomTilt/Plugin.cs`.
- Built with `Dalamud.NET.Sdk`; the SDK version in `ZoomTilt/ZoomTilt.csproj`
  determines the Dalamud API level.

## Releases

Publishing is split between two repos and neither holds a credential for the other.

This repo, via `.github/workflows/build.yml` on a push to `master`: reads `<Version>`
from `ZoomTilt/ZoomTilt.csproj`, skips if the tag `v<Version>` already exists, and
otherwise builds `ZoomTilt.zip`, lifts the matching section out of `CHANGELOG.md`, and
publishes a release tagged `v<Version>` with that section as the notes.

`tea-time-xiv/pluginmaster`, via its own scheduled workflow: every 15 minutes it reads
this repo's latest stable release, takes `ZoomTilt.json` **from inside the zip**, lifts
the `## <version>` section out of `CHANGELOG.md` at the release tag, and regenerates
`pluginmaster.json`. Nothing here pushes to it. A new release shows up in game within
15 minutes, or immediately via
`gh workflow run publish.yml -R tea-time-xiv/pluginmaster`.

What follows from that:

- Bumping `<Version>` in the csproj is what triggers a release. Bump it only when a
  release is intended.
- `ZoomTilt/ZoomTilt.json` is the single source of truth for the listing text and
  `DalamudApiLevel`. Editing it is how the installer entry changes; do not look for
  those values in a workflow.
- Every version needs its own `## <version>` section in `CHANGELOG.md` before merge.
  The build fails without one, on pull requests too. That section is what players read
  in `/xlplugins` -> Changelog, and Dalamud renders it as plain text, so keep it to
  short `-` bullets.
- Do not reintroduce a step that writes to the pluginmaster repo from here. That was
  the old pattern; it raced the generator and needed a cross-repo token.
