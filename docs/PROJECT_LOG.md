# Project Log – blaebus

> Chronologisches Protokoll aller Sessions, Entscheidungen und offenen Fragen. Neueste Session oben.

---

## 2026-03-16 – Session 02: Brainstorming & Landscaping

**Ziel:** Möglichkeiten und Grenzen der Pipeline abstecken, Landschaft kartieren, Dokumentationsgerüst aufsetzen.

**Besprochen:**

- Dokumentationsstrategie festgelegt: 4 lebende Dokumente (PROJECT_LOG, ROADMAP, ARCHITECTURE, DECISIONS)
- Obsidian ↔ GitHub Sync evaluiert → Obsidian Git Plugin (Vinzent03) als Empfehlung
- Vault-Ordner = geklontes Repo, Auto-Sync konfigurierbar
- Brainstorming-Vorgehen in 3 Runden definiert:
    1. Landkarte (Pipeline Stufe für Stufe, explorativ)
    2. Querschnittsthemen (DSGVO, Docker, GitHub-Struktur, Obsidian-Integration)
    3. Priorisierung und offene Fragen

**Entscheidungen:**

- → Dokumentation von Anfang an, strukturiert und nachvollziehbar
- → Obsidian Git für Vault ↔ GitHub Sync (siehe DECISIONS.md #002)

**Offene Fragen:**

- [ ] Repo-Name und GitHub-Struktur festlegen
- [ ] Scriberr-Evaluation (aus Session 01 übernommen)
- [ ] Obsidian Vault-Schema für KI-generierte Notizen designen

---

## 2026-03-XX – Session 01: Initiale Planung (Zusammenfassung)

**Hinweis:** Diese Session fand über mehrere Gespräche statt. Zusammengefasst aus dem Context-Snapshot.

**Besprochen:**

- Recherche Plaud Note Pro: Funktionsweise dokumentiert, DSGVO-Konformität kritisch bewertet
- DIY-Pipeline-Architektur entworfen: 5 Stufen (Erfassung → Vorverarbeitung → Transkription → KI-Zusammenfassung → Ausgabe)
- Open-Source-Landschaft gesichtet: whisperbot, whisper-transcriber-telegram-bot, Scriberr, Meetily, Char
- Infrastruktur-Architektur entschieden: Heimserver (Dev) + Hetzner VPS (Prod) + LLM Pay-per-Use
- Kostenrahmen: ~13–23 €/Monat vs. Plaud ~190 € + 100–240 €/Jahr

**Entscheidungen:**

- → Telegram-Bot als Frontend/Interface
- → faster-whisper (CPU, VPS) für Transkription
- → pyannote.audio für Sprechererkennung
- → DeepFilterNet/RNNoise für Rauschunterdrückung
- → ChromaDB für Vektordatenbank/RAG
- → Obsidian Vault als Zielspeicher
- → Docker Compose für Deployment

**Offene Fragen (übernommen in Session 02):**

- [ ] Scriberr evaluieren
- [ ] Obsidian Vault-Integration designen
- [ ] Hetzner VPS aufsetzen