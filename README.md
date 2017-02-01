QualitéAir
==========

Le script `qualiteair` permet de récupérer l’indice de qualité de l’air pour
les villes de Normandie. L’information est récupérée depuis le site de
l’association Air Normand par du web scraping.

Utilisation
-----------

Affiche l’aide :

    qualiteair

Affiche l’indice de qualité de l’air pour Rouen :

    qualiteair indice "Rouen - 76"

Affiche les données JSON de qualité de l’air pour Le Havre :

    qualiteair json "Le Havre - 76"

Indice de la qualité de l’air
-----------------------------

L’indice prend des valeurs de 0 à 5 :

- **0** : très mauvais
- **1** : mauvais
- **2** : médiocre
- **3** : moyen
- **4** : bon
- **5** : très bon

La structure JSON
-----------------

Exemple de structure JSON retournée :

    {
        "collecte": "2017-02-01T19:03:16+01:00",
        "date": "2017-02-01",
        "maj": "2017-02-01",
        "indice": 5
    }

Les champs ont la signification suivante :

- **collecte** : date de la collecte des informations par le script
- **date** : date de validité de l’indice
- **maj** : date d’élaboration de l’indice
- **indice** : indice de la qualité de l’air pour la ville sélectionnée

Note
----

La liste des codes de ville connus par Air Normand au 1er février 2017 se trouve
dans le fichier `communes-2017-02-01.txt`.

Proxy
-----

Le script `qualiteair` utilise cURL pour la récupération de la page web.

Pour utiliser le script derrière un proxy, il suffit de positionner la variable
d’environnement `http_proxy` à la bonne valeur.

Installation
------------

À la main ! :o)

Prérequis
---------

Assurez-vous que le script `qualiteair` soit exécutable et que les fichiers
d’extension `xsls` se trouvent dans le même répertoire que ce script.

Les programmes suivants sont requis par `qualiteair` :

- cURL
- xsltproc
- xslclearer (Python 3)

### Installation de xsltproc

Sous Debian :

    sudo apt-get install xsltproc

### Installation de xslclearer

XSLClearer est un projet personnel hébergé sur GitHub.

    wget https://github.com/Zigazou/xslclearer/archive/master.zip
    unzip xslclearer-master.zip
    cd xslclearer-master
    sudo python3 setup.py install

Une fois installé, deux nouvelles commandes sont disponibles : `xslsproc` et
`xslclearer`.

Technique
---------

Le script utilise une feuille de style XSL pour faire du scraping sur les pages
web du site de l’association Air Normand.

La liste des communes a été récupérée avec le script suivant :

    for letter in {a..z}
    do
      curl "http://www.airnormand.fr/dyn/cgi/action.php?Action=LIST_COMMUNES&q=$letter&limit=5000"
    done > communes.txt

