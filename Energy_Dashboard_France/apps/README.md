# Applications Django

## Architecture
- **core** : Modèles de base (TimeStampedModel, utilitaires)
- **data_collection** : Récupération API RTE + cache
- **analytics** : Calculs CO2, agrégations temporelles
- **dashboard** : API REST pour le frontend

## Flux de données
1. data_collection récupère depuis RTE
2. analytics traite et calcule
3. dashboard expose via API REST
4. frontend consomme l'API

