# tizentube-pinned

A pinned, lightly patched build of the [TizenTube](https://github.com/reisxd/TizenTube) userscript
(`@foxreis/tizentube` 1.15.2), for a personal TizenTube Cobalt setup. It is served by jsDelivr
at a fixed commit, so the script an app loads cannot change without the app's URL changing.

Not affiliated with the TizenTube project. All credit to Reis Can and contributors.
Licence: GPL-3.0-only, as upstream (`LICENSE`).

## Source

- Upstream: `reisxd/TizenTube` commit `f99945a84ff767bf14ad8c1146e8e3fd49a3d628` (the commit that
  published npm 1.15.2). An unpatched rebuild of it equals the npm bundle, apart from CRLF line
  endings in one embedded CSS string.
- `patches/0001-premium-defaults.patch`, then `0002-load-marker.patch` (one `console.info` line) `0003-audio-shift.patch` and `0004-night-mode.patch`, the only changes:
  - ad blocking off by default (for YouTube Premium accounts);
  - self-updater off by default, and the only caller of `h5vcc.tizentube.InstallAppFromURL` removed;
  - random device user-agent spoofing removed.
  - `0003`: shifts the audio SourceBuffer timeline by `audioShiftMs` (default -115 ms, sound earlier) to
    compensate a late audio path (HDMI ARC); set it to 0 to disable.
  - `0004`: night mode, a black overlay (strength `nightModeStrength`, default 60 %) from 22:00 to 06:00 in the TV's local time (`enableNightMode`, on by default; setting under TizenTube settings > UI settings).

## Rebuild `u.js`

```sh
git clone https://github.com/reisxd/TizenTube.git && cd TizenTube
git checkout f99945a84ff767bf14ad8c1146e8e3fd49a3d628
git apply /path/to/patches/0001-premium-defaults.patch /path/to/patches/0002-load-marker.patch /path/to/patches/0003-audio-shift.patch /path/to/patches/0004-night-mode.patch
cd mods && npm ci --ignore-scripts && npx rollup -c rollup.config.js
# output: ../dist/userScript.js  (== u.js)
```

`u.js` SHA-256: see `u.js.sha256`.
