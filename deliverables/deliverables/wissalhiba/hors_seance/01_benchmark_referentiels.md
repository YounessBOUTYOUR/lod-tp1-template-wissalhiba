# Benchmark des référentiels externes

## Identification

- **Groupe** : Groupe wissalhiba 
- **Jeu de données étudié** : *Établissements de l'enseignement supérieur universitaire public ouverts au titre de l'année 2024-2025* — MESRSI, Portail Open Data du Maroc (data.gov.ma), licence ODbL, mis à jour le 19 février 2025.
- **Types d'entités retenus** : Établissement · Université · Ville
## 1. Référentiels comparés

| Référentiel | URL | Types d'entités couverts | Intérêt potentiel | Limites observées |
|---|---|---|---|---|
|Wikidata | https://www.wikidata.org | Établissement, Université, Ville, Région | Identifiants QID stables et déréférençables, liens interlangues (ar/fr/en) | Couverture inégale : certains petits établissements marocains absents ou mal renseignés ; données parfois non vérifiées |
|GeoNames | https://www.geonames.org | Ville, Région, Pays | Identifiants numériques stables pour les entités géographiques ; coordonnées GPS ; hiérarchie administrative (commune → région → pays) | Ne couvre pas les établissements d'enseignement ; noms parfois en version anglaise ou translittérée seulement |
| DBpedia | https://dbpedia.org | Établissement, Ville | Extraction automatique depuis Wikipédia ; liens owl:sameAs vers Wikidata et GeoNames ; données structurées en RDF natif | Couverture dépend de la présence d'un article Wikipédia ; mise à jour moins fréquente ; translittérations variables |
|ISNI (International Standard Name Identifier)| https://isni.org | Organisation (dont université) | Identifiant normatif ISO 27729 pour les organisations ; utilisé par les bibliothèques nationales | Couverture limitée pour les établissements marocains récents ; non adapté aux entités géographiques |
| ROR (Research Organization Registry) | https://ror.org | Université, Institut de recherche | Identifiants stables pour les organisations de recherche ; API ouverte ; aligné avec Wikidata et ISNI | Orienté recherche : les facultés sans activité de recherche propre peuvent manquer ; couverture Maroc partielle |

## 2. Critères de comparaison

Les critères retenus pour comparer les référentiels sont les suivants :

Couverture: proportion des entités du jeu de données que le référentiel est susceptible de couvrir. Wikidata et DBpedia offrent la meilleure couverture pour les universités marocaines connues (12 universités publiques), mais GeoNames est plus complet pour les villes secondaires.

