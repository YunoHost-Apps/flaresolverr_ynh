FlareSolverr est un serveur proxy qui contourne la protection Cloudflare
(défis JavaScript et sites protégés par e-mail) pour le compte d'autres
applications, le plus souvent la suite *arr (Prowlarr, Lidarr, Radarr,
Sonarr, Bazarr...) et le RSS de qBittorrent.

Il utilise un Chromium sans interface en arrière-plan : installez-le puis
pointez le champ « Proxy » (ou FlareSolverr) des indexeurs de vos apps arr
vers `http://127.0.0.1:<port>` — voir la doc admin.

Ce paquet exécute l'application Python native avec le Chromium du système,
il fonctionne donc sur amd64 et arm64, contrairement aux archives binaires
fournies par l'amont.
