# Plan de normalisation et d'identification

## Identification

- **Groupe :** team_cl
- **Jeu de données :** *Établissements de l'enseignement supérieur universitaire public ouverts au titre de l'année 2024-2025* — MESRSI / data.gov.ma
- **Contexte :** 165 établissements affiliés à 12 universités publiques, format XLSX, licence ODbL, mise à jour : 19 février 2025

---

## 1. Entités prioritaires

| Type d'entité | Pourquoi est-il prioritaire ? | Identifiant actuel dans le dataset | Stratégie proposée |
|---|---|---|---|
| **Établissement** | Entité centrale du dataset — chaque ligne décrit un établissement. Sans identifiant stable, aucun lien vers l'extérieur n'est possible. | Aucun identifiant sémantique — il existe probablement un nom et un sigle, mais aucun code officiel ni URI | Construire un identifiant local préfixé `MA-MESRSI-{SIGLE}` à partir du sigle officiel, complété si nécessaire par la ville (`MA-ENSIAS-RABAT`) ; aligner ensuite sur Wikidata via le site web officiel |
| **Université de rattachement** | 165 établissements sont affiliés à 12 universités. La relation hiérarchique établissement → université est fondamentale pour le graphe. | Probablement le nom de l'université (texte libre) | Créer une entité distincte pour chaque université avec un URI propre ; aligner sur Wikidata (`owl:sameAs Q…`) |
| **Ville** | Ancre géographique commune à plusieurs établissements. Sans identifiant de ville, les relations spatiales ne sont pas exploitables en RDF. | Nom de ville en texte libre (probablement en français ou en translittération variable) | Aligner chaque ville sur son identifiant GeoNames (`geonames:2536173` pour Rabat, etc.) ; ajouter un champ `geonames_id` |
| **Région administrative** | Le Maroc est découpé en 12 régions. La région est un niveau hiérarchique pertinent entre la ville et le pays. | Non présent dans le dataset (à inférer depuis la ville) | Ajouter un champ `region` normalisé selon le découpage officiel post-2015 ; aligner sur GeoNames et Wikidata |

---

## 2. Champs à normaliser

