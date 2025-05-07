# DVF
L'objectif est de construire un indice des prix de ventes des appartements en France sur la (quasi) totalité du territoire. Dans ce projet des méthodes typiquement classé en "machine learning" sont utilisés. Néanmoins il n'est pas ici question de construire un modèle de prédiction, mais de construire un indice. Autrement dit, les modèles servent d'outils descriptifs. 

Pour cela deux approches économétriques sont employées : l'approche comparative et l'approche hédonique.

## approche comparative

L'idée est d'attribuer un prix à un bien en le comparant avec des biens similaires. Un bien est caractérisé par ses attributs (nombre de pièces, surface, etc) et son environnement (nombre de lieux culturels dans le quartier, nombre d'écoles dans le quartier, activité économique du quartier, etc). 

L'application de cette méthode passe par l'utilisation d'un k-nn par type d'appartements (selon nombre de pièces) et avec une pondération des variables fournit par l'utilisateur (une méthode appelée processus analytique hiérarchique est implémentée pour aider le métier à déterminer les pondérations des variables). 

## approche hédonique

L'approche comparative est limitante sur plusieurs aspects dont le manque de compréhension du résultat. L'approche hédonique vise décomposer le prix d'un bien selon ses caractéristiques dans un modèle interprétable (au niveau global). Ce besoin d'interprétation est la raison pour laquelle les modèles populaires style XGboost et réseau de neurones ne sont pas utilisés. Ici nous utilisons le modèle MARS car ce dernier est interprétable et peut détecter des intéractions entre variables et des courbures. 

Une autre difficulté pour l'approche hédonique repose sur le fait que les ventes sont inégalement réparties sur le territoire (certains iris n'ont même aucune vente). Ainsi, si le modèle est entrainé sur les seules données de ventes disponibles, il s'ajustera sur des ventes de biens non représentatifs et ne permettra pas de fournir des prix "corrects" pour les biens rares. Le terme "correct" est ici entre guillemets car étant donné que nous allons attribuer des prix à des biens non (ou peu) présents dans le jeu de données, cette notion est discutable. Par là, il faut comprendre que l'extrapolation du modèle à des biens atypiques pourrait être jugée "mauvaise" car celle ci serait (quasi) identiques aux bien non-atypiques. 

Pour résoudre le problème les données sont alors pondérées de façon à ce que la distribution marginale des caractéristiques des biens vendues corresponde à la distribution marginale des caractéristiques des iris en France. (A noter que parceque le modèle MARS est lent lorsque l'on utilise directement les pondérations, un échantillon plus grand que celui des données est tirée en suivant la distribution des poids attribués à chaque vente). 

# Ouverture 

Ce projet est un premier jet. Il ne serait donc pas surprenant que certains résultats soient étonnants. Néanmoins il n'y a pas à ma connaissance de projet permettant de fournir des prix sur l'ensemble du territoire métropolitain (uniquement là où il y a un minimum de x ventes). Ainsi je suis persuadé que des résultats (même faux) restent mieux que pas de résultat du tout et qu'avec un peu de fine tuning (métier et statistique), les modèles proposées pourront être plus précis et donc plus utiles. 

D'autres variables pourraient être pertinentes : revenus, attractivité (position dans l'aire d'attraction), la période de vente, etc.

Le nombre de données est relativement faible étant donné le nombre de variables et d'iris en France. On pourrait utiliser les ventes des années précédentes (corrigées de l'inflation) afin d'avoir un jeu de données plus important (surtout pour les iris avec peu ou pas de ventes). 

La pondération fait correspondre les distributions marginales, mais cela ne garantit pas la correspondance des distributions jointes. L'utilisation d'un algorithme permettant cette correspondance serait plus pertinent. 


# Sources donnees trop lourdes : 
dvf : https://www.data.gouv.fr/fr/datasets/demandes-de-valeurs-foncieres-geolocalisees/#/community-resources

contours iris : https://geoservices.ign.fr/contoursiris
