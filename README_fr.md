<h1>
  <img src="logos/flaresolverr.png" width="32px" alt="Logo de FlareSolverr">
  FlareSolverr, packagé pour YunoHost
</h1>

Serveur proxy pour contourner la protection Cloudflare

[![Version : 3.5.2~ynh1](https://img.shields.io/badge/Version-3.5.2~ynh1-128a0b?style=for-the-badge)](https://github.com/YunoHost-Apps/flaresolverr_ynh/blob/master/manifest.toml)

Serveur proxy sans interface web, pensé pour la suite arr (Prowlarr, Lidarr,
Radarr, Sonarr...) : il exécute un Chromium sans interface pour résoudre les
défis JavaScript Cloudflare au nom d'autres applications du serveur. Ce paquet
exécute le Python natif de l'amont avec le Chromium du système (amd64 et
arm64).

Il écoute sur 127.0.0.1 uniquement ; pointez le champ Proxy de vos apps arr
vers `http://127.0.0.1:<port>` — voir doc/ADMIN_fr.md.

## 📦 Informations pour les développeurs

🛠️ Dépôt amont de FlareSolverr : <https://github.com/FlareSolverr/FlareSolverr>

### 📚 Documentation d'empaquetage

Voir <https://doc.yunohost.org/dev/packaging/>.
