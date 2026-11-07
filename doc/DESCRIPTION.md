FlareSolverr is a proxy server that handles Cloudflare protection (JS challenges and
email-protected sites) on behalf of other applications, most commonly the *arr suite
(Prowlarr, Lidarr, Radarr, Sonarr, Bazarr...) and qBittorrent RSS.

It runs a headless Chromium under the hood, so it is heavy-ish but works out of the
box: install it, then point your arr apps' indexer "Proxy" (or FlareSolverr) setting
at `http://127.0.0.1:<port>` — see the admin doc for the exact steps.

This package runs the native Python app with the system Chromium, so it works on
both amd64 and arm64, unlike upstream's bundled release archives.
