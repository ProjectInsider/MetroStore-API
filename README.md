# MetroStore catalog

Catalog for the Windows 8 / Visual Studio 2012 MetroStore client. Distributed as static files through GitHub: no community server, account or subscription is required by the client.

## Inventory and artwork

The catalog contains 518 archived products and six framework families under `dependencies`. Every entry lists its exact downloadable archive, checksum, application-package entries and archive source. Empty files and split language/resource packages are excluded from installation choices. Artwork is extracted from the archived packages: square logos for All Apps, and original splash screens for the home tiles where available. Assets are also bundled with the client for offline browsing.

Framework artwork uses the original VCLibs and PlayReady package logos and the [Microsoft WinJS logo](https://commons.wikimedia.org/wiki/File:WinJS_logo.png) (Microsoft Corporation, public-domain text logo; trademark rights remain with Microsoft). Each dependency includes `imageSourceUrl`. Original small framework icons are kept at their native resolution.

The publisher owns the original application and artwork. Archive availability does not establish installation compatibility, a Store license, a trusted signing certificate or the continued availability of online services. Minimum Windows versions are taken from the inspected primary package when available; architecture variants can have different versions. Unknown prices are left unknown. Verified free listings include a publisher/source link in `priceSourceUrl`.

## Daily Spotlight

The client takes eligible `spotlight` entries and computes unsigned 32-bit FNV-1a over `yyyy-MM-dd|id`, using the UTC date and the UTF-16 character values. It sorts ascending by that score, then ordinal ID, and displays the first five. Optional `spotlightStart` and `spotlightEnd` constrain eligibility. The same catalog and UTC date produce the same selection on every device. No scheduled server process is necessary. Offline clients use their saved catalog.

## Collections and privacy

Favorites, ratings, viewing history and notes remain on the device. Best Rated uses those private ratings. Picks for You uses local category interests. New Releases lists recent catalog additions, not claims of newly released archived software. Top Free includes only documented free applications; it does not imply download popularity. Top Rising requires sourced published trend statistics; the current catalog contains no invented download or review numbers.

## Updating the catalog

Keep `store.catalogVersion` at 3 or later. Add a stable unique ID, a checked archive URL, `sizeBytes`, `sha1`, and nonempty application entries under `packages` with their exact names, architecture and size. Do not include zero-byte files, dependencies or split resource packages as installable apps. Set `packageName` to an actual application entry. Add the matching logo under `Assets/Catalog` and optional splash image under `Assets/Splash`; use paths beginning with `/Assets/` in JSON. The client uses packaged artwork first and falls back to these GitHub files for newer entries.

The SHA-1 checksum identifies the archived file; it is not a security certificate. The client checks the archive, prepares the APPX, and asks Windows to open it. Windows performs installation separately.
