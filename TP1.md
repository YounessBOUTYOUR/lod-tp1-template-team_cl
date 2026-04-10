# TP1 - Introduction aux données ouvertes liées

## Positionnement du TP

Ce premier TP introduit les principes des **Linked Open Data** à partir d'une approche métier, analytique et conceptuelle.

Vous n'allez pas encore produire de RDF ni écrire de triplets. Votre travail consiste à :

- analyser un jeu de données ouvert
- identifier ce qui pourrait devenir des ressources
- repérer des identifiants et des liens potentiels
- montrer comment ce jeu de données pourrait évoluer vers des données liées
- construire un premier diagnostic d'ingénierie des données exploitable pour la suite du module

Les aspects formels de RDF, Turtle et SPARQL seront traités dans les prochaines séances.

## Contexte

Une organisation publie ses données sous forme tabulaire sur un portail Open Data. Ces données sont ouvertes et réutilisables, mais elles restent difficiles à connecter à d'autres sources. Votre mission consiste à évaluer si ce jeu de données peut servir de base à une future publication en **données ouvertes liées**.

Vous travaillez comme une petite équipe d'ingénierie des données chargée d'auditer un jeu de données avant sa transformation future en graphe de données. À l'issue du TP, vous devez fournir un diagnostic technique, une proposition de mise en relation et une stratégie de préparation au LOD.

## Objectifs pédagogiques

En fin de séance, vous devez être capables de :

- expliquer la différence entre `Open Data`, `Linked Data` et `Linked Open Data`
- décrire l'intérêt d'un identifiant stable pour une ressource
- évaluer la qualité d'un jeu de données pour un usage futur en LOD
- identifier des entités centrales et leurs relations conceptuelles
- proposer des liens vers des ressources externes pertinentes
- distinguer entités, attributs, valeurs contrôlées et identifiants
- construire une grille de maturité et de qualité
- produire un rapport technique synthétique et argumenté

## Organisation

- **Durée** : `2 heures`
- **Mode** : binôme 
- **Livrable final** : dossier GitHub par groupe
- **Travail hors séance** : voir [`TRAVAIL_A_RENDRE.md`](./TRAVAIL_A_RENDRE.md)

## Niveau attendu

Ce TP requiert une démarche d'analyse technique rigoureuse :

- ne pas vous limiter à décrire le dataset
- expliciter les critères utilisés pour juger sa qualité
- justifier les liens externes proposés
- formuler des hypothèses raisonnables en les annonçant clairement
- identifier les risques de confusion, d'ambiguïté et de duplication

## Jeu de données

Deux options sont possibles :

1. utiliser le jeu de données simplifié fourni dans [`data/higher_education_sample.csv`](./data/higher_education_sample.csv)
2. choisir un jeu de données ouvert réel sur un portail public, à condition qu'il soit :
   - clairement documenté
   - exploitable
   - assez riche pour identifier plusieurs entités et liens potentiels
   - suffisamment structuré pour permettre une évaluation technique sérieuse

Si vous choisissez un jeu externe, indiquez explicitement sa source et son URL dans votre rapport.

Dans tous les cas, vous devez mobiliser **au moins deux référentiels ou sources externes** pour vos propositions de liaison, par exemple `Wikidata`, `GeoNames`, `DBpedia`, `data.gouv.fr`, `data.gov.ma`, un thésaurus métier ou un référentiel institutionnel.

## Déroulement suggéré

### Partie 1 - Cadrage et découverte (`20 min`)

Lisez rapidement :

- ce sujet
- [`resources/links.md`](./resources/links.md)
- la description du jeu de données choisi

Puis répondez aux questions suivantes dans le groupe :

- Quelle différence faites-vous entre une donnée ouverte et une donnée liée ?
- Pourquoi un simple fichier CSV ne suffit-il pas pour faciliter l'interconnexion de données ?
- Quels types d'entités réelles apparaissent déjà dans le jeu de données ?
- Quelle est l'unité d'observation du dataset et quel est son niveau de granularité ?
- Quels champs pourraient jouer le rôle d'identifiant ou de clé candidate ?

### Partie 2 - Analyse du jeu de données (`30 min`)

Complétez la fiche `01_fiche_analyse_dataset.md` à partir du gabarit fourni.

Vous devez notamment analyser :

- le producteur du jeu de données
- le domaine métier
- le format de publication
- la présence ou l'absence d'identifiants stables
- la granularité des enregistrements
- la présence d'attributs réutilisables pour le lien vers d'autres sources
- la qualité apparente des données
- les freins à une publication future en LOD
- la maturité du jeu de données selon le modèle `5 étoiles`
- les clés candidates, les doublons potentiels et les besoins de normalisation

Vous devez attribuer un **score argumenté** à plusieurs dimensions de qualité :

- accessibilité
- documentation
- réutilisabilité
- stabilité des identifiants
- qualité sémantique
- potentiel de liaison externe

### Partie 3 - Identification des entités et des liens potentiels (`35 min`)

Complétez `02_carte_des_liens.md`.

