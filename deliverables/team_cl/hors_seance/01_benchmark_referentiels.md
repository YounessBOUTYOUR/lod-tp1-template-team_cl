# Benchmark des référentiels externes

## Identification

- **Groupe :** team_cl
- **Jeu de données étudié :** *Établissements de l'enseignement supérieur universitaire public ouverts au titre de l'année 2024-2025* — MESRSI / data.gov.ma (Universités Publiques 2024-2025.xlsx)
- **Types d'entités retenus :** Établissement · Université · Ville · Région administrative

---

## 1. Référentiels comparés

| Référentiel | URL | Types d'entités couverts | Intérêt potentiel | Limites observées |
|---|---|---|---|---|
| **Wikidata** | https://www.wikidata.org/ | Établissements, universités, villes, régions, pays, personnes, concepts | Identifiants `Q` stables et déréférençables ; propriétés riches (`P856` site web, `P131` localisation, `P571` date de fondation) ; notices bilingues arabe/français disponibles pour les établissements marocains ; liens automatiques vers DBpedia, GeoNames | Couverture inégale : les petites écoles et instituts récents sont absents ou peu documentés ; données parfois non vérifiées (contributions communautaires) ; risque de faux appariement sur les noms partiellement similaires |
| **GeoNames** | https://www.geonames.org/ | Villes, régions, pays, entités géographiques | Identifiants numériques stables (`geonames_id`) ; hiérarchie administrative complète (commune → province → région → pays) ; coordonnées GPS pour toutes les villes ; API gratuite ; codes ISO pour pays et régions | Ne couvre pas les établissements d'enseignement ; noms en translittération variable (arabe, latin) pouvant créer une ambiguïté avec les noms du dataset ; absence de lien direct vers les données institutionnelles |
| **DBpedia** | https://dbpedia.org/ | Universités, villes, régions, pays (tout ce qui est présent sur Wikipédia) | URIs dérivés de Wikipédia (`dbpedia.org/resource/`) ; classes OWL (`dbo:University`, `dbo:City`) ; propriétés structurées héritées des infoboxes Wikipédia ; liens `owl:sameAs` vers Wikidata et GeoNames déjà présents | Dépendance totale à Wikipédia : couverture très variable pour les établissements marocains ; données moins à jour que Wikidata ; API SPARQL parfois instable ; noms en anglais ou en transcription non normalisée |
| **data.gov.ma** | https://www.data.gov.ma/ | Établissements publics, données administratives marocaines | Source officielle du jeu de données ; fournit le contexte institutionnel et la structure hiérarchique (établissement → université de rattachement) | Pas un référentiel au sens LOD : pas d'identifiants URI stables par entité ; données en format XLSX sans lien machine-readable ; pas d'API SPARQL |
| **ISCED-F 2013 (UNESCO)** | https://uis.unesco.org/en/isced-mappings | Domaines de formation (codes à 4 chiffres) | Classification internationale standardisée pour les disciplines académiques ; codes stables et largement utilisés (`0613` informatique, `0714` électronique, `0811` architecture…) | Ne couvre pas les entités institutionnelles (universités, villes) ; alignement manuel nécessaire car le dataset n'utilise pas de codes ISCED |

---

## 2. Critères de comparaison

Les critères suivants ont été retenus pour évaluer et comparer les référentiels :

**Couverture**
Wikidata offre la couverture la plus large pour les universités marocaines. Les 12 universités publiques principales sont toutes documentées (UM5, UH2C, UCA, USMBA, UIZ, UIT, UAE, UMP, UMI, USMS, UCD, Hassan 1er). Les établissements plus petits (écoles nationales, instituts spécialisés) sont couverts de manière inégale. GeoNames couvre toutes les villes et régions mentionnées dans le dataset. DBpedia couvre les universités principales mais avec moins de détail que Wikidata pour le contexte marocain.

**Qualité des identifiants**
GeoNames fournit les identifiants les plus stables et les plus fiables pour les entités géographiques (villes, régions). Les identifiants Wikidata (`Q`) sont stables et déréférençables, mais peuvent pointer vers des notices incomplètes. Les identifiants DBpedia sont dérivés du titre Wikipédia et peuvent donc changer en cas de renommage d'article.

**Précision sémantique**
Wikidata dispose de propriétés précises pour les établissements d'enseignement (`P571` date de fondation, `P856` site officiel, `P131` entité administrative, `P17` pays, `P2196` nombre d'étudiants). GeoNames fournit une hiérarchie administrative précise. ISCED-F assure une granularité sémantique fine pour les domaines disciplinaires.

