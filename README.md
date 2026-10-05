<div align="center">

![Version](https://img.shields.io/badge/version-3.1.0-000091?style=for-the-badge)
![Statut](https://img.shields.io/badge/statut-production-16a34a?style=for-the-badge)
![Licence](https://img.shields.io/badge/licence-CC%20BY--NC%204.0-E1000F?style=for-the-badge)

![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)
![GDAL](https://img.shields.io/badge/GDAL-3.8.4-5CAE58?style=for-the-badge)
![QGIS](https://img.shields.io/badge/QGIS-3.x-589632?style=for-the-badge&logo=qgis&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES2022-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

![IFREMER](https://img.shields.io/badge/IFREMER-HYSCORES-000091?style=for-the-badge)
![La Réunion](https://img.shields.io/badge/🇷🇪-La%20Réunion-000091?style=for-the-badge)
![Open Data](https://img.shields.io/badge/Open%20Data-data.gouv.fr-000091?style=for-the-badge)

![Datasets](https://img.shields.io/badge/datasets-13-16a34a?style=flat-square)
![Rasters](https://img.shields.io/badge/rasters-7-16a34a?style=flat-square)
![Surface](https://img.shields.io/badge/surface-290%20km²-fbbf24?style=flat-square)
![Résolution](https://img.shields.io/badge/résolution-40%20cm-fbbf24?style=flat-square)

</div>

---

<h1 align="center">🔬 Monitor IFREMER Récifs Pro</h1>

<p align="center">
  <strong>Système d'analyse scientifique des récifs coralliens de La Réunion</strong><br>
  <em>Vitalité Corallienne Hyperspectrale (VCH) · 2009-2015 · IFREMER / HYSCORES / Spectrhabent-OI</em>
</p>

---

## 🌊 Présentation

**Monitor IFREMER Récifs Pro** est une plateforme complète d'analyse scientifique des récifs coralliens de la côte ouest de La Réunion. Elle combine :

- 🛰️ **Données hyperspectrales aéroportées** (IFREMER, 40 cm de résolution)
- 🐍 **Scripts Python** pour l'analyse raster et vectorielle
- 📊 **Dashboard HTML interactif** avec 12 vues spécialisées
- 🗺️ **Intégration QGIS** pour la visualisation cartographique
- 🏛️ **Templates DCE** pour le reporting réglementaire

Le projet exploite les campagnes **HYSCORES** (2015-2016) et **Spectrhabent-OI** (2009-2012), couvrant **~290 km²** de récifs sur 4 secteurs : Saint-Gilles, Saint-Leu, Étang-Salé et Saint-Pierre.

---

## ✨ Fonctionnalités

### 🎯 Analyses scientifiques

| Indicateur | Description | Formule |
|---|---|---|
| **VCH** | Vitalité Corallienne Hyperspectrale | `corail vivant / (corail + algues)` |
| **ASA** | Algae Surface Area | `algues / (algues + sable)` |
| **Entités coralliennes** | Seuil珊瑚 > 30 % | Classification binaire |
| **Ratio C/A** | Compétition benthique | `corail / (corail + algues)` |
| **Dominance** | Santé corallienne | `VCH × fraction_corail` |
| **Stress** | Pression algale | `fraction_algues × (1 − VCH/100)` |

### 📊 Dashboard 12 vues

1. 📊 Vue d'ensemble — Comparaison des scénarios
2. 🎯 Scénario actif — Procédure experte détaillée
3. 🗂️ Datasets — 13 datasets IFREMER scorés
4. 🔬 Analyses — Résultats scientifiques
5. 📐 Indicateurs — 6 indicateurs documentés
6. 🔲 Matrice — Croisement années × thèmes
7. 📅 Timeline — Chronologie des acquisitions
8. 📈 Évolution — Courbes VCH 2009-2015
9. 📊 Statistiques — Agrégations multi-axes
10. 🏷️ Tags — Exploration sémantique
11. 🏛️ DCE — Rapport réglementaire
12. ⚠️ Avertissements — Incertitudes documentées

---

## 🔧 Pipeline complet

Le pipeline s'articule en **7 étapes** séquentielles, de l'acquisition des données brutes à la visualisation interactive.

```
┌─────────────────────────────────────────────────────────────┐
│  1. DONNÉES BRUTES (SExtant / data.gouv.fr)                 │
│     → 13 datasets JSON · 7 rasters GeoTIFF                  │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│  2. TÉLÉCHARGEMENT AUTOMATISÉ                               │
│     → crawler_sextant.py                                    │
│     → 7 rasters (9.1 GB)                                    │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│  3. VÉRIFICATION GDAL                                       │
│     → gdalinfo · verifier_2fichiers.py                      │
│     → Intégrité + CRS + résolution                          │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│  4. ANALYSE RASTER                                          │
│     → analyser_rasters.py · analyse_stack.py                │
│     → VCH 2009/2015 · ΔVCH · classifications                │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│  5. GÉNÉRATION JSON ENRICHI                                 │
│     → exporter_dashboard.py                                 │
│     → reunion_974_analyses_completes.json                   │
└─────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────┐
│  6. VISUALISATION                                           │
│     → Dashboard HTML (12 vues)                              │
│     → QGIS (7 rasters GeoTIFF)                              │
└─────────────────────────────────────────────────────────────┘
```

### Détail des 7 étapes

**Étape 1 — Acquisition** : Récupération des 13 datasets JSON (métadonnées) + 7 rasters GeoTIFF (données brutes) depuis data.gouv.fr et SExtant IFREMER.

**Étape 2 — Vérification** : Contrôle d'intégrité GDAL (driver, dimensions, CRS, valeurs min/max). Détection des fichiers corrompus.

**Étape 3 — Prétraitement** : Génération des pyramides (overviews) pour accélérer l'affichage QGIS. Conversion optionnelle en COG.

**Étape 4 — Analyse** : Extraction des statistiques VCH, classifications par secteur, évolution 2009-2015. Dénormalisation `VCH_% = ((valeur + 1) / 2) × 100`.

**Étape 5 — Synthèse** : Fusion des métadonnées et analyses en un JSON enrichi unique.

**Étape 6 — Export** : Génération des livrables CSV, JSON, HTML, Rapport DCE.

**Étape 7 — Visualisation** : Dashboard HTML interactif + QGIS pour l'analyse cartographique.

---

## 🏗️ Architecture

```
monitor-ifremer-recifs-pro/
├── index.html                          # Dashboard 12 vues
├── requirements.txt                    # Dépendances Python
├── README.md                           # Documentation
├── LICENSE                             # CC BY-NC 4.0
├── .gitignore                          # Exclusions Git
│
├── data/                               # 7 rasters (9.1 GB)
│   ├── 01_enticor_stack_2009_2015.tif
│   ├── 02_enticor_diff_2009_2015.tif
│   ├── 03_vch_2009_brut.tif
│   ├── classification_saint_gilles_2015.tif
│   ├── classification_saint_leu_2015.tif
│   ├── classification_etang_sale_2015.tif
│   └── classification_saint_pierre_2015.tif
│
├── scripts/                            # 9 scripts Python
│   ├── enrichir.py                     # Étape 1
│   ├── telecharger_v2.py               # Étape 1
│   ├── analyser_ressources.py          # Étape 1
│   ├── crawler_sextant.py              # Étape 1
│   ├── verifier_2fichiers.py           # Étape 2
│   ├── analyser_rasters.py             # Étape 4
│   ├── analyse_finale.py               # Étape 4
│   ├── analyse_stack.py                # Étape 4
│   └── exporter_dashboard.py           # Étape 5-6
│
├── output/                             # Résultats
│   ├── rapport_analyse.json
│   ├── analyse_finale.json
│   ├── analyse_stack.json
│   └── reunion_974_analyses_completes.json
│
└── docs/                               # Documentation
    ├── METHODOLOGIE.md
    ├── INCERTITUDES.md
    └── DCE_TEMPLATE.md
```

---

## 📊 Résultats

### VCH 2009-2015 (Saint-Gilles)

| Métrique | 2009 | 2015 | Δ |
|---|---|---|---|
| **VCH moyen** | 57.1% | 56.6% | **-0.52** |
| **VCH médian** | 52.3% | 51.5% | -0.80 |
| **Écart-type** | 12.4% | 12.3% | -0.10 |

### Distribution du changement

| Catégorie | % |
|---|---|
| **Gain** (+2 pts) | 24.1% |
| **Stable** (±2 pts) | 49.2% |
| **Perte** (-2 pts) | 26.8% |

### Classifications par secteur (2015)

| Secteur | Surface | Récif | Corail vivant |
|---|---|---|---|
| **Saint-Gilles** | 96.96 km² | 2.85% | 1.53% |
| **Saint-Leu** | 15.09 km² | 4.5% | **2.93%** |
| **Étang-Salé** | 3.14 km² | 7.0% | 1.82% |
| **Saint-Pierre** | 7.83 km² | 5.3% | 1.49% |

### Synthèse globale

- 🗺️ **Surface récifale totale** : 4.08 km²
- 🐠 **Corail vivant total** : 2.168 km²
- 🏆 **Meilleur secteur** : Saint-Leu
- 📉 **Tendance** : **stable** (2009-2015)

---

## 🚀 Installation

### Prérequis

- **OS** : Ubuntu 22.04+ / Debian 12+
- **Python** : 3.10+
- **Espace disque** : 15 GB minimum
- **RAM** : 8 GB recommandé

### 1. Cloner le dépôt

```bash
git clone https://github.com/gunout/monitor-ifremer-recifs-pro.git
cd monitor-ifremer-recifs-pro
```

### 2. Installer les dépendances système

```bash
sudo apt update
sudo apt install -y \
  gdal-bin \
  python3-gdal \
  python3-numpy \
  python3-pip \
  python3-venv \
  wget \
  curl \
  qgis
```

### 3. Créer l'environnement Python

```bash
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Vérifier l'installation

```bash
gdalinfo --version
python3 -c "import rasterio; print('✅', rasterio.__version__)"
python3 -c "import geopandas; print('✅', geopandas.__version__)"
```

---

## 📖 Utilisation

### 1. Télécharger les données

```bash
python3 scripts/crawler_sextant.py --download --priorite 2
```

Cela télécharge **7 rasters** (~10 GB) depuis SExtant IFREMER.

### 2. Lancer les analyses

```bash
python3 scripts/analyser_rasters.py
python3 scripts/analyse_stack.py
python3 scripts/exporter_dashboard.py
```

Cela génère `output/reunion_974_analyses_completes.json`.

### 3. Ouvrir le dashboard

```bash
open index.html
```

Puis **glissez** `output/reunion_974_analyses_completes.json` dans la zone de dépôt.

### 4. Visualiser dans QGIS

```bash
qgis data/*.tif &
```

---

## ⚠️ Incertitudes documentées

| Source | Amplitude | Impact |
|---|---|---|
| **Décote marégraphique 2015** | ±15 points VCH | Sous-estimation possible |
| **Géoréférencement** | ±10 % longueur | Faux positifs |
| **Seuils VCH** | Non quantifiée | Classification abusive |
| **Licence** | `notspecified` | Usage juridique incertain |

### Précautions d'interprétation

- ❌ **Ne pas** attribuer causalement sans contrôle des covariables
- ❌ **Ne pas** interpréter ΔVCH > +70 % comme "restauration réussie"
- ❌ **Ne pas** utiliser VCH comme indicateur unique de conformité DCE
- ✅ **Toujours** mentionner les incertitudes
- ✅ **Toujours** croiser avec d'autres indicateurs (DCE I, CORRAM)

---

## 🎯 Scénarios d'usage

| Scénario | Datasets | Risque | Recommandation |
|---|---|---|---|
| **Bilan 2009-2015** | Polyligne + VCH agrégé | Faible | ✅ **Recommandé** |
| **Analyse écologique fine** | Fractions + VCH surfacique | Moyen | ✅ Recommandé |
| **Détection de changement** | Raster différentiel + morphologie | Moyen | ⚠️ Avec précautions |
| **Reporting DCE** | Polyligne + VCH + incertitudes | Faible | ✅ Officiel |
| **ΔVCH brut** | Différentiel VCH seul | Élevé | 🚫 **À éviter** |

---

## 🛠️ Technologies

| Catégorie | Outil | Version |
|---|---|---|
| **Langage** | Python | 3.12 |
| **SIG** | GDAL | 3.8.4 |
| **SIG** | QGIS | 3.x |
| **Analyse raster** | rasterio | 1.5.2 |
| **Calcul** | numpy | 2.5.3 |
| **Dataframes** | pandas | 3.0.6 |
| **Vectoriel** | geopandas | 1.2.0 |
| **Visualisation** | matplotlib | 3.11.2 |
| **Image** | scikit-image | 0.26.0 |
| **Frontend** | JavaScript | ES2022 |

---

## 🤝 Contribution

Les contributions sont les bienvenues !

1. **Fork** le dépôt
2. Créez une **branche** (`git checkout -b feature/nouvelle-analyse`)
3. **Committez** (`git commit -m 'Ajout analyse X'`)
4. **Pushez** (`git push origin feature/nouvelle-analyse`)
5. Ouvrez une **Pull Request**

### Guidelines

- Suivre la **PEP 8** pour Python
- Documenter les fonctions avec **docstrings**
- Ajouter des **tests** pour les nouvelles fonctionnalités
- Mettre à jour le **README** si nécessaire

---

## 📜 Licence

Ce projet est distribué sous licence **Creative Commons BY-NC 4.0**.

- ✅ **Utilisation** : Recherche, éducation, usage personnel
- ❌ **Interdiction** : Usage commercial sans autorisation
- 📝 **Attribution** : Mentionner l'auteur et la source

**⚠️ Note importante** : Les données sources (IFREMER) sont sous licence `notspecified`. Vérifiez les conditions d'usage auprès de l'IFREMER avant toute redistribution.

---

## 🙏 Remerciements

- **IFREMER** pour les données HYSCORES et Spectrhabent-OI
- **data.gouv.fr** pour le portail Open Data
- **SExtant** pour l'infrastructure de diffusion
- **Communauté QGIS** pour les outils SIG
- **Communauté Python** pour rasterio, geopandas, numpy

---

## 📞 Contact

- **Auteur** : [@gunout](https://github.com/gunout)
- **Issues** : [GitHub Issues](https://github.com/gunout/monitor-ifremer-recifs-pro/issues)
- **Discussions** : [GitHub Discussions](https://github.com/gunout/monitor-ifremer-recifs-pro/discussions)

---

## 🔗 Liens utiles

- [IFREMER](https://www.ifremer.fr/)
- [HYSCORES](https://www.ifremer.fr/hyscores)
- [SExtant Ocean Indien](https://sextant.ifremer.fr/fr/web/ocean_indien)
- [data.gouv.fr](https://www.data.gouv.fr/)
- [Documentation GDAL](https://gdal.org/)
- [QGIS](https://qgis.org/)

---

<div align="center">

**⭐ Si ce projet vous est utile, n'oubliez pas de lui donner une étoile ! ⭐**

![Made with ❤️ in La Réunion](https://img.shields.io/badge/Made%20with%20❤️%20in-La%20Réunion-000091?style=for-the-badge)

</div>

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
