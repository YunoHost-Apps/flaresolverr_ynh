## Using it with the arr suite

FlareSolverr listens on `127.0.0.1` only — it is reachable from apps on the
same server, not from outside. Get the port with:

```bash
sudo yunohost app setting flaresolverr port
```

Then in Lidarr/Prowlarr/Radarr/Sonarr:

- **Prowlarr**: Settings → Indexers → add/edit an indexer → Proxy →
  `http://127.0.0.1:<port>` (or FlareSolverr field on older versions)
- **Other arrs**: Settings → Indexers → "Proxy" field → same URL

Test it with: `curl http://127.0.0.1:<port>/health`

Logs: `sudo yunohost log list` / `sudo journalctl -u flaresolverr`
(or `sudo tail -f /var/log/flaresolverr/flaresolverr.log`).

Config (log level, captcha solver, timeout): edit
`/var/www/flaresolverr/.env` then `sudo systemctl restart flaresolverr`.
A `yunohost app upgrade` regenerates that file.
