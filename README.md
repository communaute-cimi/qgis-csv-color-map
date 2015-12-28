# qgis-csv-color-map

###Colorer une carte avec des données CSV (LibreOffice et QGIS).

Lier des données tabulaires avec des données géographiques c'est une demande récurrente dès lors qu'une organisation gère un térritoire que ce soit au niveau local ou national. 

Les exemples sont innombrables cela peut aller de la réprésentation de la délinquance, de phénomènes climatiques, des résultats d'élections ou encore de l'implantation d'unités. En fait tout phénomène qui peut être rattaché à un échelon territorial existant (commune, département, région...)


<img src="/images/qgis-labelling.jpg" width="400">
<img src="/images/ooo-pivot-final.png" align="left" width="250">


Cette démo vous explique comment, à partir d'un fichier CSV comprenant une colonne département et une colonne compte, on peut facilement colorer une carte dans QGIS.

Pour le tester il suffit de télécharger le projet et d'ouvrir le fichier <b>color_zones.qgs</b>.

## Préparatifs

La première étape est de disposer d'un fichier CSV comprenant les données que l'on souhaite représenter, soit une colonne de référence (j'ai pris les départements pour cette démo) et une colonne qui représente une somme (pour chaque département).

La seconde étape est de trouver un fichier géographique (un fichier shapefile (.shp) généralement) qui contient la donnée de référence, dans notre exemple les numéros de département.

Cela peut être décliné à l'infini à condition de pouvoir faire correspondre la données CSV et la donnée géographique, c'est à dire de pouvoir faire une jointure entre les deux fichiers.

## Ouvrir le fichier de forme

Le fichier de forme (shape) est dans le répertoire datas (en fait plusieurs fichiers). Pour l'ouvrir soit vous déposez le fichier .shp dans l'application QGIS (glissé déposé), soit vous utiliser le bouton "ajouter une couche vecteur".

Le rendu est la carte des déparements sans couleur, sans étiquette.

<img src="/images/qgis-geofla-open.png" width="400"> 

## Ouvrir le CSV

Pour ouvrir le csv, passez par le bouton "ajouter une couche de texte délimité" 
Renseigner les bon paramètres et cliquer sur le bouton radio "pas de géométrie"
Vous pouvez également cocher "surveiller le fichier" si vous souhaitez pouvoir rafraîchir dynamiquement la carte quand le contenu du CSV change.

<img src="/images/qgis-csv-open.jpg" width="400"> 

Le csv apparaît dans la liste des couches. Pour le moment les données ne sont pas visibles.



Ensuite il faut créer la jointure entre ce fichier et le fichier géographique.

## Créer une jointure dans le fichier de forme

Cliquer sur propriétés du fichier de formes et aller dans jointures.
Cliquer sur "+" en bas à droite et sélectionner le fichier csv et les deux colonnes de jointure (département).

<img src="/images/qgis-join-define.png" width="400"> 

Vérifier que la jointure est bien faite en ouvrant la table d'attibuts.

<img src="/images/qgis-button-attributes.png" width="400"> 

Enfin, créer un champ virtuel pour gérer les null dont la valeur n'est pas interprêtée au moment de la définition des styles et de l'étiquetage.

Toujours dans les propriétés de la couche DEPARTEMENT, aller dans Champs puis on clique sur éditer et enfin du calculatrice de champ.

Cliquer sur créer un champ virtuel, nom du champ : "total", type entier et fonction : 

```
CASE WHEN  "ctr_dpt_Sum - COMPTE"   IS NULL 
THEN  0
ELSE  "ctr_dpt_Sum - COMPTE" 
END
```

Soit : 

<img src="/images/qgis-field-virtual.jpg" width="400"> 

Cliquer sur ok et vérifier la présence de la nouvelle colonne "total" et des valeurs récupérées (plus de null en l'occurrence).

## Ajouter le style

Nous souhaitons colorer des zones en fonction de la valeur de la colonne total.

Pour cela, propriété du fichier de forme et cliquer sur style. 
Dans la liste déroulante du haut choisir gradué (cela implique d'utilier une colonne de type entier). 
Choisir la colonne total puis la palette souhaitée et enfin le mode de distribution (quantile par exemple) puis cliquer sur classer.

<img src="/images/qgis-styling.png" width="400"> 

Ce qui donne : 

<img src="/images/qgis-styling-result.jpg" width="400"> 

Il ne reste plus qu'à étiqueter.

## Etiqueter

Toujours dans les propriétés de la couche, aller dans "Etiquettes" puis lister les champs disponibles et choisir celuis que l'on souhaite afficher, le champ "total" dans notre exemple.

<img src="/images/qgis-labelling.jpg" width="700" align="center"> 

Et voilà... 


## Ressources
* [Site du CNRS : Lier des données en fonction de leurs attributs : jointures attributaires](http://www.ades.cnrs.fr/tutoqgis/08_01_jointure_attrib.php)
* [QGIS Tutorials and Tips](http://www.qgistutorials.com/fr/)
* [Les Jointures de tables (JOIN)](http://www.qgistutorials.com/fr/docs/performing_table_joins.html)
* [Site du MEDDE, "jointures attributaires" (en PDF)](http://www.geoinformations.developpement-durable.gouv.fr/fichier/pdf/QGIS_jointure_avec_fichier_externe_cle5beef4.pdf?arg=177828611&cle=3a1f046f0dcb9df9a87a49ae4ab5f1766621b2fc&file=pdf%2FQGIS_jointure_avec_fichier_externe_cle5beef4.pdf)

## Discussions, S.A.V.

Pour discuter de ce tutoriel vous pouvez nous retrouver sur le [forum de la CIMI](http://forum.cimi.ext.minint.fr) (accessible sur le réseau du MININT).




