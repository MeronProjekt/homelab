# Uptime Kuma

Monitoring aller selbst gehosteten Services im Homelab.

## Stack
- Uptime Kuma (Docker-Container auf Proxmox LXC)
- Netz: 192.168.1.3:3001 (internes LAN)

## Setup

Installiert über Docker Compose (siehe [`docker-compose.yml`](docker-compose.yml)):

```bash
mkdir -p /root/uptime-kuma && cd /root/uptime-kuma
# docker-compose.yml anlegen (siehe Datei in diesem Ordner)
docker compose up -d
```

Web-Interface danach erreichbar unter `http://192.168.1.3:3001` — Admin-Account 
wird beim ersten Aufruf im Browser eingerichtet.

## Zweck
- Überwacht Erreichbarkeit von Nextcloud, Immich, AdGuard, Dashboard u.a.
- Benachrichtigung bei Ausfällen

## Verwaltung
Wird über Ansible automatisiert verwaltet, siehe [`../ansible-Proxmox/`](../ansible-Proxmox/README.md)
