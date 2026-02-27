# Analyse Exploratoire des Vols Aériens

_Projet d'EDA sur le pricing des billets d'avion — Python (Jupyter)_

## Contexte et objectif

Ce dépôt contient une **analyse exploratoire complète (EDA)** d'un jeu de données du secteur aérien comportant 10 683 observations de vols commerciaux.

**Objectifs** :
- Nettoyer et structurer les données (valeurs manquantes, outliers, standardisation).
- Comprendre les **facteurs influençant le prix des billets** (compagnie, durée, escales, destination).
- Préparer les données pour la **modélisation prédictive** (régression, classification).
- Fournir des **recommandations opérationnelles** pour optimiser les prix et la fréquence des vols.

## Données

**Source** : Fichier Excel `Data_Train.xlsx`  
**Dimensions** : 10 683 lignes × 11 colonnes

### Variables

| Variable | Description | Type |
|----------|-------------|------|
| `Airline` | Compagnie aérienne (12 compagnies) | Catégorique |
| `Date_of_Journey` | Date du vol | Date |
| `Source` | Ville de départ (Delhi, Cochin, Banglore...) | Catégorique |
| `Destination` | Ville d'arrivée | Catégorique |
| `Route` | Itinéraire détaillé | Catégorique |
| `Dep_Time` | Heure de départ | Date/Heure |
| `Arrival_Time` | Heure d'arrivée | Date/Heure |
| `Duration` | Durée du vol (en minutes) | Numérique |
| `Total_Stops` | Nombre d'escales (0–4) | Numérique |
| `Additional_Info` | Informations complémentaires | Catégorique |
| `Price` | Prix du billet (monnaie locale) | Numérique |

### Statistiques descriptives (après nettoyage)

| Variable | Moyenne | Médiane | Min | Max | Écart-type |
|----------|---------|---------|-----|-----|------------|
| **Prix** | 9 022 | 8 372 | 1 759 | 23 017 | 4 260 |
| **Durée** (min) | 642 | 520 | 5 | 2 070 | — |
| **Escales** | — | 1 | 0 | 2,5 | — |

## Méthodologie

### 1. Nettoyage des données

**Étapes appliquées** :
- **Doublons** : Aucun détecté.
- **Valeurs manquantes** : 1 valeur dans `Route` et `Total_Stops` → suppression (impact négligeable).
- **Types de données** :
  - Conversion dates/heures → `datetime`.
  - Conversion `Duration` → minutes (numérique).
  - Mapping `Total_Stops` : `'non-stop'` → 0, `'1 stop'` → 1, etc.
- **Outliers** : Détection via méthode IQR (intervalle interquartile) → remplacement par les bornes.
- **Standardisation** : Homogénéisation des noms de compagnies, villes, catégories.

### 2. Analyse univariée

**Variables numériques** :
- **Prix** : Distribution asymétrique positive (moyenne > médiane).
- **Durée** : Distribution multimodale (plusieurs pics de fréquence).
- **Escales** : Mode = 1 escale (vols avec 0 ou 1 escale dominants).

**Variables catégorielles** :
- **Compagnie** : 12 compagnies, **Jet Airways** la plus fréquente.
- **Source/Destination** : **Delhi, Cochin, Banglore** dominent.
- **Additional_Info** : Majorité `'No info'` (modalités rares regroupées).

**Visualisations** : Histogrammes, boxplots (avant/après nettoyage), diagrammes en barres.

### 3. Analyse bivariée

#### 3.1 Corrélations numériques

| Paire de variables | Corrélation (r) | Interprétation |
|--------------------|-----------------|----------------|
| Prix × Durée | ≈ 0,45 | Modérée positive : vols longs = plus chers |
| Prix × Escales | ≈ 0,18 | Faible : escales influencent peu le prix |
| Durée × Escales | Positive | Plus d'escales = durée plus longue |

**Visualisation** : Heatmap des corrélations.

#### 3.2 Croisements catégoriels

- **Compagnie × Prix** :
  - **Jet Airways, Air India** : prix moyens élevés.
  - **GoAir, SpiceJet** : prix bas (compagnies low-cost).
- **Destination × Prix** :
  - Vols vers **Cochin, Banglore** : prix moyens plus élevés.
- **Escales × Prix** :
  - Vols **directs (non-stop)** : généralement plus chers.

#### 3.3 Tests statistiques

- **ANOVA** (Prix selon compagnie) : différence significative (p < 0,01).
- **Kruskal-Wallis** (Prix selon escales) : différence significative (p < 0,01).

### 4. Tendances temporelles

- **Avril** : vols moins fréquents, prix plus bas.
- **Mars, mai, juin** : prix élevés (haute saison).
- **Horaires** : matin et après-midi = créneaux de départ les plus fréquents.

## Structure du dépôt

```text
.
├── data/
│   └── Data_Train.xlsx            # Données brutes
├── notebooks/
│   └── Analyse_Exploratoire_Complete.ipynb  # Notebook EDA complet
├── reports/
│   └── Rapport-analyse-exploratoire-secteur-aerien.pdf
├── figures/
│   ├── boxplots_prix_avant_apres.png
│   ├── heatmap_correlations.png
│   ├── barplot_compagnies.png
│   ├── prix_par_destination.png
│   └── distribution_escales.png
├── requirements.txt
└── README.md
