# Nextcloud (selbst gehostet)

Eigene Nextcloud-Instanz für Dateisync, Kalender und Videomeetings (Nextcloud Talk, 
bis zu 10 Teilnehmer) — von außen erreichbar über DuckDNS.

## Stack (Stand: September 2026)

- **Betriebssystem**: Debian (LXC-Container 101 auf Proxmox)
- **Webserver**: Apache 2.4.66
- **PHP**: 8.2.30 (mit OPcache)
- **Datenbank**: MariaDB 10.11.14
- **Erreichbarkeit von außen**: DuckDNS + Nginx Proxy Manager (siehe [`../Proxy/`](../Proxy/README.md))

## Warum dieses Setup?

Bewusst **ohne Docker** aufgesetzt (klassisches LAMP-Stack), um Webserver-, PHP- und 
Datenbank-Konfiguration in der Praxis zu verstehen, statt nur fertige Container zu starten.

## Diagnose-Befehle (aktuellen Stand prüfen)

Alle Befehle werden vom **Proxmox-Host** aus per `pct exec` ausgeführt:

```bash
# Apache-Version
pct exec 101 -- apache2 -v

# PHP-Version
pct exec 101 -- php -v

# MariaDB-Version
pct exec 101 -- mysql --version

# Installationspfad von Nextcloud finden
pct exec 101 -- find / -maxdepth 3 -iname "nextcloud" -type d 2>/dev/null

# Nextcloud-eigene Versionsnummer
pct exec 101 -- php /var/www/nextcloud/occ status
```

## Wichtige Hinweise

- Config-Dateien (`config.php`) und Zugangsdaten werden **bewusst nicht** in diesem 
  Repository geführt, da sie Datenbank-Passwörter und Secrets enthalten
- SSL/HTTPS wird über den vorgeschalteten Nginx Proxy Manager terminiert (Let's Encrypt)

## Nächste Schritte / Ausbau

- [ ] Automatisiertes Backup der Nextcloud-Datenbank per Ansible
- [ ] Monitoring der Nextcloud-Verfügbarkeit über Uptime-Kuma
