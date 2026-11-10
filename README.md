# 🐙 CuttleFix

> *Une collection de scripts d'audit et de remédiation, un dossier par CVE, pour corriger vite et proprement.*

![License](https://img.shields.io/github/license/CyberPoulpe/CuttleFix?style=flat-square)
![Language](https://img.shields.io/github/languages/top/CyberPoulpe/CuttleFix?style=flat-square)
![Stars](https://img.shields.io/github/stars/CyberPoulpe/CuttleFix?style=flat-square)

---

## 🎯 Aperçu & Objectif

Quand une CVE tombe, il faut répondre à deux questions : **« suis-je impacté ? »** puis **« comment corriger ? »**. CuttleFix regroupe des scripts prêts à l'emploi qui répondent à ces deux questions, CVE par CVE, sur Linux et Windows.

Le dépôt est destiné aux **administrateurs système** et aux **équipes Cyber / Blue Team** qui doivent vérifier et corriger rapidement un parc de machines.

### Key Features
- 🔍 **Un script d'audit par CVE :** détecte si la machine est vulnérable, sans rien modifier.
- 🛠️ **Un script de remédiation par CVE :** applique la correction et vérifie le résultat.
- 📁 **Un dossier par CVE :** chaque correctif est isolé, documenté et indépendant des autres.
- 🚦 **Codes de retour homogènes :** intégration facile dans une supervision, une GPO, Ansible ou un pipeline.
- 🖥️ **Multi-plateforme :** versions Bash (Linux) et PowerShell (Windows) selon la CVE.

---

## 📚 Catalogue des CVE

| CVE | Composant | Description | Plateformes | Dossier |
|---|---|---|---|---|
| CVE-2026-91403 | Java JSSE / eMudhra | Suppression des certificats eMudhra des keystores Java (`cacerts`) | Linux, Windows | [`CVE-2026-91403/`](./CVE-2026-91403) |

*(Cette table est mise à jour à chaque nouvelle CVE ajoutée.)*

---

## 🗂️ Structure du dépôt

```text
CuttleFix/
├── README.md                  # Ce fichier
├── CVE-2026-91403/
│   ├── README.md              # Détails de la CVE et mode d'emploi
|   ├── Linux
|   |   ├── CVE-2026-91403_check.sh
|   |   └── CVE-2026-91403_patch.sh
|   ├── Windows
│   |   ├── CVE-2026-91403_check.ps1
│   |   └── CVE-2026-91403_patch.ps1
└── CVE-XXXX-XXXXX/            # Prochaines CVE...
```

### Conventions

Pour garder le dépôt homogène, chaque dossier de CVE suit les mêmes règles :

- **Nom du dossier :** l'identifiant exact de la CVE (`CVE-ANNÉE-NUMÉRO`).
- **Nom des scripts :** `CVE-ANNÉE-NUMÉRO_check.<ext>` pour l'audit et `CVE-ANNÉE-NUMÉRO_patch.<ext>` pour la remédiation (`.sh` pour Linux, `.ps1` pour Windows).
- **Un `README.md` par dossier :** contexte de la CVE, prérequis, utilisation, limites connues.
- **Codes de retour :**

| Code | Signification |
|---|---|
| `0` | Machine saine, non concernée, ou remédiation réussie |
| `1` | Erreur d'exécution (ex. droits insuffisants) |
| `2` | Machine vulnérable, ou anomalie persistante après remédiation |

---

## 🚀 Installation & Prérequis

### Prérequis
- **Linux :** Bash et droits `root` (`sudo`)
- **Windows :** PowerShell 5.1+ / 7.x, exécuté en **administrateur**
- Les prérequis propres à chaque CVE (ex. `keytool` pour une CVE Java) sont détaillés dans le README du dossier concerné.

### Installation
```bash
git clone https://github.com/CyberPoulpe/CuttleFix.git
cd CuttleFix
chmod +x */*.sh
```

### 🔧 Troubleshooting

Si vous obtenez l'erreur suivante :

```text
-bash: ./CVE-XXXX-XXXXX_check.sh : ne peut exécuter : le fichier requis n'a pas été trouvé
```

Les fins de ligne sont au format Windows (CRLF). Convertissez les scripts :

```bash
find . -type f -name "*.sh" -exec sed -i -e 's/\r$//' {} +
```

---

## 💡 Utilisation

Le principe est toujours le même : **auditer d'abord, corriger ensuite**.

```bash
# Linux
cd CVE-2026-91403
sudo ./CVE-2026-91403_check.sh     # 1. Suis-je impacté ?
sudo ./CVE-2026-91403_patch.sh     # 2. Correction
```

```powershell
# Windows (PowerShell administrateur)
cd CVE-2026-91403
.\CVE-2026-91403_check.ps1
.\CVE-2026-91403_patch.ps1
```

Consultez le `README.md` de chaque dossier pour le détail de la CVE concernée.

### ⚠️ Bonnes pratiques

- **Lancez toujours l'audit avant la remédiation**, et relancez-le après pour valider la correction.
- **Sauvegardez** les fichiers ou configurations modifiés avant de patcher.
- **Testez** sur un environnement de préproduction avant un déploiement à grande échelle.
- **Redémarrez** les services concernés si la correction l'exige.

---

## 🛡️ Sécurité & Avertissement

Ces scripts modifient la configuration des systèmes sur lesquels ils sont exécutés. Ils sont fournis à des fins éducatives et d'audit interne uniquement, sans aucune garantie. L'utilisateur est seul responsable de son utilisation conformément aux lois et réglementations en vigueur.

---

## 🤝 Contribution & Contact

Les contributions, signalements de bugs ou suggestions d'améliorations sont les bienvenus ! Pour ajouter une CVE, créez un dossier à son nom en respectant les conventions ci-dessus, puis ajoutez une ligne au catalogue.

- **Auteur :** CyberPoulpe
- **GitHub :** [@CyberPoulpe](https://github.com/CyberPoulpe)
