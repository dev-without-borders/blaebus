# Roadmap – blaebus

> Phasen-basierte Vorwärtssicht. Darf sich ändern – Änderungen werden im PROJECT_LOG dokumentiert. Letzte Aktualisierung: 2026-03-16

---

## Übergeordnete Ziele

|Priorität|Ziel|Beschreibung|
|---|---|---|
|1|Lernen & POW|Nachvollziehbarer Lernprozess, öffentliches Portfolio|
|2|Effizienz|Pipeline soll im Alltag tatsächlich nützlich sein|
|3|Datenschutz|DSGVO-konform, volle Datensouveränität|
|4|Portability|Komponenten austauschbar, kein Vendor-Lock-in|
|5|Skalierbarkeit|Vom Solo-Setup zum potenziell mehrnutzerfähigen System|

---

## Phase 0 – Orientierung & Konzept ← AKTUELL

**Ziel:** Überblick gewinnen, Landschaft kartieren, Architektur finalisieren

**Ergebnisse:**

- [x] Plaud Note Pro recherchiert und dokumentiert
- [x] Pipeline-Architektur entworfen (5 Stufen)
- [x] Infrastruktur-Entscheidung (Heimserver + VPS + LLM API)
- [x] Dokumentationsgerüst aufgesetzt
- [ ] Scriberr evaluieren
- [ ] Obsidian Vault-Schema designen
- [ ] Open-Source-Landschaft vollständig kartieren
- [ ] GitHub-Repo-Struktur festlegen

---

## Phase 1 – Minimaler Prototyp (MVP)

**Ziel:** Eine funktionsfähige Kette von Telegram-Sprachnachricht bis Obsidian-Notiz

**Geplante Ergebnisse:**

- [ ] Telegram-Bot empfängt Audio
- [ ] Audio → WAV Konvertierung (ffmpeg)
- [ ] Transkription mit faster-whisper
- [ ] Einfache LLM-Zusammenfassung
- [ ] Markdown-Output in Obsidian Vault
- [ ] Docker Compose Setup (lokal lauffähig)

---

## Phase 2 – Qualität & Features

**Ziel:** Pipeline robuster und nützlicher machen

**Geplante Ergebnisse:**

- [ ] Rauschunterdrückung (DeepFilterNet/RNNoise)
- [ ] Sprechererkennung (pyannote.audio)
- [ ] Intelligentes Tagging und Einordnung in Obsidian
- [ ] Mehrere Zusammenfassungs-Templates
- [ ] Error Handling und Retry-Logik

---

## Phase 3 – RAG & Wissensarchiv

**Ziel:** Aufnahmen durchsuchbar und abfragbar machen

**Geplante Ergebnisse:**

- [ ] ChromaDB Integration
- [ ] Embedding-Pipeline
- [ ] "Frag dein Archiv" via Telegram
- [ ] Verknüpfung mit bestehendem Obsidian-Wissen

---

## Phase 4 – Produktion & Deployment

**Ziel:** Stabil auf Hetzner VPS, zuverlässig im Alltag

**Geplante Ergebnisse:**

- [ ] Hetzner VPS aufgesetzt
- [ ] CI/CD Pipeline (GitHub Actions)
- [ ] Monitoring & Logging
- [ ] Backup-Strategie
- [ ] Dokumentation für Dritte nachvollziehbar

---

## Backlog / Ideen

- Anrufaufzeichnung (DSGVO-Minenfeld, bewusst zurückgestellt)
- Web-UI als Alternative/Ergänzung zu Telegram
- Multi-User-Fähigkeit
- Eigenes Whisper-Finetuning auf deutsche Dialekte/Fachsprache
- Integration mit anderen Notiz-Systemen (Notion, Logseq)
- Mobile App für direktere Aufnahme