| Champ | Problème observé | Règle de normalisation proposée | Impact attendu |
|---|---|---|---|
| **Nom de l'établissement** (nom complet) | Probablement sans accents ou avec une casse non uniforme, comme observé dans le fichier pédagogique. Translittérations variables pour les noms d'origine arabe (ex. « Mohammed » / « Mohamed » / « Muhammad »). | Normaliser en UTF-8 avec accents ; appliquer une casse titre cohérente (majuscule à chaque mot principal) ; choisir une translittération de référence et l'appliquer uniformément (recommandation : suivre l'orthographe officielle du MESRSI) | Améliore les correspondances textuelles avec Wikidata et DBpedia ; réduit le risque de doublons perçus |
| **Sigle / acronyme** | Risque d'ambiguïté internationale (ex. « UAE » pour Abdelmalek Essaadi University = aussi abréviation courante des Émirats arabes unis). | Préfixer systématiquement par le code pays : `MA-{SIGLE}` (ex. `MA-UAE`, `MA-ENSIAS`). En cas de collision interne (deux établissements avec le même sigle), ajouter la ville : `MA-ENSA-KENITRA` vs `MA-ENSA-FES`. | Élimine les ambiguïtés internationales ; permet un usage sûr comme clé candidate locale |
| **Ville** | Noms de villes potentiellement anglicisés (« Marrakesh » au lieu de « Marrakech », « Fes » au lieu de « Fès ») ou avec des variantes orthographiques (« Meknes » / « Meknès »). Absence de code géographique officiel. | Normaliser vers la forme française officielle (`Fès`, `Meknès`, `Marrakech`) ; ajouter un champ `city_geonames_id` contenant l'identifiant GeoNames correspondant | Permet l'alignement automatique sur GeoNames et l'enrichissement géographique (coordonnées GPS, région, province) |
| **Région** | Champ absent du dataset. La région est pourtant un niveau hiérarchique essentiel pour la couverture territoriale des universités. | Ajouter un champ `region` à partir de la ville, en utilisant le découpage officiel du Maroc en 12 régions (post-réforme 2015). Exemple : Rabat → *Rabat-Salé-Kénitra* ; Marrakech → *Marrakech-Safi* | Enrichit le graphe avec un niveau géographique intermédiaire ; permet des requêtes SPARQL sur la distribution régionale |
| **Université de rattachement** | Chaque établissement est affilié à une université mère, mais cette relation n'est probablement pas encodée comme un champ structuré distinct — elle peut être implicite dans le nom ou absente. | Ajouter un champ `parent_university` contenant le nom normalisé de l'université mère, avec un identifiant `parent_university_wikidata_id` | Modélise la relation hiérarchique `membre de` / `fait partie de` directement exploitable en RDF (`org:memberOf`, `schema:parentOrganization`) |
| **Type d'établissement** | Valeurs probablement en texte libre ou peu standardisées (université, faculté, école, institut…). | Définir un vocabulaire fermé local à 5-7 valeurs ; aligner chaque valeur sur `schema.org` (`schema:CollegeOrUniversity`, `schema:EducationalOrganization`) ou sur une ontologie institutionnelle (ex. EUNIS — European University Information Systems) | Réduit l'ambiguïté sémantique ; permet un typage RDF cohérent |
| **Domaine de formation principal** | Valeur textuelle libre sans code normalisé. Granularité hétérogène. | Aligner sur la classification ISCED-F 2013 (codes à 4 chiffres publiés par l'UNESCO) : `0613` pour l'informatique, `0714` pour l'électronique et l'automatisation, `0811` pour l'architecture et la construction, etc. | Comparabilité internationale ; permet des requêtes thématiques en SPARQL |
| **Statut d'ouverture** | Le dataset porte spécifiquement sur les établissements *ouverts* en 2024-2025. Ce critère temporel n'est probablement pas encodé comme un champ distinct. | Ajouter un champ `academic_year` avec la valeur `2024-2025` et un champ `status` avec la valeur `open` | Permet la versionnage temporel du dataset ; essentiel pour une publication LOD pérenne |

---

## 3. Stratégie d'identifiants

| Entité cible | Base de construction envisagée | Risque | Recommandation |
|---|---|---|---|
| **Établissement** | `https://data.gov.ma/resource/etablissement/MA-{SIGLE}` — URI construite à partir du préfixe pays et du sigle officiel de l'établissement | Collision si deux établissements ont le même sigle ; instabilité si le sigle change (ex. renommage d'université) | Utiliser le sigle comme base mais ajouter la ville en cas de collision (`MA-ENSA-KENITRA`) ; documenter la règle de construction ; prévoir une redirection en cas de changement |
| **Université mère** | `https://data.gov.ma/resource/universite/MA-{SIGLE_UNIV}` + lien `owl:sameAs` vers l'identifiant Wikidata correspondant | Les 12 universités marocaines ont toutes une notice Wikidata vérifiable — risque faible | Aligner directement sur Wikidata (`owl:sameAs`) pour les 12 universités principales ; utiliser l'URI locale uniquement comme point de départ |
| **Ville** | Réutiliser directement l'identifiant GeoNames (`https://sws.geonames.org/{ID}/`) — pas d'URI locale à créer | GeoNames est un référentiel externe non contrôlé par le MESRSI ; un identifiant GeoNames pourrait en théorie évoluer | Adopter GeoNames comme référentiel géographique de référence ; créer un mapping local `city_name → geonames_id` et le versionner dans le dépôt |
| **Région administrative** | Réutiliser les identifiants Wikidata des 12 régions marocaines (ex. *Rabat-Salé-Kénitra* → `Q2734891`) | Faible — les régions marocaines post-2015 sont stables et bien documentées | Aligner sur Wikidata (`owl:sameAs`) ; ajouter le code ISO 3166-2 de la région marocaine lorsqu'il est disponible |
| **Domaine de formation** | Réutiliser les codes ISCED-F 2013 de l'UNESCO (`https://uis.unesco.org/isced/F/{CODE}`) | L'alignement est manuel et approximatif pour les valeurs génériques (ex. « multidisciplinaire ») | Effectuer l'alignement ISCED au niveau `skos:closeMatch` et non `owl:sameAs` pour les correspondances incertaines ; documenter chaque décision d'alignement |

