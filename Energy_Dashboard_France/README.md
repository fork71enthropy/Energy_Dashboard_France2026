# Dashboard de consommation d'énergie en France

## 📊 Description
Dashboard interactif affichant en temps réel la production et consommation électrique française, avec analyse du mix énergétique et des émissions CO2.

## 💡 Valeur ajoutée
- **Analyse comparative** : Évolution du mix énergétique sur plusieurs années
- **Insights CO2** : Identification des périodes à forte/faible intensité carbone
- **Tendances** : Détection de patterns de consommation (saisonnalité, pics)


## 🎯 Objectifs
Projet réalisé dans le cadre d'une recherche de stage , démontrant :
- Intégration d'APIs externes (RTE eCO2mix)
- Traitement de données temporelles
- Visualisation de données énergétiques
- Architecture Django full-stack

## 🚀 Technologies
- **Backend** : Django 5.0, Django REST Framework
- **Frontend** : React / Vue.js (ou précise ce que tu utilises)
- **Base de données** : PostgreSQL + TimescaleDB (built on top of PostgreSQL)
- **Visualisation** : Chart.js / Plotly
- **Tâches asynchrones** : Celery + Redis
- **Cache** : Django cache (stockage temporaire 15min, pas de Celery pour MVP pour les tâches aynchrones)

## 📁 Structure du projet
```
energy_dashboard/
├── config/              # Configuration Django
├── apps/
│   ├── core/           # Modèles de base et utilitaires
│   ├── data_collection/# Récupération données RTE
│   ├── analytics/      # Analyses et calculs
│   └── dashboard/      # API REST et vues
└── frontend/           # Application frontend
```

## 🔧 Installation
[À compléter après]

## 📈 Fonctionnalités
- [ ] Mix énergétique temps réel
- [ ] Historique de production par source
- [ ] Estimation émissions CO2
- [ ] Prévisions de consommation

## 📚 Sources de données
- API RTE eCO2mix (https://www.rte-france.com/eco2mix)

## 👤 Auteur
Étudiant M1 mathématiques appliquées - Recherche stage domaine de l'énergie

## Vue d'ensemble du projet avec tous ses fichiers 
```
energy_dashboard/
├── README.md                          # ✅ Vue d'ensemble (celui que tu as déjà)
│
├── docs/
│   ├── README.md                      # Index de toute la documentation
│   ├── installation.md                # Setup complet (PostgreSQL, TimescaleDB, Redis)
│   ├── architecture.md                # Schéma technique + choix techno
│   ├── api.md                         # Documentation endpoints API REST
│   ├── rte-api-integration.md         # Intégration API RTE eCO2mix
│   └── deployment.md                  # Guide déploiement production
│
├── config/
│   └── settings.py
│
├── apps/
│   ├── README.md                      # Vue d'ensemble des 4 apps + interactions
│   │
│   ├── core/
│   │   ├── README.md                  # Modèles de base, utilitaires partagés
│   │   ├── models.py
│   │   └── utils.py
│   │
│   ├── data_collection/
│   │   ├── README.md                  # Récup API RTE, cache 15min, scheduling
│   │   ├── models.py                  # Stockage données brutes
│   │   ├── services.py                # Logique appel API RTE
│   │   └── tasks.py                   # Tâches planifiées (si ajout Celery)
│   │
│   ├── analytics/
│   │   ├── README.md                  # Calculs CO2, agrégations, trends
│   │   ├── models.py                  # Résultats d'analyse
│   │   ├── services.py                # Algorithmes d'analyse
│   │   └── calculations.py            # Formules CO2, mix énergétique
│   │
│   └── dashboard/
│       ├── README.md                  # Endpoints API REST, serializers
│       ├── views.py                   # API REST DRF
│       ├── serializers.py
│       └── urls.py
│
└── frontend/
    └── README.md                      # Setup React/Vue, composants, API calls
```
