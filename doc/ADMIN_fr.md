## Utilisation avec la suite arr

FlareSolverr écoute uniquement sur `127.0.0.1` — il est joignable par les apps
du même serveur, pas de l'extérieur. Récupérez le port :

```bash
sudo yunohost app setting flaresolverr port
```

Puis dans Lidarr/Prowlarr/Radarr/Sonarr, champ Proxy (Indexers) :
`http://127.0.0.1:<port>`

Test : `curl http://127.0.0.1:<port>/health`