Qualité des identifiants: les identifiants Wikidata (ex. `Q654060` pour l'Université Mohammed V de Rabat) sont stables, déréférençables en HTTP et utilisés dans de nombreux jeux de données liés. Les identifiants GeoNames (ex. `2542007` pour Rabat) sont aussi stables et largement référencés. Les identifiants ROR (`https://ror.org/XXXXXXX`) sont spécialisés pour les organisations de recherche.

Précision sémantique: Wikidata et DBpedia permettent de distinguer l'université (institution mère) de l'établissement (composante : faculté, école, institut). GeoNames ne supporte pas cette distinction institutionnelle.

Facilité d'appariement: Wikidata est interrogeable via SPARQL (endpoint public) ; GeoNames via son API de géocodage ; DBpedia via SPARQL. L'appariement manuel reste nécessaire pour des établissements peu connus.

Richesse des informations disponibles: Wikidata fournit des données structurées multilingues (nom en arabe, français, anglais), des liens vers des sources externes, la date de création, la ville, le site officiel. GeoNames fournit des coordonnées, le code administratif, l'altitude.

## 3. Cas d'appariement manuel

> Les entités ci-dessous sont issues du jeu de données MESRSI 2024-2025 (165 établissements, 12 universités).

| Entité locale | Type | Référentiel cible | Correspondance candidate | Indices utilisés | Niveau de confiance | Décision |
|---|---|---|---|---|---|---|
| Université Mohammed V – Rabat | Université | Wikidata | [Q654060](https://www.wikidata.org/wiki/Q654060) Université Mohammed V de Rabat| Nom officiel, ville (Rabat), tutelle MESRSI, présence de P131=Rabat et P749=ministère marocain |Élevé| Appariement confirmé |
| Université Hassan II – Casablanca | Université | Wikidata | [Q1124560](https://www.wikidata.org/wiki/Q1124560) Université Hassan II de Casablanca | Nom officiel, ville (Casablanca), vérification via libellé en arabe et site web officiel | Élevé| Appariement confirmé |
| Faculté des Sciences Juridiques, Économiques et Sociales Rabat (FSJES Agdal) | Établissement | Wikidata | [Q3394131](https://www.wikidata.org/wiki/Q3394131) Faculté des sciences juridiques, économiques et sociales de Rabat | Nom, ville, université de rattachement (Mohammed V), acronyme FSJES |Moyen|Appariement probable(plusieurs FSJES à Rabat) |
| Faculté des Sciences – Agadir (FSA) | Établissement | Wikidata | Page Wikidata [Q3394130](https://www.wikidata.org/wiki/Q3394130) Faculté des Sciences d'Agadir| Nom, ville (Agadir), rattachement à Université Ibn Zohr |Moyen | Appariement probable |
| Rabat | Ville | GeoNames | [2542007](https://www.geonames.org/2542007) Rabat (capitale administrative) | Nom de ville, statut de capitale, présence dans GeoNames avec coordonnées 34.01325 / -6.83255 | Élevé| Appariement confirmé |
| Marrakech | Ville | GeoNames | [2542993](https://www.geonames.org/2542993) Marrakech | Nom de ville, région Marrakech-Safi, coordonnées connues | Élevé| Appariement confirmé |
| Université Cadi Ayyad – Marrakech | Université | ROR | [https://ror.org/01pjx1a68](https://ror.org/01pjx1a68) — Cadi Ayyad University| Nom en anglais correspondant, ville (Marrakech), présence dans le registre ROR avec lien Wikidata |Élevé|  Appariement confirmé |
| École Nationale Supérieure d'Informatique et d'Analyse des Systèmes – Rabat (ENSIAS) | Établissement | Wikidata | [Q3051694](https://www.wikidata.org/wiki/Q3051694) — ENSIAS | Acronyme, nom complet, ville (Rabat), rattachement Université Mohammed V | Élevé|Appariement confirmé |
| Institut National des Postes et Télécommunications – Rabat (INPT) | Établissement | Wikidata | [Q3151490](https://www.wikidata.org/wiki/Q3151490) — *Institut national des postes et télécommunications* | Nom complet, acronyme INPT, ville, type d'établissement |Moyen | À confirmer — statut « sous tutelle MESRSI » à vérifier |
| Oujda | Ville | GeoNames | [2542503](https://www.geonames.org/2542503) Oujda| Nom de ville, région Oriental, coordonnées |Élevé |Appariement confirmé |
## 4. Synthèse
Quel référentiel paraît le plus utile pour chaque type d'entité ?

- Établissement : Wikidata est le référentiel le plus approprié. Il couvre les principales composantes universitaires marocaines, fournit des QID stables, et est aligné avec DBpedia et GeoNames via des propriétés `owl:sameAs`. Pour les établissements absents de Wikidata, la création d'un nouvel item est possible et recommandée.
-Université : Wikidata en premier choix, ROR en complément pour les institutions à vocation de recherche. Les 12 universités publiques marocaines sont toutes présentes dans Wikidata.
- Ville: GeoNames est le référentiel de référence pour les entités géographiques. Il offre des identifiants numériques stables, des coordonnées GPS, et une hiérarchie administrative complète (ville → province → région → Maroc).

Quels cas restent ambigus ?

- Les FSJES (Facultés des Sciences Juridiques, Économiques et Sociales) : plusieurs villes en ont une (Rabat Agdal, Rabat Souissi, Casablanca, Fès…). Le seul nom « FSJES » sans ville précise est insuffisant pour l'appariement.
- Les Centres universitaires régionaux récemment créés (ex. centre universitaire de Guelmim) peuvent être absents de Wikidata faute d'article Wikipédia.
- Certains Instituts de recherche scientifique (3 dans le jeu de données) ont une présence variable selon les référentiels.

Quels champs du dataset aident le plus à l'appariement ?

Les champs les plus discriminants sont : le nom complet de l'établissement, la ville siège, l'université de rattachement, et l'acronyme(quand disponible). Le champ ville combiné au nom de l'université parente permet de lever la majorité des ambiguïtés.
