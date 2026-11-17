# FlareSolverr — packaging design

Headless Cloudflare-bypass proxy driven over HTTP by other apps (the arr suite). The shaping constraint: upstream's release tarball is a pyinstaller bundle with a bundled x86-64 Chrome, so this package runs the native Python source with the system Chromium instead.

Anything not mentioned below is standard YunoHost boilerplate identical in most packages.

## Lifecycle
- **Install:** no questions ([install] empty) → source unpack → `/app/chromedriver` patch + per-instance chromedriver copy → venv → .env render → unit, logrotate, service registration → start, health gate on `FlareSolverr [0-9]` in the log.
- **Upgrade:** stop → apt upgrade chromium/chromium-driver → source full_replace → re-patch, chromedriver copy, venv rebuild → .env re-render → restart. `data_dir` (chromedriver patch cache under `$HOME`) survives untouched.
- **Backup/restore:** `install_dir` + `data_dir` archived; venv and chromedriver copy are regenerated on restore (script reruns install steps).

## Decisions
- **Source:** upstream tag tarball v3.5.2, sha256-pinned, `in_subdir` default. [manifest.toml L37-44](../manifest.toml#L37-L44)
- **Runtime:** system python3 venv (pip install requirements.txt), system chromium/chromium-driver/xvfb from apt. [manifest.toml L58-63](../manifest.toml#L58-L63)
- **Services:** single systemd unit; `EnvironmentFile` from the rendered `.env`. [conf/systemd.service L9](../conf/systemd.service#L9)
- **Storage:** `data_dir` holds undetected_chromedriver's patched-driver cache (`HOME` points there). [conf/systemd.service L10](../conf/systemd.service#L10) [manifest.toml L50-52](../manifest.toml#L50-L52)
- **Auth:** standard — no ldap/sso (`not_relevant`); loopback-only HTTP API, no permission surface.
- **Email:** no mail.
- **Path:** no web surface — bind `127.0.0.1` only, port resource only. [conf/flaresolverr.env L7-8](../conf/flaresolverr.env#L7-L8)
- **Ports/firewall:** loopback only, `main.default = 8191`. [manifest.toml L54-55](../manifest.toml#L54-L55)
- **Scheduler:** standard — none.
- **Migration:** no prior versions in this repo; upgrades from same version tested.
- **Sandboxing:** example_ynh baseline minus `RestrictNamespaces`/@privileged deny — Chromium runs `--no-sandbox` (upstream's own unit does the same). [conf/systemd.service L19-20](../conf/systemd.service#L19-L20)
- **Backup:** both dirs archived, venv regenerated on restore.
- **Admin-surface:** no config panel; live knobs via `/var/www/flaresolverr/.env` + restart, documented in doc/ADMIN.md.

## Don't break
- The `sed` on `src/utils.py` must run after every source (re)install — upstream hardcodes `/app/chromedriver` as its driver-detection Docker-ism; per-instance copy is what keeps multi-instance installs from fighting. [scripts/install L21-22](../scripts/install#L21-L22) [scripts/upgrade L34-35](../scripts/upgrade#L34-L35)
- `HOME=$data_dir` in the unit: undetected_chromedriver writes its patched-driver cache under `$HOME`; losing it means re-patching per start. [conf/systemd.service L10](../conf/systemd.service#L10)
- Health gate greps `FlareSolverr [0-9]` in `/var/log/$app/$app.log` — the unit's `StandardOutput=append:` must keep writing there. [conf/systemd.service L16](../conf/systemd.service#L16) [scripts/install L63](../scripts/install#L63)
- Multi-instance true and tested (CI test 3 green) — keep the per-instance chromedriver copy when touching install/upgrade.
