# radiospiral-config
Configuration repository for the RadioSpiral streaming radio app

This repository contains the web-based config files for the RadioSpiral
streaming radio application, for RadioSpiral, based at https://radiospiral.net.
The app fetches these files from GitHub at runtime, so changes pushed to `master`
reach existing installs without an app release.

The config files here are
 - `config.json`: defines the configurations that the app should use to derive its configuration.
   Subsumes the deprecated stations.json and adds a new Azuracast-specific config section that
   the app can use to probe an Azuracast server for active streams, plus a list of streams that
   can be active but should be excluded from the app.
 - `stations.json`: (**deprecated**) a file defining the station's main streaming URL, name and short
   description. Should RadioSpiral host other streams at a later date, this file
   could be updated to push out the new streams in real time.
 - `credits.json`: defines the station staff's roles and names. Used in the iOS app's credits
   screen.
 - `logo.png`: the station logo referenced by `imageURL`. It must have the same dimensions as
   the app's built-in logo resources.

## `config.json`

The top level is an object with a single key, `configs`, holding an array of
configuration sources. Each source has a `format` key that says what kind of source
it is. Sources are listed in priority order: the first entry is the primary source
and later entries are fallbacks.

```json
{
  "configs": [
    { "format": "azuracast", ... },
    { "format": "static", ... }
  ]
}
```

### `"format": "azuracast"`

Discovers the active streams by querying the public API of an Azuracast server.

| Key       | Type             | Description |
|-----------|------------------|-------------|
| `format`  | string           | Always `"azuracast"`. |
| `server`  | string           | Hostname of the Azuracast server to query, without a scheme (e.g. `"spiral.radio"`). |
| `exclude` | array of strings | Short codes of stations that may be active on the server but should not appear in the app (e.g. `["rstest"]` for the test stream). Use `[]` to exclude nothing. |

### `"format": "static"`

A fixed list of stations, used when the Azuracast server can't be reached.

| Key        | Type             | Description |
|------------|------------------|-------------|
| `format`   | string           | Always `"static"`. |
| `stations` | array of objects | The station definitions, each with the fields described below. |

#### Station fields

These are the same fields used by the entries in the deprecated `stations.json`.

| Key          | Type   | Description |
|--------------|--------|-------------|
| `name`       | string | Station display name (e.g. `"RadioSpiral"`). |
| `streamURL`  | string | Full URL of the audio stream (e.g. `"https://spiral.radio:8000/stream.mp3"`). |
| `imageURL`   | string | URL of the station logo. Normally the raw GitHub URL of `logo.png` in this repo. |
| `desc`       | string | Short one-line description (e.g. `"captivating electronica"`). |
| `longDesc`   | string | Longer description. Use `\n` for line breaks. |
| `serverName` | string | Hostname of the streaming server (e.g. `"spiral.radio"`). |
| `shortCode`  | string | The station's Azuracast short code, the same identifier used in its Azuracast URLs and in the `exclude` list (e.g. `"radiospiral"`). |
| `defaultDJ`  | string | Name shown as the DJ when no live DJ is on air (e.g. `"Spud the Ambient Robot"`). |

## `stations.json` (deprecated)

Kept for app versions that predate `config.json`; don't remove it while those versions
are still in use. The top-level key is `station`, an array of station objects with the
same fields as the [station fields](#station-fields) above.

```json
{
  "station": [
    { "name": "RadioSpiral", "streamURL": "...", ... }
  ]
}
```

## `credits.json`

Defines the staff credits shown on the app's About screen. Entries are displayed in the
order listed.

```json
{
  "credits": [
    {"role": "Curator & founder", "name": "Mike Metlay (Mr. Spiral)"},
    ...
  ]
}
```

| Key      | Type   | Description |
|----------|--------|-------------|
| `role`   | string | What the person does for the station. |
| `name`   | string | The person's name, optionally followed by their handle in parentheses. |

Each entry must be an object with `role` and `name` keys, matching the app's Swift struct;
the earlier `["role", "name"]` array form is not supported. Credits live in their own file
because the app's `config.json` parser can't handle a credits section there.