Travail demandé :

- identifier au moins `8` entités ou types d'entités
- distinguer les entités principales des attributs simples et des valeurs contrôlées
- proposer au moins `10` liens ou correspondances conceptuelles
- proposer au moins `5` liens externes cibles répartis sur **au moins deux référentiels**
- identifier, pour au moins `3` types d'entités, un identifiant local potentiel ou un besoin d'identifiant stable
- pour chaque lien, préciser :
  - l'entité locale concernée
  - la ressource externe ciblée
  - le type de correspondance envisagé
  - les critères d'appariement retenus
  - le bénéfice attendu
  - le niveau de confiance
  - le risque d'erreur ou d'ambiguïté

Vous devez aussi produire un schéma conceptuel simple du jeu de données. Ce schéma peut être :

- un diagramme dessiné puis exporté
- un schéma Mermaid
- un schéma réalisé avec draw.io

Le schéma doit rester **conceptuel**, mais il doit être suffisamment rigoureux pour distinguer :

- les entités
- les relations
- les attributs descriptifs
- les identifiants existants ou à créer
- les points de liaison vers l'extérieur

### Partie 4 - Préparation du travail hors séance (`25 min`)

Avant la fin de la séance, chaque groupe doit :

- choisir définitivement son jeu de données
- terminer une première version de la fiche d'analyse
- terminer une première version de la carte des entités et des liens
- identifier les lacunes de normalisation à traiter avant le TP2
- esquisser une stratégie d'identification pour les entités principales
- choisir les types d'entités qui feront l'objet du travail hors séance
- sélectionner au moins deux référentiels externes à comparer hors séance

Le travail hors séance doit être pensé comme une **étude complémentaire distincte**, et non comme une simple finition des documents de séance.

### Partie 5 - Finalisation du dépôt (`10 min`)

À la fin de la séance, votre dépôt doit déjà contenir au minimum :

- `deliverables/Groupe-XX/seance/01_fiche_analyse_dataset.md`
- `deliverables/Groupe-XX/seance/02_carte_des_liens.md`

Les livrables du travail hors séance seront ajoutés avant la prochaine séance.

## Questions auxquelles vous devez répondre

Ces questions doivent apparaître dans vos livrables ou dans votre rapport :

1. Quelle est la différence entre `Open Data` et `Linked Open Data` ?
2. Quels éléments du jeu de données peuvent être considérés comme des entités centrales ?
3. Le jeu de données contient-il des identifiants stables et exploitables ?
4. Quelles ressources externes pourraient enrichir ce jeu de données ?
5. Quels sont les principaux obstacles techniques ou sémantiques à une publication en données liées ?
6. Quel serait le bénéfice concret d'une mise en relation avec `Wikidata`, `DBpedia`, `GeoNames` ou un autre référentiel ?
7. Quelle est la granularité exacte du dataset et quel impact cela a-t-il sur une future modélisation ?
8. Quelles valeurs devraient être normalisées avant toute transformation en données liées ?
9. Quels champs pourraient servir de clé candidate locale, et avec quelles limites ?
10. Quel risque de faux appariement existe pour les liens externes proposés ?

## Livrables attendus

### 1. Fiche d'analyse du dataset

Elle doit contenir au minimum :

- la source du jeu de données
- l'objectif du jeu de données
- le format
- la licence, si disponible
- les principales colonnes ou champs
- l'unité d'observation et la granularité
- les clés candidates et leurs limites
- une première évaluation de la qualité
- un score de maturité et de préparation au LOD
- le potentiel d'évolution vers le LOD

### 2. Carte des entités et des liens

Elle doit contenir :

- un inventaire des entités
- un schéma conceptuel
- un tableau des liens externes proposés
- une distinction claire entre entités, attributs, codes et valeurs
- une première stratégie d'identification pour les entités majeures

## Travail à rendre avant la prochaine séance

Ce travail est **obligatoire**. Son énoncé est séparé du TP principal et se trouve dans [`TRAVAIL_A_RENDRE.md`](./TRAVAIL_A_RENDRE.md).

Il doit être déposé avant le début du **TP2**, selon la modalité fixée par l'enseignant.

## Barème indicatif

- compréhension des notions de base et rigueur conceptuelle : `3/20`
- qualité de l'audit du jeu de données réalisé en séance : `4/20`
- pertinence des entités, de la granularité et des identifiants proposés : `4/20`
- qualité du benchmark des référentiels et de la stratégie d'appariement : `4/20`
- qualité du plan de normalisation hors séance : `3/20`
- analyse critique des limites et des risques : `2/20`
- qualité du rapport et de l'argumentation : `2/20`

## Ce qu'il ne faut pas faire

- ne pas écrire de triplets RDF
- ne pas produire une ontologie formelle
- ne pas se limiter à recopier la description du portail Open Data
- ne pas proposer des liens externes sans justification
- ne pas confondre une valeur textuelle et une entité du monde réel
- ne pas proposer des identifiants "stables" sans expliquer leur construction

