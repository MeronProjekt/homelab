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

## Installation (Grundschritte)

Manuelle Installation auf Debian, kein Docker. Grober Ablauf:

```bash
# System aktualisieren
apt update && apt upgrade -y

# Apache installieren
apt install -y apache2

# PHP 8.2 + benötigte Module installieren
apt install -y php8.2 php8.2-gd php8.2-mysql php8.2-curl php8.2-mbstring \
    php8.2-intl php8.2-gmp php8.2-bcmath php8.2-xml php8.2-imagick \
    php8.2-zip libapache2-mod-php8.2

# MariaDB installieren
apt install -y mariadb-server
mysql_secure_installation

# Datenbank für Nextcloud anlegen
mysql -u root -p
# In der MySQL-Shell:
#   CREATE DATABASE nextcloud;
#   CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY '<PASSWORT>';
#   GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';
#   FLUSH PRIVILEGES;
#   EXIT;

# Nextcloud herunterladen und entpacken
cd /var/www
wget https://download.nextcloud.com/server/releases/latest.zip
unzip latest.zip
chown -R www-data:www-data /var/www/nextcloud

# Apache-Konfiguration für Nextcloud anlegen (VirtualHost)
# /etc/apache2/sites-available/nextcloud.conf
a2ensite nextcloud.conf
a2enmod rewrite headers env dir mime setenvif ssl
systemctl restart apache2
```

Danach: Ersteinrichtung über den Browser (`http://<SERVER-IP>`) — Admin-Account und 
Datenbankverbindung werden dort eingerichtet.

## Nextcloud Talk (Videomeetings)

Zusätzlich installierte App für Videomeetings (bis zu 10 Teilnehmer). Installation 
über den Nextcloud App Store im Web-Interface (Verwaltung → Apps → "Talk").


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
