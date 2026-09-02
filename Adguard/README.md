# AdGuard Home

DNS-basierte Filterung (Werbung, Tracker, NSFW-Inhalte) für alle Geräte im Haushalt.

## Stack
- AdGuard Home — **direkt installiert** (kein Docker), läuft als systemd-Service
- Binary-Pfad: `/opt/AdGuardHome/AdGuardHome`
- Netz: 192.168.178.105 (Fritzbox-Netz) — bewusst dort, damit alle Geräte im Haushalt 
  es direkt als DNS nutzen können

## Setup

Offizielles Install-Skript von AdGuard Home:

```bash
curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -v
```

Das installiert AdGuard Home als systemd-Service unter `/opt/AdGuardHome/`.

## Verwaltung

```bash
systemctl status AdGuardHome    # Status prüfen
systemctl restart AdGuardHome   # Neu starten
```

Erst-Konfiguration (Admin-Zugang, DNS-Filterlisten) erfolgt über das Web-Interface 
auf Port 80/3000 beim ersten Start.

## Wichtiger Hinweis
Die Konfigurationsdatei `AdGuardHome.yaml` (enthält Admin-Passwort-Hash und 
Filterregeln) wird bewusst **nicht** in diesem Repository geführt.

## Verwaltung (Updates)
Wird über Ansible automatisiert verwaltet, siehe [`../ansible-Proxmox/`](../ansible-Proxmox/README.md)
