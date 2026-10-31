# Releases

This repository publishes the Waev web companion's releases. Waev's source
lives in a private repository. Each release is built there and published
here, with the exact source commit in its notes.

## What a release holds

| Asset | What it is |
|---|---|
| `waev_companion-<version>-py3-none-any.whl` | The plugin. It carries the launcher, the web UI and the engine for each supported processor (linux-arm, linux-arm64, linux-x64), compressed. The launcher unpacks the one your repeater needs. |
| `SHA256SUMS` | SHA-256 checksums of the other assets. |
| `THIRD_PARTY_NOTICES.md` | The licences of the third-party components the plugin bundles, with a source pointer for each that needs one. |
| `RELEASE_NOTES.md` | The release's notes, also its description on GitHub: the source commit, required fixes, engine sizes and the checks the release passed. |

## Version numbers

A version is the Waev app's major and minor version, then 10000 plus the
number of commits in the source history at the release's commit. `1.0.12546`
is Waev 1.0 at a commit with 2,546 commits behind it. A later commit always
gets a greater version, so the repeater's plugin manager orders releases
correctly.

The tag is `v<version>`. It sits on this repository's own default branch; the
source commit is named in the notes.

## How a release is built

A release comes from Waev's release lane on the maintainers' machine. No
hosted CI is involved. The lane:

- refuses an uncommitted or dirty checkout;
- builds from a fresh copy of the source at the release commit;
- compiles the engine for each processor and checks that each stays under
  16 MiB;
- runs the engine against a stand-in companion and checks that attaching
  never changes the companion's flood scope;
- checks the web UI's use of browser features that need a secure context;
- checks that every bundled component's licence is listed, with its source;
- builds in a fixed folder, so a rebuild of the same commit can be compared
  with the release byte for byte;
- re-verifies the release before publishing it, and publishes each version
  once.

## Check a download

Download the wheel and `SHA256SUMS` into one folder, then:

```sh
sha256sum -c --ignore-missing SHA256SUMS          # Linux
shasum -a 256 -c --ignore-missing SHA256SUMS      # macOS
```

The line for the wheel must end in `OK`.

## A release, once published

A release's assets are never replaced. A fix ships as a new version.

## The openHop plugin catalogue

The plugin is not listed in the openHop plugin catalogue yet. A listing would
name one approved version with its wheel URL and SHA-256, and each update
would go through the catalogue's review before repeaters offer it.
