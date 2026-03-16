# Architecture Decision Records (ADR) – blaebus

> Jede technische Entscheidung wird hier dokumentiert: Kontext, betrachtete Optionen, Entscheidung, Begründung. Format angelehnt an [Michael Nygard's ADR-Vorlage](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions).

---

## ADR-001: Grundlegende Architekturentscheidungen

**Datum:** 2026-03 (Session 01) **Status:** Akzeptiert

### Kontext

Aufbau einer selbstgehosteten Blaebus als Alternative zum Plaud Note Pro. Kernanforderungen: Datenschutz (DSGVO), keine Cloud-Abhängigkeit für Kerndaten, geringes Budget, Lernprojekt.

### Betrachtete Optionen

**Interface:**

- Telegram-Bot → gewählt
- Eigenes Web-UI → zu viel Aufwand für den Start, keine Push-Notifications
- Native App → unverhältnismäßiger Entwicklungsaufwand

**Transkription:**

- faster-whisper (lokal, CPU) → gewählt
- Original OpenAI Whisper → langsamer, mehr RAM
- Whisper.cpp → gut für Edge-Devices, aber Python-Ökosystem bevorzugt
- Cloud-APIs (Google, AssemblyAI) → widerspricht Datensouveränität

**LLM-Zusammenfassung:**

- Pay-per-Use API (OpenRouter / direkt) → gewählt
- Lokales LLM auf VPS → CPU zu langsam für brauchbare Modelle
- Festes Cloud-Abo → unnötige Fixkosten bei variablem Bedarf

**Deployment:**

- Docker Compose → gewählt
- Kubernetes → Overkill für Solo-Projekt
- Bare Metal → nicht reproduzierbar, schwer zu migrieren

**Hosting:**

- Hetzner VPS → gewählt
- Eigener Heimserver (Produktion) → nicht zuverlässig genug (Strom, IP, Uptime)
- Andere Hoster → Hetzner ideal wegen DE-Rechenzentren, Preis-Leistung

### Entscheidung

Telegram-Bot + faster-whisper (CPU) + LLM Pay-per-Use + Docker Compose + Hetzner VPS.

### Begründung

Maximale Datensouveränität bei minimalem Budget (~13–23 €/Monat). Docker Compose gewährleistet Portability zwischen Heimserver (Dev) und VPS (Prod). Telegram als Interface eliminiert Frontend-Entwicklung und liefert Push-Notifications gratis. Nur die LLM-Zusammenfassung geht nach extern – dort ist Pay-per-Use die pragmatischste Lösung, weil lokale LLMs auf CPU nicht konkurrenzfähig sind.

### Konsequenzen

- Transkription begrenzt durch CPU-Leistung des VPS → Whisper-Modellgröße muss getestet werden (small vs. medium)
- LLM-API als einzige externe Abhängigkeit → Provider-Wechsel muss einfach sein
- Docker Compose als Deployment-Tool muss gut dokumentiert werden

---

## ADR-002: Obsidian ↔ GitHub Sync

**Datum:** 2026-03-16 (Session 02) **Status:** Akzeptiert

### Kontext

Projektdokumentation soll sowohl im GitHub-Repo als auch in der lokalen Obsidian Vault verfügbar sein, idealerweise mit minimalem manuellem Aufwand.

### Betrachtete Optionen

- **Obsidian Git** (Vinzent03) → gewählt
    - Etabliert, große Community, echte Git-Integration
    - Auto-Commit/Push/Pull konfigurierbar
    - Source Control View, Diff-Ansicht im Editor
    - Setzt lokale Git-Installation voraus
- **GitHub Gitless Sync**
    - Kein lokales Git nötig, arbeitet über GitHub API
    - Neu (Community-Plugin seit 03/2025), kleinere Community
    - Gut für Nutzer ohne Git-Kenntnisse
- **Manuelles Git im Terminal**
    - Volle Kontrolle, kein Plugin nötig
    - Kein Komfort, leicht zu vergessen

### Entscheidung

Obsidian Git (Vinzent03).

### Begründung

Git-Kenntnisse sind ein explizites Lernziel → echte Git-Integration ist Vorteil, nicht Hürde. Auto-Sync verhindert vergessene Commits. Größte Community = bester Support. Vault-Ordner = Repo-Klon = ein einziger Ort für alles.

### Konsequenzen

- Git muss auf allen Geräten installiert sein, auf denen Obsidian läuft
- `.gitignore` muss sinnvoll konfiguriert werden (workspace.json, .trash/)
- Mobile Sync über dieses Plugin ist instabil → falls Mobile gewünscht, alternative Lösung nötig

---

## ADR-XXX: Vorlage

**Datum:** **Status:** Vorgeschlagen / Akzeptiert / Abgelöst / Verworfen

### Kontext

_Warum steht diese Entscheidung an?_

### Betrachtete Optionen

_Was wurde in Betracht gezogen?_

### Entscheidung

_Was wurde entschieden?_

### Begründung

_Warum diese Option?_

### Konsequenzen

_Was folgt daraus – positiv und negativ?_