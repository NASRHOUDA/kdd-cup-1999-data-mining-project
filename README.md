<div align="center">

# 🔐 KDD Cup 1999 — Network Intrusion Detection

**Détection d'intrusions réseau par Data Mining**

[![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3.0-F7931E?style=flat-square&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![Pandas](https://img.shields.io/badge/Pandas-2.0.3-150458?style=flat-square&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-639922?style=flat-square)](LICENSE)

[Vue d'ensemble](#-vue-densemble) · [Résultats](#-résultats) · [Structure](#-structure-du-projet) · [Démarrage](#-démarrage-rapide) · [Équipe](#-équipe)

</div>

---

## 📌 Vue d'ensemble

Ce projet analyse **4.9 millions de connexions réseau** pour détecter automatiquement les intrusions en s'appuyant sur le dataset KDD Cup 1999. Les connexions sont classifiées en **5 catégories** :

| Classe | Type | Exemples |
|--------|------|----------|
| ✅ **Normal** | Trafic légitime | Connexions utilisateur standard |
| 🚫 **DoS** | Déni de service | Neptune, Smurf, Teardrop, Land |
| 🔍 **Probe** | Reconnaissance réseau | Ipsweep, Nmap, Portsweep, Satan |
| 🔓 **R2L** | Accès distant non autorisé | Guess_passwd, Ftp_write, Imap |
| 👑 **U2R** | Élévation de privilèges | Buffer_overflow, Rootkit |

> **Meilleur modèle :** Random Forest — **99.78% de précision** sur 41 features

---

## 🔄 Pipeline

```
RAW DATA          PREPARATION          EDA               MODÉLISATION
─────────         ───────────          ───              ────────────────
494k lignes  →  • Déduplication   →  • Stats        →  • Logistic Regression
41 features     • Encodage           • Corrélations    • Random Forest
                • Normalisation      • Boxplots        • GridSearchCV
                • Outliers IQR       • Importance      • Validation croisée
                • Undersampling
                ──────────────
                145k lignes | 5 classes
```

---

## 📊 Résultats

### Comparaison des modèles

| Métrique | Logistic Regression | Random Forest |
|----------|:-------------------:|:-------------:|
| Accuracy | 97.74% | **99.78%** |
| Precision | 98.27% | **99.78%** |
| Recall | 97.74% | **99.78%** |
| F1-Score | 97.92% | **99.78%** |

### Features les plus discriminantes

| Rang | Feature | Importance | Description |
|------|---------|:----------:|-------------|
| 1 | `serror_rate` | 0.18 | Taux d'erreurs SYN |
| 2 | `srv_serror_rate` | 0.15 | Taux d'erreurs SYN par service |
| 3 | `dst_host_serror_rate` | 0.12 | Taux d'erreurs SYN hôte destination |
| 4 | `same_srv_rate` | 0.10 | Ratio même service |
| 5 | `dst_host_same_srv_rate` | 0.08 | Ratio service hôte destination |

---

## 📁 Structure du Projet

```
kdd-cup-1999-data-mining-project/
│
├── 📄 README.md
├── 📄 requirements.txt
├── 📄 LICENSE
│
├── 📁 data/
│   ├── raw/
│   │   ├── kddcup.data_10_percent       # 494k connexions (74.9 MB)
│   │   ├── kddcup.names                 # Noms des 41 features
│   │   └── training_attack_types        # Mapping des attaques
│   │
│   └── prepared/
│       ├── X_train_prepared.csv         # Features entraînement (25.9 MB)
│       ├── X_test_prepared.csv          # Features test (6.5 MB)
│       ├── y_train.csv / y_test.csv     # Labels
│       ├── scaler.pkl                   # StandardScaler
│       ├── label_encoder_flag.pkl       # Encodeur flag
│       ├── label_encoder_service.pkl    # Encodeur service
│       └── attack_mapping.json
│
├── 📓 notebooks/
│   ├── 01_data_preparation.ipynb        # P2 — Nettoyage & préparation
│   ├── 02_exploratory_data_analysis.ipynb  # P3 — EDA & visualisations
│   └── 03_modeling_evaluation.ipynb     # P4 — Modélisation & évaluation
│
├── 📊 results/
│   ├── figures/                         # Visualisations EDA (PNG)
│   └── models/                          # Modèles sauvegardés
│
├── 📚 docs/
│   ├── project_architecture.gif
│   └── confusion_lr.png / confusion_rf.png
│
└── 🔧 src/
    └── preprocessor.py                  # Fonctions de preprocessing
```

---

## ⚡ Démarrage Rapide

**Prérequis :** Python 3.8+, pip, Git

```bash
# 1. Cloner le dépôt
git clone https://github.com/NASRHOUDA/kdd-cup-1999-data-mining-project.git
cd kdd-cup-1999-data-mining-project

# 2. Installer les dépendances
pip install -r requirements.txt

# 3. Lancer Jupyter
jupyter notebook notebooks/
```

Exécuter les notebooks **dans l'ordre** :

| Étape | Notebook | Contenu |
|-------|----------|---------|
| 1 | `01_data_preparation.ipynb` | Nettoyage, encodage, normalisation |
| 2 | `02_exploratory_data_analysis.ipynb` | Visualisations et insights |
| 3 | `03_modeling_evaluation.ipynb` | Entraînement et évaluation |

---

## 🛠️ Stack Technique

| Catégorie | Librairie | Version |
|-----------|-----------|---------|
| Langage | Python | 3.8+ |
| Data Processing | pandas, numpy | 2.0.3, 1.24.3 |
| Machine Learning | scikit-learn | 1.3.0 |
| Imbalanced Learning | imbalanced-learn | 0.11.0 |
| Visualisation | matplotlib, seaborn | 3.7.2, 0.12.2 |
| Environnement | Jupyter Notebook | 1.0.0 |

---

## 💡 Insights Clés

- **Déséquilibre massif** : 78% des connexions sont des attaques DoS
- **Données dupliquées** : 70% du dataset original étaient des doublons
- **Protocole révélateur** : TCP/UDP/ICMP corrèle fortement avec le type d'attaque
- **Random Forest** surpasse la régression logistique grâce à sa gestion native de la non-linéarité et des features corrélées

---

## ⚠️ Limitations & Pistes d'amélioration

**Limitations :**
- Dataset de 1999 — ne couvre pas les attaques modernes (ransomware, APTs)
- Classe U2R très rare (0.04%) → risque de sous-généralisation
- Échantillon 10% du dataset complet

**Améliorations futures :**
- Tester sur NSL-KDD (version corrigée du KDD Cup 1999)
- Expérimenter XGBoost, LightGBM, et architectures Deep Learning
- Implémenter un pipeline de détection en temps réel
- Monitorer le concept drift en production

---

## 👥 Équipe

| Membre | Phase | Responsabilités |
|--------|-------|-----------------|
| **Zainab El Bouyed** | P1 — Data Understanding |
| **Houda Nasr** | P2 — Data Preparation |
| **Meryem El Mansouri** | P3 — EDA | Analyses exploratoires, graphiques |
| **Majda Bendifi** | P4 — Modeling | Modèles ML, évaluation, résultats |

*Encadré par : **Mme EL ASRI IKRAM***

---

## 📄 Licence

Distribué sous licence MIT. Voir [LICENSE](LICENSE) pour plus de détails.

---

<div align="center">
  <sub>KDD Cup 1999 — De la donnée brute à la détection intelligente 🔐</sub>
</div>
