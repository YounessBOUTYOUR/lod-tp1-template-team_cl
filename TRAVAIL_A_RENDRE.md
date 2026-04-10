# Travail à rendre - Étude de cas réelle

## Intitulé

**Étude de cas : préparation à l'ouverture liée du jeu de données des établissements de l'enseignement supérieur universitaire public au Maroc (année 2024-2025)**

## Contexte

Le travail à rendre après le TP1 porte sur une **étude de cas réelle**. Il ne s'agit pas d'une simple reprise des productions réalisées en séance, mais d'une étude complémentaire fondée sur un jeu de données public effectivement publié.

### Source retenue

- **Jeu de données** : *Etablissements de l'enseignement supérieur universitaire public ouverts au titre de l'année 2024-2025*
- **Producteur** : MESRSI
- **Portail** : Données ouvertes du Maroc
- **URL** : `https://data.gov.ma/data/dataset/etablissements-de-l-enseignement-superieur-universitaire-public-ouverts/resource/fb9c2300-77f7-461c-86a5-9184b00e57d9?view_id=2122a95d-30fe-46e4-b4b8-f3d4cd835cf7`
- **Ressource** : `Universités Publiques 2024-2025.xlsx`
- **Format** : `XLSX`
- **Licence** : `ODbL`
- **Dernière mise à jour des données** : `19 février 2025`

### Informations utiles

D'après le portail Open Data du Maroc, ce jeu de données :

- présente les établissements de l'enseignement supérieur universitaire public ouverts au titre de l'année `2024-2025`
- inclut `165` établissements affiliés à `12` universités
- comprend `162` établissements de formation et `3` instituts de recherche scientifique

## Objectif du travail

Votre objectif est d'évaluer dans quelle mesure ce jeu de données réel peut être préparé pour une future publication en **données ouvertes liées**.

Vous devez adopter une démarche d'analyse technique et répondre à trois questions :

1. Quelles sont les entités centrales de ce jeu de données ?
2. Quels référentiels externes seraient les plus pertinents pour l'enrichir ou l'aligner ?
3. Quelles étapes de normalisation et d'identification seraient nécessaires avant une modélisation RDF ?

## Travail demandé

Le travail à rendre comporte trois volets.

### Volet 1 - Benchmark de référentiels externes

À partir du jeu de données réel, sélectionnez `2 à 3` types d'entités prioritaires, par exemple :

- établissement
- université
- ville
- région

Puis :

- comparez au moins `2` référentiels externes pour chacun des types d'entités retenus
- documentez au moins `6` cas d'appariement manuel
- justifiez les critères d'appariement utilisés
- explicitez les ambiguïtés rencontrées

### Volet 2 - Plan de normalisation et d'identification

Proposez un plan de préparation du jeu de données en vue d'une future transformation en graphe de données.

Vous devez notamment :

- identifier les champs à normaliser en priorité
- proposer des règles de nettoyage ou de standardisation
- repérer les champs susceptibles de devenir des identifiants ou de servir à la construction d'URI
- signaler les limites de cette stratégie

### Volet 3 - Rapport de synthèse

Rédigez un rapport structuré montrant :

- votre compréhension du cas étudié
- les référentiels comparés
- les cas d'appariement les plus significatifs
- la stratégie de normalisation et d'identification retenue
- les risques de faux appariement
- l'intérêt de cette préparation pour le TP2

## Livrables attendus

Le rendu doit être déposé dans :

- `deliverables/Groupe-XX/hors_seance/01_benchmark_referentiels.md`
- `deliverables/Groupe-XX/hors_seance/02_plan_normalisation.md`
- `deliverables/Groupe-XX/hors_seance/03_rapport_hors_seance.pdf`

## Contraintes

- le travail doit porter sur le **cas réel imposé** ci-dessus
- il ne doit pas se limiter à reformuler les documents produits pendant la séance
- les cas d'appariement doivent être justifiés
- les hypothèses doivent être signalées explicitement
- aucun triplet RDF n'est demandé à ce stade

## Critères d'évaluation

- pertinence de l'analyse du jeu de données réel
- qualité du benchmark des référentiels
- rigueur du plan de normalisation
- qualité de l'argumentation
- clarté du rapport

## Gabarits à utiliser

Pour ce travail, utilisez les fichiers suivants :

- [`templates/benchmark_referentiels.md`](./templates/benchmark_referentiels.md)
- [`templates/plan_normalisation.md`](./templates/plan_normalisation.md)
- [`templates/rapport_hors_seance_template.md`](./templates/rapport_hors_seance_template.md)
