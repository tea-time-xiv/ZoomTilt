# CLAUDE.md

## Identity

All git and GitHub work in this repository is done as the **tea-time-xiv**
account (`Tea Time`), never the default global `Frituur` identity.

Repo-local git config is already set to match:

| Key | Value |
| --- | --- |
| `user.name` | `Tea Time` |
| `user.email` | `280381899+tea-time-xiv@users.noreply.github.com` |
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

`.github/workflows/build.yml` releases automatically on every push to
`master`: it reads `<Version>` from `ZoomTilt/ZoomTilt.csproj`, skips if the
tag `v<Version>` already exists, and otherwise builds `ZoomTilt.zip`, creates
the release, and pushes an updated entry to `tea-time-xiv/pluginmaster`.

- Bumping `<Version>` in the csproj is what triggers a release. Bump it only
  when a release is intended.
- The `DalamudApiLevel` written to the plugin repo is hardcoded in the
  workflow's `Update pluginmaster` step; update it alongside an SDK bump.
- The root `pluginmaster.json` is a leftover from upstream and is no longer
  read by anything. Do not bother updating it.

Record every user-visible change under `## [Unreleased]` in `CHANGELOG.md`,
then move those entries into a versioned section when cutting a release.
