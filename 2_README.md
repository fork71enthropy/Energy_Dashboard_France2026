# Modélisation Stochastique et Pricing de Risque sur les Marchés Électriques Français

> Projet quantitatif junior — Recherche de stage en finance de l'énergie  
> Étudiant L3 Mathématiques-Informatique | Candidat Master Mathématiques Appliquées

---

## Objectifs

Ce projet applique des outils de mathématiques financières et de modélisation stochastique aux données réelles des marchés électriques français (API RTE eCO2mix). Il ne s'agit pas d'un dashboard de visualisation, mais d'un moteur de modélisation quantitative sur données énergétiques réelles.

Les objectifs sont :

- Modéliser la dynamique stochastique des prix spot de l'électricité
- Pricer des contrats forward sur l'électricité à partir des données historiques
- Estimer le risque de marché par simulation Monte Carlo
- Optimiser un portefeuille de sources énergétiques sous contraintes
- Détecter les régimes de marché et événements extrêmes

---

## Fondements Mathématiques

### 1. Modélisation des Prix Spot — Processus d'Ornstein-Uhlenbeck

L'électricité présente une forte mean-reversion (contrairement aux actions). On modélise le prix spot par :

```
dS(t) = κ(μ - S(t))dt + σ dW(t)
```

où κ est la vitesse de retour à la moyenne, μ le niveau d'équilibre long terme, σ la volatilité instantanée, et W(t) un mouvement brownien standard.

Estimation des paramètres par **maximum de vraisemblance** sur données RTE historiques.

### 2. Modèle à Saut — Gestion des Pics de Prix

L'électricité présente des price spikes extrêmes non capturés par un modèle gaussien. Extension par processus de Merton (jump-diffusion) :

```
dS(t) = κ(μ - S(t))dt + σ dW(t) + J dN(t)
```

où N(t) est un processus de Poisson d'intensité λ et J l'amplitude des sauts (log-normale).

### 3. Pricing de Contrats Forward

Construction de la courbe forward à partir des données spot historiques. Prix forward théorique sous la mesure risque-neutre :

```
F(t,T) = E^Q[S(T) | F_t]
```

Implémentation du **modèle de Schwartz (1997)** à deux facteurs : prix spot + convenience yield stochastique.

### 4. Simulation Monte Carlo

Génération de N scénarios de trajectoires de prix pour :
- Estimer la **Value at Risk (VaR)** et **Expected Shortfall (CVaR)** d'un portefeuille énergétique
- Pricer des options sur électricité (call/put asiatiques, options sur spread)
- Stress-testing sous scénarios extrêmes (vague de froid, arrêt nucléaire)

### 5. Optimisation de Portefeuille Énergétique

Problème d'optimisation sous contraintes (mix solaire, éolien, nucléaire, gaz) :

```
min  w^T Σ w
s.t. w^T μ ≥ r_cible
     w^T 1 = 1
     w_i ≥ 0
     CO2(w) ≤ seuil
```

Résolution par **programmation quadratique** (scipy.optimize, CVXPY).

---

## Architecture Technique

```
energy_quant/
│
├── data/
│   ├── rte_client.py          # Client API RTE eCO2mix
│   ├── preprocessing.py       # Nettoyage, détection outliers, séries temporelles
│   └── storage.py             # TimescaleDB — stockage séries temporelles
│
├── models/
│   ├── ornstein_uhlenbeck.py  # Estimation OU par MLE
│   ├── jump_diffusion.py      # Modèle de Merton avec sauts
│   ├── two_factor.py          # Modèle de Schwartz deux facteurs
│   └── regime_detection.py    # HMM — détection de régimes de marché
│
├── pricing/
│   ├── forward_curve.py       # Construction courbe forward
│   ├── monte_carlo.py         # Moteur de simulation MC (vectorisé numpy)
│   ├── options.py             # Pricing options asiatiques, spread options
│   └── calibration.py         # Calibration aux prix de marché observés
│
├── risk/
│   ├── var_cvar.py            # Value at Risk, Expected Shortfall
│   ├── stress_testing.py      # Scénarios extrêmes
│   └── backtesting.py         # Validation des modèles sur données historiques
│
├── optimization/
│   ├── portfolio.py           # Optimisation Markowitz + contrainte CO2
│   ├── efficient_frontier.py  # Frontière efficiente énergétique
│   └── constraints.py         # Contraintes réglementaires et techniques
│
├── notebooks/
│   ├── 01_exploratory.ipynb   # Analyse statistique des prix spot RTE
│   ├── 02_calibration.ipynb   # Calibration modèles stochastiques
│   ├── 03_monte_carlo.ipynb   # Simulations et pricing
│   └── 04_portfolio.ipynb     # Optimisation de portefeuille
│
└── tests/
    ├── test_models.py          # Tests unitaires modèles stochastiques
    ├── test_pricing.py         # Tests pricing (parité call-put, bornes)
    └── test_optimization.py    # Tests convexité, contraintes
```

---

## Stack Technique

| Composant | Technologie | Justification |
|---|---|---|
| Modélisation | Python (numpy, scipy, statsmodels) | Standard quantitatif |
| Optimisation | CVXPY | Programmation convexe robuste |
| Simulation MC | numpy vectorisé | Performance sur N=100k scénarios |
| Séries temporelles | TimescaleDB | Stockage efficace données tick |
| Calibration | scipy.optimize (L-BFGS-B, Nelder-Mead) | MLE, moindres carrés |
| Machine Learning | sklearn (HMM uniquement) | Détection régimes marché |
| Source de données | API RTE eCO2mix | Données réelles françaises |

---

## Résultats Attendus

- Paramètres calibrés du modèle OU sur données RTE 2020-2024 (κ, μ, σ)
- Courbe forward reconstruite vs prix forward observés (erreur de calibration < 5%)
- Distribution de VaR/CVaR à 95% et 99% sur portefeuille énergétique simulé
- Frontière efficiente d'un mix énergétique sous contrainte CO2
- Détection des régimes (basse/haute volatilité, pics) sur historique RTE

---

## Ce que ce projet démontre

- Maîtrise des **processus stochastiques** appliqués à la finance de l'énergie
- Capacité à **calibrer des modèles** sur données réelles (MLE, moindres carrés)
- Implémentation from scratch d'un **moteur Monte Carlo** vectorisé
- Connaissance des **produits dérivés énergétiques** (forward, options sur spread)
- Application de l'**optimisation mathématique** à un problème industriel réel

---

## Références

- Schwartz, E. (1997). *The Stochastic Behavior of Commodity Prices*. Journal of Finance.
- Lucia, J. & Schwartz, E. (2002). *Electricity Prices and Power Derivatives*. Review of Derivatives Research.
- Geman, H. (2005). *Commodities and Commodity Derivatives*. Wiley Finance.
- Rockafellar & Uryasev (2000). *Optimization of Conditional Value-at-Risk*. Journal of Risk.

---

## Auteur

Étudiant L3 Mathématiques-Informatique — Candidat Master Mathématiques Appliquées  
Recherche stage Finance de l'Énergie / Modélisation Quantitative
