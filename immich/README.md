# Immich (Foto-/Video-Verwaltung)

Selbst gehostete Alternative zu Google Photos — mit Gesichtserkennung, KI-Suche und 
automatischer Sicherung von Fotos/Videos.

## Stack

- **Server**: `immich-server` (Docker)
- **Machine Learning**: `immich-machine-learning` — Gesichtserkennung, Objekterkennung, KI-Suche
- **Datenbank**: PostgreSQL mit Vector-Extension (`vectorchord`, `pgvectors`) für KI-basierte Bildsuche
- **Cache**: Valkey (Redis-kompatibel)

Setup folgt der offiziellen Immich-Anleitung: https://docs.immich.app/install/docker-compose

## Setup

1. `.env` aus [`.env.example`](.env.example) erstellen und mit echten Werten befüllen
2. Container starten:
```bash
   docker compose up -d
```

## Installation (Grundschritte)

Offizielle Installationsmethode via Docker Compose (Debian-Host mit installiertem Docker):

```bash
# Projektordner anlegen
mkdir -p /root/immich && cd /root/immich

# Offizielle docker-compose.yml herunterladen
wget https://github.com/immich-app/immich/releases/latest/download/docker-compose.yml

# Beispiel-.env herunterladen und anpassen
wget -O .env https://github.com/immich-app/immich/releases/latest/download/example.env
nano .env
# → UPLOAD_LOCATION, DB_PASSWORD, DB_USERNAME, DB_DATABASE_NAME eintragen

# Container starten
docker compose up -d

# Status prüfen
docker compose ps
```

Web-Interface danach erreichbar unter `http://<SERVER-IP>:2283` — Ersteinrichtung 
(Admin-Account) erfolgt im Browser beim ersten Aufruf.

## Migration von Google Photos

1. Export aus Google Takeout
2. Sortierung nach Datum/Metadaten mit [`immich-go`](https://github.com/simulot/immich-go):
```bash
   immich-go upload from-google-photos --server=http://<SERVER-IP>:2283 \
       --api-key=<DEIN-API-KEY> /pfad/zu/takeout-export
```
3. Fotos erscheinen automatisch in der Immich-Bibliothek


3. Web-Interface unter `http://<SERVER-IP>:2283` — Ersteinrichtung (Admin-Account) im Browser

## Workflow: Migration von Google Photos

1. Export aus Google Takeout
2. Sortierung nach Datum/Metadaten mit [`immich-go`](https://github.com/simulot/immich-go)
3. Import in Immich über `immich-go upload`

## ⚠️ Aktueller Ist-Zustand: Geteilter Container (102)

Container 102 läuft aktuell **nicht nur** Immich, sondern ist historisch gewachsen und 
enthält zusätzlich mehrere weitere, thematisch getrennte Services:

- **Nginx Proxy Manager** (Reverse Proxy für alle Services) — siehe [`proxy-npm/`](proxy-npm/)
- **Coturn + Signaling-Server** (für Nextcloud Talk)
- **Homepage/Dashboard**
- **Zwei eigenständige Business-Projekte** (Übersetzungs-Tool, Business-Webseite) mit 
  eigenen Backends, Webseiten und Cloudflare-Tunneln

Das ist bewusst dokumentiert, um den Ist-Zustand ehrlich zu zeigen — nicht der 
Ziel-Zustand.

## 🎯 Geplante Aufräumung (Roadmap)

- [ ] **Schritt 1**: Nginx Proxy Manager in eigenen LXC-Container auslagern (zuerst, da 
      zentral für Erreichbarkeit aller Services)
- [ ] **Schritt 2**: Coturn/Signaling zu Nextcloud (Container 101) verschieben — gehört 
      funktional zu Nextcloud Talk
- [ ] **Schritt 3**: Business-Projekte (Übersetzung, Business-Webseite) in eigenen 
      Container trennen
- [ ] **Schritt 4**: Homepage/Dashboard konsolidieren (Container 104 ist dafür bereits vorgesehen)
- [ ] **Ziel**: Container 102 enthält am Ende nur noch den reinen Immich-Stack

## Wichtige Hinweise

- Die echte `.env`-Datei mit Datenbank-Passwort wird **nicht** in diesem Repository geführt
- Nginx Proxy Manager Admin-Zugangsdaten liegen in einer separaten, nicht versionierten 
  SQLite-Datenbank innerhalb des Containers
