# ElectricityPricing_FR

> Plateforme fullstack de modélisation quantitative des prix spot de l'électricité française  
> Candidat Master Mathématiques Appliquées — Recherche stage Finance de l'Énergie

---

## Vision du Projet

Ce projet est une plateforme fullstack qui combine ingénierie logicielle et mathématiques financières appliquées aux marchés électriques français. Le backend collecte et stocke les données RTE en temps réel, le moteur quantitatif calibre un modèle stochastique de mean-reversion sur l'historique des prix spot, et le frontend expose les résultats via une interface interactive.

Ce n'est pas un dashboard de visualisation — c'est un moteur de modélisation qui tourne sur des données réelles.

---

## Fondements Mathématiques

### Modèle d'Ornstein-Uhlenbeck (Mean-Reversion)

L'électricité présente une forte mean-reversion — contrairement aux actions, les prix reviennent vers un niveau d'équilibre. On modélise le prix spot par :

```
dS(t) = κ(μ - S(t))dt + σ dW(t)
```

où κ est la vitesse de retour à la moyenne, μ le niveau d'équilibre long terme, σ la volatilité instantanée, et W(t) un mouvement brownien standard.

Estimation des paramètres par **maximum de vraisemblance (MLE)** sur données RTE historiques.

### Simulation Monte Carlo

Une fois les paramètres calibrés, on génère N trajectoires de prix simulées pour :
- Estimer la distribution des prix futurs
- Calculer une **Value at Risk (VaR) à 95%** sur un horizon donné
- Visualiser les intervalles de confiance sur les scénarios de prix

```
S(t+dt) = S(t) + κ(μ - S(t))dt + σ√dt · ε,   ε ~ N(0,1)
```

---

## Architecture Fullstack

```
ElectricityPricing_FR/
│
├── backend/                           # Django REST Framework
│   ├── config/
│   │   └── settings.py
│   ├── apps/
│   │   ├── data_collection/
│   │   │   ├── rte_client.py          # Client API RTE eCO2mix
│   │   │   ├── preprocessing.py       # Nettoyage, détection outliers
│   │   │   └── tasks.py               # Collecte automatique (cron)
│   │   ├── models_quant/
│   │   │   ├── ornstein_uhlenbeck.py  # Calibration MLE (numpy/scipy)
│   │   │   └── monte_carlo.py         # Simulation N trajectoires
│   │   └── api/
│   │       ├── views.py               # Endpoints REST
│   │       ├── serializers.py
│   │       └── urls.py
│   └── manage.py
│
├── frontend/                          # React + Plotly.js
│   ├── src/
│   │   ├── components/
│   │   │   ├── PriceChart.jsx         # Prix spot historiques
│   │   │   ├── ModelParams.jsx        # Affichage κ, μ, σ calibrés
│   │   │   └── MonteCarloChart.jsx    # Trajectoires simulées + VaR
│   │   └── App.jsx
│   └── package.json
│
├── notebooks/
│   ├── 01_exploratory.ipynb           # Analyse statistique prix spot RTE
│   └── 02_calibration.ipynb           # Calibration OU + validation
│
└── tests/
    ├── test_rte_client.py
    ├── test_ornstein_uhlenbeck.py
    └── test_monte_carlo.py
```

---

## Stack Technique

| Composant | Technologie | Rôle |
|---|---|---|
| Backend | Django · DRF | API REST, collecte données |
| Base de données | PostgreSQL · TimescaleDB | Stockage séries temporelles |
| Modélisation | NumPy · SciPy · Statsmodels | Calibration MLE, simulation MC |
| Frontend | React · Plotly.js | Visualisation interactive |
| Source de données | API RTE eCO2mix | Prix spot français temps réel |

---

## Roadmap — 8 Semaines

### Semaine 1 — Collecte et Stockage des Données
- [ ] Connexion API RTE eCO2mix et récupération des prix spot historiques
- [ ] Modèle Django + TimescaleDB pour stocker les séries temporelles
- [ ] Script de collecte automatique quotidienne

### Semaine 2 — Analyse Exploratoire
- [ ] Notebook `01_exploratory.ipynb` : statistiques descriptives des prix spot
- [ ] Visualisation saisonnalité, autocorrélation, détection des pics
- [ ] Vérification de la stationnarité (test ADF)

### Semaine 3 — Calibration du Modèle OU
- [ ] Implémentation de la log-vraisemblance du processus OU
- [ ] Estimation des paramètres κ, μ, σ par scipy.optimize
- [ ] Notebook `02_calibration.ipynb` : résultats et validation du modèle

### Semaine 4 — Simulation Monte Carlo
- [ ] Moteur de simulation vectorisé (numpy) — N=10 000 trajectoires
- [ ] Calcul de la VaR à 95% sur horizon 7 jours
- [ ] Tests unitaires sur la simulation

### Semaine 5 — API REST Backend
- [ ] Endpoints DRF : prix historiques, paramètres calibrés, scénarios MC
- [ ] Serializers et documentation des endpoints
- [ ] Tests d'intégration API

### Semaine 6 — Frontend React
- [ ] Composant `PriceChart` : prix spot historiques (Plotly.js)
- [ ] Composant `ModelParams` : affichage κ, μ, σ avec interprétation
- [ ] Connexion frontend ↔ API backend

### Semaine 7 — Visualisation Monte Carlo
- [ ] Composant `MonteCarloChart` : N trajectoires simulées + intervalles de confiance
- [ ] Affichage VaR 95% sur le graphique
- [ ] Contrôle interactif de l'horizon de simulation

### Semaine 8 — Finalisation
- [ ] Tests end-to-end
- [ ] Déploiement (AWS EC2 ou Railway)
- [ ] README final + documentation des résultats

---

## État d'avancement

- [x] Initialisation du projet Django
- [ ] Connexion API RTE eCO2mix
- [ ] Stockage TimescaleDB
- [ ] Calibration Ornstein-Uhlenbeck *(semaine 3)*
- [ ] Simulation Monte Carlo *(semaine 4)*
- [ ] API REST complète *(semaine 5)*
- [ ] Frontend React *(semaines 6-7)*
- [ ] Déploiement *(semaine 8)*

---

## Références

- Schwartz, E. (1997). *The Stochastic Behavior of Commodity Prices*. Journal of Finance.
- Lucia, J. & Schwartz, E. (2002). *Electricity Prices and Power Derivatives*. Review of Derivatives Research.

---

## Auteur

Candidat Master Mathématiques Appliquées  
Recherche stage Finance de l'Énergie / Modélisation Quantitative
