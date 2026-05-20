# LAB8_SM - Rapport d'Analyse de Sécurité

## 📋 Fichiers de Documentation

- `analyse_info.txt` – métadonnées de l'audit
- `commands.log` – toutes les commandes exécutées

---

## 📦 Artefact Autorisé – Diva.apk

### 🔍 Étape 1 – Dépôt du Fichier

**Capture extraite de BeVigil :**

![BeVigil Interface](https://github.com/user-attachments/assets/48bf8e3c-2709-41f1-93d3-ce3d4ee5cdbd)

✅ **diva.apk prêt à être analysé**
- **Package :** jakhar.aseem.diva
- **Version :** 1.0

### ✅ Confirmation de Réception

![Confirmation BeVigil](https://github.com/user-attachments/assets/cd8cf949-2ee3-4b4e-999e-11d7268f8d9e)

**Message :** « Merci d'avoir soumis le fichier APK »

### 🔐 Hash SHA-256 (Traçabilité)

```powershell
Get-FileHash -Path "00-scope/diva.apk" -Algorithm SHA256
```

Résultat ajouté dans `analyse_info.txt`

---

## 🌐 BeVigil – Scan Externe

### 🖥️ Interface et Résultats

![BeVigil Results](https://github.com/user-attachments/assets/f8532033-7c58-4f54-af93-9b67f63728f9)

### 📊 Détection Globale

| Indicateur | Valeur |
|---|---|
| Total issues | 2 |
| High | 0 |
| Med | 1 |
| Low | 1 |
| Security Rating | 8.2 / 10 (Good) |

### 🧨 Vulnérabilités Identifiées

| Severity | Description |
|---|---|
| 🟠 **MED** | Non-parameterized SQL Query |
| 🟡 **LOW** | Possible Secret Detected |

💡 **Remarque Soukaina :** Une requête SQL non paramétrée peut mener à une injection. À corréler avec Yaazhini.

---

## 🧠 Yaazhini – Scan Interne & Proxy

### ⚙️ Décompression et Analyse

![Yaazhini Decompilation](https://github.com/user-attachments/assets/87e77057-387a-4ab7-b1fe-11b498d43a73)

```
✔ APK scan initialized
✔ APK verified
✔ Decompilation started
```

### 🔐 Installation du Certificat Proxy

![Proxy Certificate](https://github.com/user-attachments/assets/378078ee-ca9a-4154-bc14-9611619ee1c6)

- **Délivré à :** YaazhiniProxy
- **Validité :** 20/03/2026 → 22/06/2028
- ✅ **Installation réussie** dans le magasin racine de confiance

Ce certificat permet l'inspection TLS pour détecter des communications non chiffrées ou des secrets en transit.

---

## 📊 Triage & Normalisation

**Fichier :** `03-triage/triage.csv`

| ID | Source | Élément | Sévérité | Confiance | Statut |
|---|---|---|---|---|---|
| FIND-001 | BeVigil | Non-parameterized SQL Query | MED | Forte | Confirmé |
| FIND-002 | BeVigil | Possible secret (hardcoded) | LOW | Moyenne | À vérifier |
| FIND-003 | Yaazhini | Proxy cert installed (audit OK) | INFO | Forte | Info technique |
| FIND-004 | BeVigil+Yaazhini | Communication claire (HTTP) | MED | Forte | Confirmé |
| FIND-005 | Yaazhini | Backup activé dans manifest | MED | Forte | Confirmé |

✨ **Dédoublonnage fait** – 5 constats uniques consolidés

---

## ⚙️ Corrélation OWASP

**Fichier :** `03-triage/owasp_mapping.md`

| ID | Catégorie OWASP | Référence |
|---|---|---|
| FIND-001 | MASVS-CODE | V4.1 (SQL injection) |
| FIND-002 | MASVS-STORAGE | V2.1 (secrets hardcodés) |
| FIND-004 | MASVS-NETWORK | V5.1 (TLS obligatoire) |
| FIND-005 | MASVS-STORAGE | V2.8 (backup désactivé) |

---

## 📝 Résumé

Ce laboratoire documente l'analyse complète de sécurité de l'application DIVA à travers deux outils complémentaires :
- **BeVigil** pour le scan externe
- **Yaazhini** pour le scan interne avec proxy TLS

Les résultats ont été triés, dédupliqués et mappés selon les standards OWASP MASVS.
