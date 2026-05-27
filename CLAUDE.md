# OmniSight EDR — Master Context

> **Lecture obligatoire avant toute action.** Ce fichier définit le scope figé, les règles strictes et les commandes du projet.

## 🎯 Mission (FIGÉE)

EDR open-source basé sur **fork Wazuh** + module custom de détection de malwares par signatures **YARA**.

**Deadline : 7 jours** (démo : 2026-06-03). Objectif : gagner le prix du meilleur projet du Master.

## ✅ Scope IN (rien d'autre n'est autorisé)

### 1. Stack EDR (fork Wazuh)
- Manager + Indexer OpenSearch + Dashboard
- 1 agent Linux dans container Docker Ubuntu 22.04
- Inscription agent automatique
- File Integrity Monitoring + Log collection actifs

### 2. Scanner malware YARA ⭐ DIFFÉRENCIATEUR
- Scan **on-demand uniquement** (API + CLI)
- Moteur `yara-python` avec règles open-source (yara-rules/rules)
- Quarantaine auto (move vers /quarantine + chmod 000)
- Alerte Wazuh rule.level=12 avec hash SHA256 + règle matchée
- Cache SQLite des hashes scannés

### 3. Dashboard
- Tab "Malware Scanner" : liste détections, bouton trigger scan, stats

### 4. Rebranding minimal (2h max)
- Logo dashboard + favicon + titre HTML + README

## ❌ Scope OUT (REJETER toute demande)

- Scan temps réel ou périodique
- ClamAV, VirusTotal, APIs externes
- ML / détection comportementale
- SOAR / playbooks
- Windows, macOS, multi-agent
- Refonte UI complète

## 📦 Repos actifs

| Repo | Rôle | Source |
|------|------|--------|
| `omnisight` | Orchestration, docs, ADRs | Ce repo |
| `omnisight-core` | Manager + agent Wazuh | Fork wazuh/wazuh |
| `omnisight-dashboard` | UI + tab Malware | Fork wazuh/wazuh-dashboard |
| `omnisight-scanner` | Module YARA scanner | **NEW (code custom)** |
| `omnisight-deploy` | Docker compose + samples | Fork wazuh/wazuh-docker |
| `omnisight-docs` | Pitch + architecture | New |

Tous sous `github.com/Omnisight-esp/`.

## 🎬 Scénario démo (45 sec)

1. `docker compose up -d` → stack opérationnelle
2. Dashboard : 1 agent connecté, état healthy
3. Drop sample EICAR dans `/samples/` du container agent
4. Trigger scan via dashboard (ou `curl POST /scan/file`)
5. Alerte rouge en <3s : hash SHA256 + nom règle YARA
6. Fichier auto-quarantiné dans `/quarantine/`
7. Montrer la règle YARA qui a matché + métadonnées

## 📜 Règles STRICTES (violations = rollback)

### Licence
- **GPLv2** hérité de Wazuh. NE JAMAIS supprimer les fichiers `NOTICE`, `COPYING`, `LICENSE`.

### Modifications interdites sur omnisight-core
- ❌ Code C dans `src/`, `framework/`, `wodles/`
- ❌ Configs dans `etc/`, `ruleset/`
- ❌ Noms de daemons (`ossec-*`, `wazuh-*`)
- ❌ API protocole agent↔manager (port 1514/1515)
- ❌ Tests, packages, install scripts

### Rebranding autorisé UNIQUEMENT
- ✅ Logo + favicon du dashboard
- ✅ Titre HTML de la page dashboard
- ✅ README.md racine
- ✅ Messages d'accueil/login UI

### Workflow obligatoire pour TOUTE modification
1. Audit non-destructif d'abord (`grep`, `find`, lecture seule)
2. Lister les fichiers concernés avant toute modif
3. Proposer un diff, attendre validation utilisateur
4. Appliquer
5. Commit atomique avec message conventionnel

### Commits
- Conventional Commits : `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`
- Branches : `feature/<name>`, `fix/<name>`
- PR vers `main` (pas de push direct sur main pour omnisight-scanner)

### Code Python (omnisight-scanner)
- Python 3.11+
- `ruff` + `black` (line-length=100)
- Type hints obligatoires
- `pytest` avec coverage min 70%
- Docstrings sur fonctions publiques

## 🛠️ Stack technique

| Composant | Tech |
|-----------|------|
| EDR base | Wazuh 4.x |
| Indexer | OpenSearch 2.x |
| Scanner | Python 3.11, yara-python, FastAPI, SQLite |
| Conteneurisation | Docker 24+ + Compose v2 |
| CI | GitHub Actions |

## 🗓️ Roadmap 7 jours

| Jour | Focus | Livrable |
|------|-------|----------|
| **J1** ✅ | Foundation | Repos créés, Wazuh forké, scope figé |
| **J2** | Deploy + Scanner MVP | Stack docker up + `omnisight-scanner` scaffold + scan EICAR OK |
| **J3** | Intégration manager | Scanner envoie alertes au manager Wazuh (custom rule) |
| **J4** | Tab Dashboard | Vue "Malware Scanner" dans dashboard |
| **J5** | Tests + samples | EICAR + 5 malwares de theZoo détectés |
| **J6** | Rebranding + dry-run | UI rebrandée + démo répétée 3x |
| **J7** | Pitch + release | Slides + `v0.1.0-demo` + repos publics |