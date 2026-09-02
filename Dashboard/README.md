# Dashboard (Homarr)

Zentrale Übersichtsseite mit Links zu allen Homelab-Services, Wetter- und Kalender-Widget.

## Stack
- **Homarr** (nicht Homepage/gethomepage.dev — Verwechslung in früherer Doku-Version korrigiert)
- Image: `ghcr.io/ajnart/homarr:latest`
- Netz: 192.168.1.20 (internes LAN), Port 80

## Setup

Direkt per `docker run` gestartet (kein Compose-File hinterlegt):

```bash
docker run -d \
  --name homarr \
  -p 80:7575 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v homarr_configs:/app/data/configs \
  -v homarr_icons:/app/public/icons \
  ghcr.io/ajnart/homarr:latest
```

Der eingebundene Docker-Socket erlaubt Homarr, laufende Container automatisch zu 
erkennen (Integration-Feature).

## ⚠️ Bekanntes Duplikat

Auf Container 102 läuft zusätzlich ein **`homepage`**-Container (gethomepage.dev, 
anderes Tool). Dieses Dashboard (104, Homarr) ist die aktiv genutzte Instanz. Der 
Homepage-Container auf 102 ist vermutlich ein ungenutztes Überbleibsel aus der 
Setup-Phase — Kandidat für die Aufräumung (siehe [`../immich/README.md`](../immich/README.md)).

## Bekannter Status-Hinweis

`docker ps` zeigt den Container-Status aktuell als `unhealthy` — funktioniert im 
Browser sichtbar einwandfrei, aber wert, den Healthcheck bei Gelegenheit zu prüfen:
```bash
pct exec 104 -- docker logs homarr --tail 50
```

## Nächste Schritte
- [ ] Prüfen, ob `homepage`-Container auf 102 noch gebraucht wird, ggf. entfernen
- [ ] `docker run` in eine `docker-compose.yml` überführen (bessere Reproduzierbarkeit)
- [ ] Healthcheck-Warnung untersuchen