**Facilité d'appariement**
Le champ `website` du dataset est l'ancre d'appariement la plus fiable pour les universités, car il correspond directement à la propriété `P856` de Wikidata. Le nom complet de l'établissement permet un appariement textuel avec Wikidata et DBpedia. Le nom de la ville permet un appariement direct avec GeoNames.

**Richesse des informations disponibles**
Wikidata enrichit les entités avec : le logo officiel (`P154`), le nombre d'étudiants (`P2196`), le recteur (`P1037`), les langues d'enseignement (`P2936`), les coordonnées (`P625`), et des liens vers les identifiants d'autres référentiels (GeoNames, VIAF, GRID). GeoNames ajoute les coordonnées GPS, le fuseau horaire, le code postal et la hiérarchie administrative complète.

---

## 3. Cas d'appariement manuel

Les cas d'appariement ci-dessous portent sur le **jeu de données réel MESRSI 2024-2025** (165 établissements, 12 universités). Les entités ont été recherchées manuellement sur Wikidata et GeoNames.

| Entité locale | Type | Référentiel cible | Correspondance candidate | Indices utilisés | Niveau de confiance | Décision |
|---|---|---|---|---|---|---|
| Université Mohammed V de Rabat | Université | Wikidata | [Q217004](https://www.wikidata.org/wiki/Q217004) | Correspondance du nom complet + ville Rabat + site officiel `um5.ac.ma` confirmé dans la propriété `P856` de la notice Wikidata | **Élevé** |  Appariement confirmé — `owl:sameAs` |
| Université Cadi Ayyad | Université | Wikidata | [Q1055303](https://www.wikidata.org/wiki/Q1055303) | Correspondance du nom + ville Marrakech + site `uca.ma` | **Élevé** |  Appariement confirmé — `owl:sameAs` |
| Université Hassan II de Casablanca | Université | Wikidata | [Q3552327](https://www.wikidata.org/wiki/Q3552327) | Correspondance du nom + ville Casablanca + site `univh2c.ma` ; distinction nécessaire avec l'ancienne Université Hassan II de Mohammedia (fusionnée) | **Élevé** |  Appariement confirmé avec vérification — la notice Wikidata correspond bien à l'université actuelle de Casablanca |
| Université Sidi Mohamed Ben Abdellah | Université | Wikidata | [Q1393479](https://www.wikidata.org/wiki/Q1393479) | Correspondance du nom + ville Fès + site `usmba.ac.ma` | **Élevé** |  Appariement confirmé — `owl:sameAs` |
| Université Ibn Zohr | Université | Wikidata | [Q3553006](https://www.wikidata.org/wiki/Q3553006) | Correspondance du nom + ville Agadir + site `uiz.ac.ma` | **Élevé** |  Appariement confirmé — `owl:sameAs` |
| Université Abdelmalek Essaadi | Université | Wikidata | [Q2840648](https://www.wikidata.org/wiki/Q2840648) | Correspondance du nom + ville Tétouan + site `uae.ac.ma` ; à distinguer de l'abréviation « UAE » ambiguë (Émirats arabes unis) | **Élevé** |  Appariement confirmé par le site web — l'indice `uae.ac.ma` élimine l'ambiguïté avec l'abréviation pays |
| Rabat | Ville | GeoNames | [2536173](https://www.geonames.org/2536173) | Correspondance exacte du nom + code pays MA + statut de capitale | **Très élevé** |  Appariement certain — `owl:sameAs` |
| Casablanca | Ville | GeoNames | [2553604](https://www.geonames.org/2553604) | Correspondance exacte du nom + code pays MA + première agglomération du pays | **Très élevé** |  Appariement certain — `owl:sameAs` |
| Marrakech | Ville | GeoNames | [2542997](https://www.geonames.org/2542997) | Correspondance du nom (le dataset peut utiliser « Marrakesh » en anglais, GeoNames liste les deux formes) + code pays MA | **Élevé** |  Appariement confirmé — attention à la variante orthographique à normaliser |
| Fès | Ville | GeoNames | [2295420](https://www.geonames.org/2295420) | Correspondance du nom (forme GeoNames : « Fes ») + code pays MA | **Élevé** |  Appariement confirmé — GeoNames utilise la translittération « Fes » cohérente avec le dataset |
| Agadir | Ville | GeoNames | [2508813](https://www.geonames.org/2508813) | Correspondance exacte du nom + code pays MA | **Très élevé** |  Appariement certain — `owl:sameAs` |
| École Nationale Supérieure d'Informatique et d'Analyse des Systèmes (ENSIAS) | Établissement (école) | Wikidata | [Q3571159](https://www.wikidata.org/wiki/Q3571159) | Correspondance du nom + ville Rabat + site `ensias.um5.ac.ma` confirmé dans `P856` | **Élevé** |  Appariement confirmé — `owl:sameAs` |
| Institut Agronomique et Vétérinaire Hassan II | Établissement (institut) | Wikidata | Recherche effectuée — notice partielle existante sans URI stable confirmé | Correspondance du nom + ville Rabat + site `iav.ac.ma` ; notice Wikipedia en arabe présente | **Moyen** |  Appariement probable mais à vérifier manuellement — utiliser `skos:closeMatch` provisoirement |
| École Mohammadia d'Ingénieurs (EMI) | Établissement (école) | DBpedia | [dbpedia:École_Mohammadia_d'ingénieurs](https://dbpedia.org/page/École_Mohammadia_d'ingénieurs) | Correspondance du nom + ville Rabat ; propriété `foaf:homepage` pointant vers `emi.ac.ma` | **Élevé** |  Appariement confirmé via DBpedia (lié à Wikidata Q3564748) |
| Université Mohammed Premier | Université | Wikidata | [Q3552889](https://www.wikidata.org/wiki/Q3552889) | Correspondance du nom + ville Oujda + site `ump.ma` | **Élevé** |  Appariement confirmé — `owl:sameAs` |

---

## 4. Synthèse

### Quel référentiel paraît le plus utile pour chaque type d'entité ?

**Pour les universités et établissements :** Wikidata est le référentiel le plus adapté. Il offre des identifiants stables, des propriétés institutionnelles riches, une couverture satisfaisante des 12 universités marocaines publiques, et des notices souvent bilingues (français/arabe). La propriété `P856` (site officiel) constitue la meilleure clé d'appariement automatisable. DBpedia est un complément utile pour les établissements disposant d'un article Wikipédia, mais les données y sont moins à jour.

**Pour les villes et régions :** GeoNames est le référentiel de référence. Ses identifiants sont extrêmement stables, son API gratuite permet une vérification automatisée, et la hiérarchie administrative (commune → province → région → pays) est directement exploitable pour modéliser les relations géographiques en RDF. Wikidata peut compléter GeoNames pour les villes disposant d'une notice riche (coordonnées, population, code postal).

**Pour les domaines de formation :** ISCED-F 2013 (UNESCO) est le référentiel pertinent. Il fournit une classification internationale permettant la comparabilité des données entre pays. L'alignement est toutefois manuel, les valeurs du dataset étant en anglais libre sans code normalisé.

### Quels cas restent ambigus ?

- **L'Institut Agronomique et Vétérinaire Hassan II** : la notice Wikidata semble exister mais sans confirmation du `P856` correspondant au site `iav.ac.ma`. Un contrôle manuel est nécessaire avant de valider l'appariement.
- **Les petits établissements et instituts récents** : les écoles nationales appliquées créées après 2000, ainsi que les centres d'études doctorales, sont peu ou pas couverts sur Wikidata. Il faudra créer les notices manquantes ou rester sur un appariement `skos:closeMatch` avec la notice de l'université mère.
- **Les établissements sans site web propre** : sans URL institutionnelle distincte, l'appariement repose uniquement sur la correspondance textuelle du nom, ce qui augmente le risque de faux positifs.
- **La forme « Marrakesh » vs « Marrakech »** : le dataset utilise une forme anglicisée qui ne correspond pas exactement au libellé officiel français ; la correspondance GeoNames reste valide mais nécessite une normalisation préalable du champ `city`.

### Quels champs du dataset aident le plus à l'appariement ?

1. **`website`** (URL du site officiel) : critère le plus discriminant. Il correspond directement à la propriété `P856` de Wikidata et à `foaf:homepage` de DBpedia. Très fiable pour les universités principales.
2. **`institution_name`** (nom complet) : permet une recherche textuelle dans Wikidata et GeoNames, mais exige une normalisation préalable (accents, casse, translittération).
3. **`city`** (ville) : ancre géographique permettant de désambiguïser les établissements homonymes ; directement alignable sur GeoNames après normalisation de la forme du nom.
4. **`short_name`** (sigle) : utile comme clé locale mais ambigu à l'international (ex. « UAE »). À utiliser uniquement en combinaison avec d'autres champs.
