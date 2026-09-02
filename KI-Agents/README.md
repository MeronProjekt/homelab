# Hermes — Eigener KI-Agent

Selbst gebauter KI-Agent, der über Telegram kommuniziert: überwacht Webseiten, meldet 
Status und Probleme proaktiv, und kann bei Bedarf verwaltend eingreifen.

## Architektur


Telegram ←→ hermes_agent (Python) ←→ Anthropic API (Claude)
│
└──→ hermes_mcp (Node.js, Model Context Protocol Server)


## Komponenten

| Service | Technologie | Zweck |
|---|---|---|
| `hermes_agent` | Python 3.12 | Kernlogik: Telegram-Kommunikation, Webseiten-Checks, Anthropic-API-Anbindung |
| `hermes_mcp` | Node.js 20 | MCP-Server (Model Context Protocol) — strukturierte Tool-Anbindung für den Agenten |

## Stack
- Docker Compose (siehe [`docker-compose.yml`](docker-compose.yml))
- Anthropic Claude API (Haiku für schnelle Checks, Sonnet für komplexere Aufgaben)
- Telegram Bot API

## Setup

1. `.env` aus [`.env.example`](.env.example) erstellen und mit echten Werten befüllen:
```bash
   cp .env.example .env
   nano .env
```
2. Container starten:
```bash
   docker compose up -d
```

## Sicherheitshinweis

Die echte `.env`-Datei mit API-Keys und Bot-Token wird **niemals** in diesem Repository 
geführt (siehe `.gitignore`). Nur die `.env.example`-Vorlage mit Platzhaltern ist Teil 
der Dokumentation.

## Ressourcen-Limits

Der `hermes_agent`-Container läuft bewusst mit begrenzten Ressourcen (1.5 CPU-Kerne, 
1536 MB RAM), um andere Services auf dem Proxmox-Host nicht zu beeinträchtigen.
