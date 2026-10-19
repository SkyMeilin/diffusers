# AGENTS.md — System- & Agenten-Architektur

Dieses Dokument definiert die Rollen, Workflows und Sicherheitsprotokolle für das autonome Multi-Agenten-System ("Sky Meilin" OS-Overlay, Planner & Publishing Pipeline).

---

## 1. Master Orchestrator: "Sky Meilin"

* **Rolle**: Persönliche Assistenz, zentrales Betriebssystem-Overlay & Steuerpult [1, 2].
* **Interface**: Transparentes Flutter Desktop/Mobile Overlay (Always-on-Top), Bluetooth-Headset (A2DP/HFP), STT/TTS-Loop, Kamera-Streaming (WebRTC/OpenCV) [1, 2].
* **Hauptaufgaben**:
  * Empfängt Sprach- und Texteingaben, steuert den Systemkontext und verteilt Aufgaben an Spezialagenten [1, 2].
  * Bündelt den Datenstrom über den **FastAPI Streaming Endpoint** (`/stream` mit `text/event-stream` SSE) zur Echtzeit-Ausgabe auf der Benutzeroberfläche [1, 3, 4].
  * Gewährleistet die lokale Prozessisolation und das Revisions-Handling [2, 5].

---

## 2. Spezialisierte Kern- & Profi-Agenten

### A. PlannerAgent (Open-Source Kern)

* **Framework**: Dart / Flutter (`PlannerAgent`) [6, 7].
* **Aufgaben**:
  * Verarbeitet Raum- und Graphenstrukturen (`ArchitecturalSpace`) mit Parametern wie `areaSqm`, `heightMeters`, `usageType` und `isBlocked` [6, 7].
  * Freie Basis-Planung und interaktive Steuerung von Kompartimenten (z. B. Büro A, Flur Nord) [6, 7].

### B. NormenAgent (Premium Modul)

* **Status**: Kostenpflichtige Lizenzierung via `PremiumServicesController` (14,99 €) [6, 7].
* **Aufgaben**:
  * Prüft Bauakten, Raumhöhen und LBO-Compliance in Echtzeit [6, 7].
  * Validiert Spezial-Container nach **DIN 14505** und **DIN 30722-1** (z. B. Hakenhöhe 1,57 m, Außenbreite max. 2,55 m, lichte Raumhöhe $\ge$ 2,10 m sowie CBRN-Überdruckkapselung für BOS/KRITIS-Einsätze) [8, 9].

### C. FlaechenAgent (Premium Modul)

* **Status**: Kostenpflichtige Lizenzierung (19,99 €) [6, 7].
* **Aufgaben**: Exakte DIN- und WoFlV-Flächenberechnung sowie gewerbliche Nutzwertanalysen [6, 7].

### D. AvatarStreamingAgent (Media Engine)

* **Schnittstellen**: Python FastAPI Backend (`mcp_server.py`), ComfyUI, Ollama (Port 11434) [3, 5, 10, 11].
* **Aufgaben**:
  * Wandelt Text-Chunks via **F5-TTS** oder **XTTS v2** in natürliche Sprache um [10].
  * Steuert Open-Source-Videomodelle (**CogVideoX**, **AnimateDiff**, **SVD**) in ComfyUI für das Avatar-Streaming an die App [10].

### E. PublishingAgent (Content Distribution)

* **Schnittstellen**: WordPress REST API, Google Apps Script Web-App Endpoint [11, 12].
* **Aufgaben**:
  * Automatisiertes Erstellen und Veröffentlichen von Beiträgen im lokalen Docker-WordPress-Setup (Port 8080, MariaDB 10.11) [11].
  * Ausführen von Publishing-Triggern über hinterlegte Google Apps Script Schnittstellen [12].

---

## 3. Kommunikation & Sicherheitsprotokolle

* **MCP-Schnittstelle (`mcp_server.py`)**: Die Prozesskopplung zwischen Flutter-Frontend und Python-Backend erfolgt deterministisch über das Model Context Protocol via stdio-Transport [5].
* **WORM-Protokoll (Write Once, Read Many)**:
  * Revisionssichere und unveränderliche Speicherung aller Audit-Logs und Systementscheidungen [2, 5].
  * Systemweit geschützt durch SHA-256 Integritäts-Hash (`7c8a9f2e3b1d4c5e6f7a8b9c0d1e2f3a4b5c6d7e`) [5].
* **Secrets & Isolation**:
  * Strikter Ausschluss von `.env`-Dateien in `.gitignore` zur Sicherung lokaler API-Keys [5].
  * Ausführung lokaler Sprachmodelle über isolierte Ollama-Container [11].
```<FollowUp>Möchtest du diese strukturierte `AGENTS.md` direkt als Datei in deinem Repository speichern oder sollen noch weitere Agenten ergänzt werden?</FollowUp>
