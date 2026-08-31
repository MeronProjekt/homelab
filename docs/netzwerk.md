# Netzwerk-Architektur

## Topologie

# Internet
│
FritzBox (Gateway/Router) 192.168.178.1
│
OPNsense (Firewall/VPN) 192.168.178.2 (WAN)
│ 192.168.1.1 (LAN)
├── Service-Subnetz (LAN) 192.168.1.0/24
└── Fritzbox-Netz (WAN) 192.168.178.0/


## Warum zwei Netze?

- **192.168.178.x (Fritzbox-Netz)**: Dienste, die von allen Geräten im Haushalt direkt 
  erreichbar sein müssen (z.B. AdGuard als DNS-Filter für alle Geräte)
- **192.168.1.x (internes LAN hinter OPNsense)**: Verwaltungs- und Anwendungs-Services, 
  geschützt hinter der Firewall — entspricht dem Prinzip "Management-Netz getrennt vom 
  Rand-Netz", wie es auch in Unternehmen üblich ist

## Geräte & Services

| Host | IP | Netz | Zweck |
|---|---|---|---|
| FritzBox | 192.168.178.1 | Gateway | Internet-Zugang, DHCP für 178.x |
| OPNsense | 192.168.178.2 (WAN) / 192.168.1.1 (LAN) | Firewall/VPN | Netzwerksegmentierung, WireGuard-VPN |
| Proxmox-Host | 192.168.178.10 | Hypervisor | Läuft alle VMs/Container |
| AdGuard Home | 192.168.178.105 | 178.x | DNS-Filterung (Werbung, NSFW) für alle Geräte |
| Nextcloud | 192.168.1.5 | 1.x (LAN) | Dateisync, Kalender, Talk-Meetings |
| Immich | 192.168.1.10:2283 | 1.x (LAN) | Foto-Verwaltung (Google-Photos-Ersatz) |
| Uptime Kuma | 192.168.1.3:3001 | 1.x (LAN) | Monitoring aller Services |
| Dashboard | 192.168.1.20 | 1.x (LAN) | Übersicht über alle Services |
| Ansible-Control | 192.168.1.108 | 1.x (LAN) | Automatisierung (siehe `../ansible-proxmox/`) |
| Nginx Proxy Manager | — | 1.x (LAN) | Reverse Proxy für interne `.home`-Domains + externe HTTPS |

## Externe Erreichbarkeit

- **DuckDNS**: Dynamic DNS für Dienste, die von außen erreichbar sein sollen (z.B. Nextcloud)
- **WireGuard**: VPN für sicheren mobilen Zugriff auf das komplette interne Netz, 
  statt einzelne Dienste einzeln nach außen zu öffnen

## Interne Domains (`.home`)

Über Nginx Proxy Manager intern aufgelöst, kein Zugriff von außen nötig:

- `fotos.meronsfamily.home` → Immich
- `cloud.meronsfamily.home` → Nextcloud
- `uptime.meronsfamily.home` → Uptime Kuma
- `dashboard.meronsfamily.home` → Dashboard

## Sicherheitsprinzip

Firewall-Regeln zwischen den Netzen sind **gezielt und minimal** (Least Privilege) — 
z.B. darf nur die Ansible-Control-IP gezielt auf Port 22 zu bestimmten Ziel-Hosts, statt 
ganze Netzsegmente pauschal zu öffnen.
