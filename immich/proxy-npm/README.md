# Nginx Proxy Manager (NPM)

Zentraler Reverse Proxy für alle selbst gehosteten Services — leitet Traffic über 
interne `.home`-Domains und extern (DuckDNS) an die richtigen Container weiter.

## Stack
- Nginx Proxy Manager (Docker, siehe [`docker-compose.yml`](docker-compose.yml))
- Let's Encrypt für automatische SSL-Zertifikate

## Setup

```bash
docker compose up -d
```

Web-Interface (Verwaltungsoberfläche) unter Port `81`, HTTP/HTTPS-Traffic läuft über 
Port `80`/`443`.

## Hinweis

Läuft aktuell zusammen mit Immich auf Container 102 (siehe [`../README.md`](../README.md) 
für den geplanten Aufräum-Fahrplan). Admin-Zugangsdaten liegen in einer separaten, nicht 
versionierten SQLite-Datenbank.
