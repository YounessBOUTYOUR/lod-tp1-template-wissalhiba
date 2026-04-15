# Plan de normalisation et d'identification
---

## 1. Entités prioritaires

| Type d'entité | Pourquoi est-il prioritaire ? | Identifiant actuel | Stratégie proposée |
|---|---|---|---|
| Établissement | Entité centrale du jeu de données (165 occurrences). C'est l'unité de base de toute future modélisation RDF. Chaque établissement doit être une ressource identifiable et déréférençable. | Aucun identifiant stable. Seul le nom textuel est disponible. | Construire un URI local (`https://data.gov.ma/id/etablissement/{slug}`) + aligner avec Wikidata QID quand disponible |
| Université | Entité de regroupement clé (12 universités pour 165 établissements). Elle structure la hiérarchie institutionnelle et constitue un pivot naturel pour les liens avec des référentiels externes (Wikidata, ROR). | Nom textuel uniquement, sans code ni identifiant officiel dans le fichier XLSX. | Construire un URI local (`https://data.gov.ma/id/universite/{slug}`) + aligner avec Wikidata QID et ROR ID |
| Ville | Dimension géographique essentielle pour l'enrichissement et la visualisation. Permet de lier le dataset à des référentiels géospatiaux (GeoNames, Wikidata). | Nom de ville textuel, sans code géographique officiel. | Normaliser les noms de villes + aligner avec identifiants GeoNames |

---

## 2. Champs à normaliser

| Champ | Problème observé | Règle de normalisation proposée | Impact attendu |
|---|---|---|---|
| Nom de l'établissement | Variantes orthographiques, usage incohérent des tirets, majuscules, accents (ex. « Faculté des Sciences » vs « Faculte des sciences ») ; présence du nom de la ville dans certains intitulés mais pas tous | Adopter une forme canonique en français : majuscule initiale + minuscules, accents normalisés, ville entre tirets ou entre parenthèses selon modèle uniforme | Appariement plus fiable avec Wikidata/DBpedia ; construction d'URI cohérents |
| Nom de l'université de rattachement | Noms parfois tronqués ou abrégés (ex. « UH2C » pour « Université Hassan II de Casablanca ») ; absence d'identifiant officiel | Définir une liste canonique des 12 universités avec leur nom officiel complet + acronyme normalisé | Permet de construire la relation `appartientA` en RDF sans ambiguïté |
| Nom de la ville | Translittérations multiples (ex. « Kénitra » / « Kenitra » / « Qnitra ») ; usage parfois du nom arabe | Adopter la forme officielle en français telle qu'utilisée par l'ONEE et les administrations (liste de référence : GeoNames + découpage administratif officiel) | Appariement avec GeoNames sans ambiguïté ; cohérence inter-datasets |
| Type d'établissement | Valeurs non homogènes (ex. « Faculté », « École », « Institut », « Centre ») sans vocabulaire contrôlé | Créer un champ `typeEtablissement` avec un vocabulaire contrôlé en 5-6 valeurs : `Faculté`, `École Nationale Supérieure`, `Institut`, `Centre Universitaire`, `École Nationale de Commerce et de Gestion`, `Institut de Recherche` | Permettra l'utilisation d'une propriété RDF typée (ex. `rdf:type` lié à un concept SKOS) |
| Région administrative | Champ potentiellement absent ou incohérent avec le découpage régional officiel (12 régions post-2015) | Ajouter ou corriger ce champ en s'appuyant sur le découpage officiel des 12 régions marocaines et le code INS | Enrichissement géographique ; liens avec jeux de données régionaux |
| Acronyme | Présent de façon incohérente (parfois dans le nom, parfois absent) | Extraire ou standardiser l'acronyme dans un champ séparé `sigle` (ex. `FSJES`, `ENCG`, `EST`, `ENSIAS`) | Facilite la désambiguïsation et l'appariement automatique |

---

## 3. Stratégie d'identifiants

| Entité cible | Base de construction envisagée | Risque | Recommandation |
|---|---|---|---|
| Établissement | URI de la forme `https://data.gov.ma/id/etablissement/{type}-{ville}-{sigle}` — ex. `https://data.gov.ma/id/etablissement/fsjes-rabat-agdal` | Collision si deux établissements ont le même sigle dans la même ville (ex. deux FSJES à Rabat) ; instabilité si le nom change | Ajouter un discriminant numérique en cas de collision (`fsjes-rabat-01`) ; documenter la règle de slug dans les métadonnées |
| Université | URI de la forme `https://data.gov.ma/id/universite/{sigle}` — ex. `https://data.gov.ma/id/universite/umv-rabat` | Peu de risque de collision (12 universités seulement) ; risque si une université change de nom ou fusionne | Croiser avec le QID Wikidata comme identifiant secondaire stable (`owl:sameAs`) |
| Ville | Réutiliser directement l'URI GeoNames — ex. `https://sws.geonames.org/2542007/` pour Rabat | Aucun risque de collision (GeoNames est le standard) ; nécessite un appariement préalable | Préférer la réutilisation d'un URI externe plutôt que la création d'un URI local pour les villes |
| Type d'établissement | URI d'un concept SKOS dans un vocabulaire contrôlé local — ex. `https://data.gov.ma/vocab/typeEtablissement/faculte` | Vocabulaire à maintenir ; risque d'obsolescence si de nouveaux types apparaissent | Publier le vocabulaire SKOS séparément et le versionner |

---

## 4. Risques et limites

**Quels champs sont trop faibles pour servir d'identifiant ?**

- Le nom complet seul est insuffisant : plusieurs établissements peuvent avoir un nom quasi-identique dans des villes différentes.
- Le nom de la ville seul est trop peu discriminant.
- L'absence d'un code officiel MESRSI (numéro d'établissement) dans le fichier est la lacune principale : sans identifiant natif, toute stratégie d'URI est indirecte.

**Quelles collisions ou ambiguïtés sont possibles ?**

- Les FSJES (Facultés des Sciences Juridiques, Économiques et Sociales) : au moins une douzaine dans le pays, certaines dans la même ville (Rabat dispose d'une FSJES Agdal et d'une FSJES Souissi, toutes deux rattachées à l'Université Mohammed V). Le slug `fsjes-rabat` serait ambigu.
- Les ENCG (Écoles Nationales de Commerce et de Gestion) : présentes dans de nombreuses villes. Le nom seul sans ville produit une collision immédiate.

**Quelles informations supplémentaires seraient nécessaires ?**

- Un code officiel interne MESRSI par établissement .
- Les coordonnées géographiques pour chaque établissement, absentes du fichier XLSX.
- Le site web officiel de chaque établissement.
- La date de création ou d'ouverture de chaque établissement.
- Le statut d'accès (accès ouvert / accès régulé) pour enrichir la description sémantique.
