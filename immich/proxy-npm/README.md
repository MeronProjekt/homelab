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

## Installation (Grundschritte)

```bash
# Projektordner anlegen
mkdir -p /root/npm && cd /root/npm

# docker-compose.yml anlegen (siehe Datei in diesem Ordner)
nano docker-compose.yml

# Container starten
docker compose up -d
```

Admin-Interface danach erreichbar unter `http://<SERVER-IP>:81` — Standard-Login beim 
ersten Start: `admin@example.com` / `changeme` (sofort nach erstem Login ändern!).

Danach im Interface: "Proxy Hosts" → "Add Proxy Host" für jeden Service, den du über 
NPM erreichbar machen willst (z.B. Nextcloud, Immich, Dashboard).




Web-Interface (Verwaltungsoberfläche) unter Port `81`, HTTP/HTTPS-Traffic läuft über 
Port `80`/`443`.

## Hinweis

Läuft aktuell zusammen mit Immich auf Container 102 (siehe [`../README.md`](../README.md) 
für den geplanten Aufräum-Fahrplan). Admin-Zugangsdaten liegen in einer separaten, nicht 
versionierten SQLite-Datenbank.
