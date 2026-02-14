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