# OmniSight EDR — Scope Final (FIGÉ)

> ⚠️ Ce scope est définitif. Toute feature hors-scope est REJETÉE.

## 🎯 Mission
EDR open-source basé sur Wazuh, avec module custom de détection de malwares par signatures YARA.

## ✅ Features IN scope

### 1. Stack EDR de base (fork Wazuh)
- Manager + Indexer (OpenSearch) + Dashboard
- 1 agent Linux (container Docker Ubuntu 22.04)
- Inscription agent automatique
- File Integrity Monitoring (syscheck) actif
- Log collection actif

### 2. Module Scanner Malware (omnisight-scanner) ⭐ DIFFÉRENCIATEUR
- Scan ON-DEMAND uniquement (déclenché via API ou CLI)
- Moteur YARA (yara-python) avec règles open-source (yara-rules/rules)
- Quarantaine automatique des fichiers détectés (move vers /quarantine + chmod 000)
- Génération alerte Wazuh (rule.level=12) avec hash SHA256 + nom règle YARA matchée
- Cache SQLite des fichiers scannés (hash → résultat) pour éviter rescan

### 3. Intégration Dashboard
- Nouveau tab "Malware Scanner" dans le dashboard Wazuh
- Vue: liste des détections, bouton "Trigger scan" sur agent sélectionné
- Stats: nb scans, nb détections, top règles matchées

### 4. Rebranding minimal (2h max)
- Logo dashboard
- Titre HTML + favicon
- README principal

## ❌ Features OUT of scope

- Scan temps réel / périodique
- ClamAV, VirusTotal, ou toute API externe
- ML / détection comportementale
- SOAR / playbooks complexes
- Multi-OS (Windows, macOS)
- Multi-agent (>1 endpoint pour la démo)
- API REST custom étendue (uniquement endpoint /scan)
- Refonte complète du dashboard

## 🎬 Scénario démo (45 sec)

1. `docker compose up -d` → stack opérationnelle
2. Dashboard montre 1 agent connecté, état healthy
3. Drop EICAR sample dans /samples du container agent
4. Trigger scan via dashboard ou `curl POST /scan`
5. Alerte rouge apparaît en <3s
6. Détails: hash SHA256, règle YARA, timestamp, action prise
7. Fichier en quarantaine vérifiable

## 📦 Repos actifs

| Repo | Status |
|------|--------|
| omnisight | Orchestration ✅ |
| omnisight-core | Fork Wazuh (rebranding minimal) |
| omnisight-dashboard | Fork dashboard (+ tab Malware) |
| omnisight-scanner | **NOUVEAU - différenciateur** |
| omnisight-deploy | Docker compose + samples |
| omnisight-docs | Pitch + architecture |

## 🗑️ Repos à archiver

- omnisight-ml, omnisight-response, omnisight-api (hors scope)
- omnisight-website, omnisight-demo (optionnels, plus tard si temps)