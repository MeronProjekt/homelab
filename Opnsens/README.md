# OPNsense (Firewall & VPN)

Zentrale Firewall zwischen Fritzbox-Netz (WAN) und internem LAN, inkl. WireGuard-VPN 
für sicheren mobilen Zugriff.

## Stack
- **OPNsense** (eigene VM auf Proxmox, kein LXC-Container)
- **WAN**: 192.168.178.2 (Fritzbox-Netz)
- **LAN**: 192.168.1.1 (internes Service-Netz)

## Warum eine eigene Firewall-VM (statt nur Fritzbox)?

Die Fritzbox bietet nur einfache Portfreigaben — OPNsense ermöglicht:
- Echte Netzwerksegmentierung (WAN/LAN als getrennte Sicherheitszonen)
- Gezielte, minimale Firewall-Regeln statt pauschaler Freigaben (Least Privilege)
- VPN-Zugang (WireGuard) für sicheren Zugriff von unterwegs auf das gesamte interne Netz
- Zentrale DNS/DHCP-Kontrolle für das interne Netz

## Firewall-Prinzip

Firewall-Regeln werden **gezielt** angelegt: einzelne Quell-IP → einzelne Ziel-IP → 
einzelner Port, statt ganze Netze pauschal zu öffnen.

**Beispiel** (eingerichtet für Ansible-Zugriff auf AdGuard):
- Interface: LAN
- Action: Pass
- Protocol: TCP
- Source: Ansible-Control (192.168.1.108)
- Destination: AdGuard (192.168.178.105)
- Port: 22 (SSH)

> Weitere Regeln sind eingerichtet (u.a. für Uptime-Kuma) — Details siehe direkt im 
> OPNsense-Webinterface unter **Firewall → Rules**, da diese sich bewusst nicht in 
> Klartext in diesem Repository befinden.

## WireGuard-VPN

Eingerichtet für sicheren Zugriff auf das komplette interne Netz von unterwegs, statt 
einzelne Services einzeln nach außen zu öffnen.

- Konfiguration: OPNsense-Webinterface unter **VPN → WireGuard**
- Peer-Konfigurationen (Client-seitig) werden **nicht** in diesem Repository geführt, 
  da sie private Schlüssel enthalten

## Backup der Konfiguration

OPNsense bietet eine integrierte Backup-Funktion (**System → Configuration → Backups**), 
die einen vollständigen XML-Export erzeugt. Dieser Export wird **bewusst nicht** hier 
im Repository abgelegt, da er Passwort-Hashes, VPN-Schlüssel und Zertifikate im 
Klartext enthält — stattdessen lokal/verschlüsselt gesichert.

## Details zur Gesamt-Netzwerk-Architektur

Siehe [`../docs/netzwerk.md`](../docs/netzwerk.md) für die komplette Topologie und 
IP-Übersicht aller Hosts.
