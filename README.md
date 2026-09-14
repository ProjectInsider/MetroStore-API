# MetroStore catalog

Catalog for the Windows 8 / Visual Studio 2012 MetroStore client. Distributed as static files through GitHub: no community server, account or subscription is required by the client.

## Inventory and artwork

The catalog contains 926 archived products and six framework families under `dependencies`. Every entry lists its exact downloadable archive, checksum, application-package entries and archive source. Empty files and split language/resource packages are excluded from installation choices. Artwork is extracted from the archived packages: square logos for All Apps, and original splash screens for the home tiles where available. Assets are also bundled with the client for offline browsing.

Framework artwork uses the original VCLibs and PlayReady package logos and the [Microsoft WinJS logo](https://commons.wikimedia.org/wiki/File:WinJS_logo.png) (Microsoft Corporation, public-domain text logo; trademark rights remain with Microsoft). Each dependency includes `imageSourceUrl`. Original small framework icons are kept at their native resolution.

The publisher owns the original application and artwork. Archive availability does not establish installation compatibility, a Store license, a trusted signing certificate or the continued availability of online services. Minimum Windows versions are taken from the inspected primary package when available; architecture variants can have different versions. Unknown prices are left unknown. Verified free listings include a publisher/source link in `priceSourceUrl`.

## Featured

The updated home page begins with one large Featured tile. It considers downloadable Spotlight-eligible entries plus Fresh Paint, respecting any availability dates. It sorts by ordinal ID and advances one entry per UTC day, anchored to Fresh Paint on 2026-09-13. The same updated client, catalog and UTC date give the same app; offline snapshots may differ. Each home tile has an opaque caption band in its app color.

## Daily Spotlight

The updated client builds a stable ordering per group by sorting eligible entries on unsigned 32-bit FNV-1a of `2000-01-01|group|id` (UTF-16 character values), followed by ordinal ID. It computes the number of UTC days since 2000-01-01 and rotates that ordering by `days * 5 mod count` for pools larger than five, or `days mod count` for smaller pools. The first five entries become the home tiles. For pools of at least ten, consecutive days have no repeated featured entries. Smaller pools necessarily reuse some apps.

Spotlight eligibility uses `spotlight`, a downloadable package and optional `spotlightStart`/`spotlightEnd`. Category groups only use their own category; New Releases uses recent catalog additions and Small downloads uses archives below 10 MiB. Dependencies rotate within their separate pool too. The same updated client, catalog and UTC date produce the same selection on every device. The client checks the date every minute while the home page is active; no scheduled server process is necessary. Offline clients use their saved catalog, so differing catalog snapshots may show different apps.

At startup the client prioritizes home artwork and warms remaining catalog logos in the background. Decoded images are shared with All Apps, transfers are limited to four at once, and retained decoded artwork has a 96 MiB budget. Startup does not wait for the full catalog to decode.

## Collections and privacy

Favorites, ratings, viewing history and notes remain on the device. Best Rated uses those private ratings. Picks for You uses local category interests. New Releases lists recent catalog additions, not claims of newly released archived software. Top Free includes only documented free applications; it does not imply download popularity. Top Rising requires sourced published trend statistics; the current catalog contains no invented download or review numbers.

## Updating the catalog

Keep `store.catalogVersion` at 3 or later and `store.catalogRevision` at 7 or later. The updated client rejects older snapshots so a stale cache cannot restore the old colors or hide the bundled additions. Add a stable unique ID, a checked archive URL, `sizeBytes`, `sha1`, and nonempty application entries under `packages` with their exact names, architecture and size. Do not include zero-byte files, dependencies or split resource packages as installable apps. Set `packageName` to an actual application entry. Add the matching logo under `Assets/Catalog` and optional splash image under `Assets/Splash`; use paths beginning with `/Assets/` in JSON. The client uses packaged artwork first and falls back to these GitHub files for newer entries.

The SHA-1 checksum identifies the archived file; it is not a security certificate. The client checks the archive, prepares the APPX, and asks Windows to open it. Windows performs installation separately.