---

## 4. Risques et limites

### Champs trop faibles pour servir d'identifiant

- **Nom complet de l'établissement** : trop sujet aux variantes orthographiques, translittérations et reformulations. Deux sources différentes peuvent nommer le même établissement de manière différente (ex. « Université Mohammed V » / « UM5 » / « Université de Rabat »). Ne peut pas être utilisé seul comme identifiant.
- **Ville seule** : plusieurs établissements sont implantés dans la même ville (ex. Rabat regroupe au moins 5 établissements distincts). La ville n'est pas discriminante à elle seule.
- **Type d'établissement** : valeur de classification, pas d'identifiant. Elle ne distingue pas les entités individuelles.

### Collisions et ambiguïtés possibles

- **Sigle non unique à l'échelle nationale** : plusieurs villes accueillent des ENSA (Écoles Nationales des Sciences Appliquées). Si le dataset contient les établissements de toutes les universités régionales, les sigles ENSA de Kénitra, Fès, Oujda, Agadir et Marrakech sont tous désignés « ENSA » localement. La règle `MA-{SIGLE}-{VILLE}` est donc indispensable.
- **Changements de tutelle ou de nom** : certains établissements marocains ont changé de nom ou d'affiliation universitaire dans les dernières années. Un identifiant basé sur le sigle peut alors devenir obsolète. Il est recommandé de maintenir un fichier de correspondance `sigle_ancien → sigle_nouveau` et de gérer les redirections d'URI.
- **Différence entre établissement de formation et institut de recherche** : le dataset contient 162 établissements de formation et 3 instituts de recherche scientifique. Ces deux catégories ont des statuts juridiques différents. Sans champ distinguant clairement les deux, un appariement sur Wikidata pourrait confondre une faculté avec un laboratoire de recherche.

### Informations supplémentaires nécessaires

- **Code officiel MESRSI** : un code ministériel attribué par le MESRSI à chaque établissement serait l'identifiant institutionnel le plus stable. S'il existe dans le dataset officiel, il doit être utilisé comme base de l'URI locale plutôt que le sigle.
- **Coordonnées GPS** : absentes du dataset décrit sur le portail. Leur ajout permettrait une désambiguïsation géographique fiable (deux établissements au même sigle dans des villes différentes seraient distincts par leurs coordonnées) et une intégration avec GeoNames sans appariement textuel.
- **Date de fermeture éventuelle** : le dataset porte sur les établissements *ouverts* en 2024-2025. Pour une publication LOD pérenne, il faudrait pouvoir tracer l'historique des ouvertures et fermetures (`schema:startDate` / `schema:endDate`), afin de ne pas invalider des URI déjà publiées si un établissement ferme.
- **Langue d'enseignement** : non présente dans le dataset selon la description du portail. Cette information est pourtant utile pour l'alignement avec des référentiels comme EUNIS ou le portail d'information sur les établissements européens.

### Limites de la stratégie d'identification proposée

La stratégie `MA-{SIGLE}-{VILLE}` est pragmatique et applicable immédiatement sans données supplémentaires, mais elle repose sur des informations textuelles susceptibles de varier. Elle constitue une solution de départ opérationnelle pour le TP2, mais devra être remplacée par un identifiant officiel stable (code MESRSI ou identifiant issu d'un registre institutionnel national) dès que celui-ci sera disponible. Les URI publiées dans un graphe LOD sont supposées être pérennes : toute modification ultérieure des identifiants nécessiterait la mise en place de redirections `303 See Other` ou de propriétés `owl:deprecated`, ce qui alourdit significativement la maintenance du graphe.
