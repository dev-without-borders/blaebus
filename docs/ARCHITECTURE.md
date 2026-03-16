# Architektur – blaebus

> Aktueller technischer Stand. Enthält Pipeline-Übersicht, Komponentenwahl und offene Positionen. Letzte Aktualisierung: 2026-03-16

---

## Pipeline-Übersicht

```
┌─────────────┐    ┌──────────────────┐    ┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│  1. ERFASSUNG│───▶│ 2. VORVERARBEITUNG│───▶│ 3. TRANSKRIPTION │───▶│ 4. KI-ANALYSE    │───▶│ 5. AUSGABE      │
│             │    │                  │    │                 │    │                  │    │                 │
│ Telegram Bot│    │ ffmpeg (→WAV)    │    │ faster-whisper  │    │ LLM via API      │    │ Obsidian Vault  │
│ Sprachnach- │    │ DeepFilterNet/   │    │ (CPU, small/med)│    │ (OpenRouter o.ä.)│    │ (Markdown +     │
│ richt       │    │ RNNoise          │    │                 │    │                  │    │  Frontmatter)   │
└─────────────┘    └──────────────────┘    └─────────────────┘    └──────────────────┘    └─────────────────┘
                                                  │                        │
                                                  ▼                        ▼
                                           ┌─────────────┐         ┌─────────────┐
                                           │ pyannote     │         │ ChromaDB    │
                                           │ (Speaker-    │         │ (Vektor-DB, │
                                           │  erkennung)  │         │  RAG-Abfrage)│
                                           └─────────────┘         └─────────────┘
```

---

## Komponentenmatrix

|Stufe|Komponente|Status|Technologie|Alternativen betrachtet|Entscheidung (→ DECISIONS.md)|
|---|---|---|---|---|---|
|1. Erfassung|Interface|✅ Entschieden|Telegram Bot|Web-UI, eigene App|#001|
|2. Vorverarbeitung|Audio-Konvertierung|✅ Entschieden|ffmpeg|–|Defacto-Standard|
|2. Vorverarbeitung|Rauschunterdrückung|⚠️ Tendenz|DeepFilterNet / RNNoise|Noisereduce, SpeechBrain|Noch zu evaluieren|
|3. Transkription|Speech-to-Text|✅ Entschieden|faster-whisper (CPU)|Original Whisper, Whisper.cpp, Cloud-APIs|#001|
|3. Transkription|Sprechererkennung|⚠️ Tendenz|pyannote.audio|NeMo, Resemblyzer|Noch zu evaluieren|
|4. KI-Analyse|Zusammenfassung|✅ Entschieden|LLM Pay-per-Use API|Lokales LLM, feste Cloud-Abos|#001|
|4. KI-Analyse|API-Gateway|🔲 Offen|OpenRouter? Direkt?|LiteLLM, direkte Provider-APIs|–|
|4. KI-Analyse|Vektordatenbank|⚠️ Tendenz|ChromaDB|Qdrant, Weaviate, Milvus|Noch zu evaluieren|
|5. Ausgabe|Notizspeicher|✅ Entschieden|Obsidian Vault|Notion, Logseq, Plain Files|#001|
|5. Ausgabe|Vault-Schema|🔲 Offen|?|?|Zu designen|
|Infra|Deployment|✅ Entschieden|Docker Compose|Kubernetes, Bare Metal|#001|
|Infra|Produktion|✅ Entschieden|Hetzner VPS (~8€/Mo)|Netcup, eigener Server|#001|
|Infra|Versionskontrolle|✅ Entschieden|GitHub|GitLab, Gitea|–|
|Infra|Obsidian ↔ Git|✅ Entschieden|Obsidian Git Plugin|GitHub Gitless Sync|#002|

---

## Infrastruktur-Topologie

```
┌──────────────────────────────────┐
│        ENTWICKLUNG (Lokal)       │
│  Heimserver (~30W Idle)          │
│  Docker Compose (identisch)      │
│  Restore-Möglichkeit             │
└──────────────┬───────────────────┘
               │ git push / Docker image
               ▼
┌──────────────────────────────────┐
│        PRODUKTION (Remote)       │
│  Hetzner VPS (~8 €/Monat)       │
│  Docker Compose                  │
│  ┌────────────────────────────┐  │
│  │ Telegram Bot (Python)      │  │
│  │ faster-whisper (CPU)       │  │
│  │ pyannote.audio             │  │
│  │ ChromaDB                   │  │
│  │ Pipeline-Orchestrierung    │  │
│  └────────────────────────────┘  │
└──────────────┬───────────────────┘
               │ API Calls (HTTPS)
               ▼
┌──────────────────────────────────┐
│        EXTERNE DIENSTE           │
│  LLM API (OpenRouter / direkt)   │
│  ~1–3 Cent pro Anfrage           │
└──────────────────────────────────┘
               │
               ▼
┌──────────────────────────────────┐
│        AUSGABE                   │
│  Obsidian Vault (lokal/sync)     │
│  GitHub Repo (Backup + Sync)     │
└──────────────────────────────────┘
```

---

## Offene Architektur-Fragen

1. **Wie kommt die Obsidian-Notiz vom VPS in den lokalen Vault?** Optionen: Git-Push vom VPS, Syncthing, rsync-Cronjob, Telegram als Transportkanal
2. **Vault-Schema:** Ordnerstruktur, Frontmatter-Standard, Tagging-Taxonomie, Verlinkungslogik
3. **API-Gateway-Strategie:** OpenRouter als Abstraktionsschicht vs. direkte Provider-APIs
4. **Queue/Worker-Architektur:** Brauchen wir eine Message Queue (Redis, RabbitMQ) oder reicht ein simpler Auftragsordner?
5. **Monitoring:** Wie überwachen wir die Pipeline? Healthchecks, Fehler-Notifications via Telegram?