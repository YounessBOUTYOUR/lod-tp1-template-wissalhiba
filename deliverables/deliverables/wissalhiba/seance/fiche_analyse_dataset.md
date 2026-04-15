# Fiche d'analyse du jeu de données

## Identification

- **Groupe** :Groupe wissalhiba
- **Date** : 14/04/2026
- **Nom du jeu de données** : higher_education_sample.csv
- **Source / producteur** : TP1
- **URL de la source** : https://github.com/YounessBOUTYOUR/lod-tp1-template-wissalhiba/blob/main/data/higher_education_sample.csv
- **Domaine métier** : Enseignement supérieur

## Description générale

- **Objectif du jeu de données** : 
- **Format(s) disponible(s)** : CSV
- **Langue(s)** : Anglais
- **Licence** : --
- **Fréquence de mise à jour** : --
- **Unité d'observation** :Etablissement d'enseignement supérieur
- **Granularité des enregistrements** : Une ligne = un établissement

## Structure du jeu de données

| Champ / colonne | Signification | Exemple de valeur | Observation |
| record_id | Identifiant local numerique  | 1 | Entier séquentiel, propre au fichier|
| institution_name | Nom de l'établissement | Ecole Nationale Superieure d'Informatique et d'Analyse des Systemes  | Pas d'accents, orthographe parfois incomplète |
| short_name | acronyme | ENSIAS | non normalisé |
| city | Ville | Rabat | Valeur textuelle |
| country | Pays  | Morocco | faible variabilité |
| website | URL du site officiel | https://www.ensias.um5.ac.ma |  potentiellement instable dans le temps |
| institution_type | Type d'établissement | public school, university, faculty | Valeur textuelle non normalisée  |
| main_field | Domaine principal | computer science, engineering | Valeur textuelle  |
| year_created | Année de création | 1992  | Donnée utile mais non vérifiable dans le fichier seul  |
| portal_hint | Portail Open Data source suggéré | data.gov.ma | peu informative |

## Évaluation "Open Data"

| Critère | Observation | Score / 5 |
| --- | --- | --- |
| Accessibilité | Données disponibles en CSV, faciles à ouvrir et exploiter | 5 |
| Réutilisabilité | Données structurées mais peu normalisées et sans identifiants stables | 3 |
| Documentation | Peu d’informations sur la source et les champs | 2 |
| Format exploitable | Format tabulaire clair et structuré (CSV) | 5 |
| Présence d'une licence | Aucune licence indiquée dans le dataset | 1 |

## Évaluation "Linked Data readiness"

| Point observé | Oui / Non / Partiel | Commentaire |
| --- | --- | --- |
| Identifiants stables pour les enregistrements | Non | record_id est un entier arbitraire, propre au fichier |
| Entités clairement distinguables | Oui | Chaque ligne représente bien un établissement distinct |
| Valeurs normalisées | Partiel | institution_type et main_field sont des chaînes libres non contrôlées |
| Informations géographiques exploitables | Partiel  | city et country sont présents mais sans coordonnées ni code |
| Possibilité de lien vers des référentiels externes | Oui | institution_name, short_name et website permettent un appariement avec Wikidata/DBpedia |
| Présence de clés candidates plausibles | partiel | short_name + city forment une clé candidate composite plausible (ex. ENSIAS + Rabat) |
| Présence de codes ou nomenclatures réutilisables | Non | Aucun code pays, code discipline ou identifiant standardisé |

## Maturité "5 étoiles"

| Niveau | Atteint ? | Justification |
| --- | --- | --- |
| 1 étoile : données disponibles sur le Web | Oui | Le fichier CSV est distribué sur un dépôt GitHub public |
| 2 étoiles : données structurées | Oui | Format CSV tabulaire, lisible par machine |
| 3 étoiles : format non propriétaire | Oui | CSV est un format ouvert |
| 4 étoiles : usage d'URI pour identifier les choses | Non | Aucune URI déréférençable n'est assignée aux établissements , record_id est un entier local |
| 5 étoiles : liens vers d'autres données | Non | Aucun lien vers Wikidata, DBpedia ou GeoNames |

## Clés candidates et identifiants

| Objet ou entité cible | Champ(s) candidat(s) | Fiabilité | Limites | Recommandation |
| Etablissement | short_name + city | moyenne  | short_name non formellement unique à l'échelle nationale | Construire un URI de type https://data.gov.ma/institution/ENSIAS-Rabat |
| Etablissement | website |bonne  | URL peut changer |Utiliser comme identifiant secondaire|
| Ville| city |faible  | Pas de code GeoNames ni ISO  |Ajouter un identifiant GeoNames pour chaque ville|
| Pays | country |faible  |Toujours Morocco ici  | Remplacer par MA |

## Normalisation préalable nécessaire

| Champ | Problème observé | Impact pour le LOD | Action recommandée |
| institution_type | Valeurs libres en anglais (public school, faculty…) | Impossible à aligner avec une ontologie sans mapping | Mapper vers des classes standard |
| main_field | Valeurs libres (multidisciplinary, applied sciences…) |Empêche l'alignement avec un thésaurus disciplinaire  | Normaliser selon un référentiel |
| city  |Texte libre sans identifiant géographique|Empêche le lien vers GeoNames ou Wikidata  | Ajouter un champ city_geonames_id ou city_wikidata_id |
| country | Texte libre (Morocco) sans code ISO |Non interopérable avec les standards géographiques |Ajouter country_code: MA  |
| institution_name| Absence d'accents (Superieure au lieu de Supérieure) | Risque d'échec d'appariement par correspondance exacte | Corriger les caractères et normaliser l'encodage UTF-8 |

## Qualité des données

- **Forces observées** : Données propres sans valeurs manquantes visibles. Analyse facilitée.
Noms complets et sigles présents. Appariement plus facile.
URLs valides disponibles. Vérification possible.
year_created rempli. Ajoute une info temporelle utile.
- **Faiblesses observées** :nstitution_type non normalisé. Manque de cohérence.
main_field libre. Risque d’incohérences.
record_id non stable. Pas fiable comme identifiant.
portal_hint inutile. Même valeur partout.
- **Ambiguïtés ou doublons potentiels** :short_name non unique. Risque de confusion.
Noms avec ville intégrée. Ambiguïtés possibles.
- **Champs manquants pour une meilleure interconnexion** :Pas de codes pays ni GPS. Liaison limitée.
Pas d’ID externe (Wikidata). Intégration difficile.
Pas de codes de discipline. Manque de standard.
- **Risque principal pour une future mise en relation** :Noms similaires entre établissements. Risque d’erreur.
Pas d’identifiant stable. Mauvais appariement possible.

## Conclusion courte
Le dataset higher_education_sample.csv est une base correcte mais encore incomplète pour le Linked Open Data. Il correspond au niveau 3 étoiles (données ouvertes et structurées), mais il manque des identifiants stables (niveau 4) et des liens externes (niveau 5).

Pour évoluer vers du LOD, trois actions sont nécessaires : créer des identifiants uniques (URI), normaliser certains champs avec des standards, et ajouter des liens vers des référentiels comme GeoNames ou Wikidata. Cela permettra de rendre les données interopérables et connectées.
