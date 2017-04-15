# Installation de la base de donn�es

La structure de la base de donn�es se trouve sous 'data/sql/install.sql'. Charger le fichier.
J'ai choisi le moteur MyISAM pour ses performances. InnoDb n'est n�cessaire dans ce cas.

# Pr�-requis

Modifier le fichier 'config/db.php' pour renseigner les bonnes valeurs de connexion � la base de donn�es.

# Informations

Les scripts sont tous en error_reporting (E_ALL | E_STRICT), memory_limit � 128M et max_execution_time � 30 secondes. La micro-application ne traite pas du sujet de l'authentification et des autorisations, du log des lignes erron�es du fichier csv (qui peut �tre un axe d'am�lioration par la suite), et ne g�re pas les lev�es d'exceptions. 

# Charger le fichier 'tickets_appels_201202.csv'

Avant de continuer, imaginons comment nous recevons ces donn�es d'un partenaire. Un partenaire nous a transmi ce fichier par flux CFT. Le contr�le de fin de transfert le positionne dans un r�pertoire pour traitement.

Le r�pertoire est le suivant : 'data/csv/tickets_appels_201202.csv'.

Un cron inspecte ce r�pertoire pour savoir si des donn�es sont � traiter. Si oui, il lance le script 'load.php'. Pour des raisons de facilit�, ce script est modifi� pour prendre en compte directement le fichier au lieu d'inspecter le r�pertoire. Il ne traite pas de la suppression de ce dernier.

Pour charger le fichier, se positionner � la racine du r�pertoire :

```sh
$ php load.php
```

Les pr�-requis (� savoir conserver la directive memory_limit � 128M et max_execution_time � 30 secondes) sont respect�s.

La consommation m�moire et le temps d'ex�cution sont affich�es � la fin du script. Si besoin, les pr�-requis peuvent �tre vu � la baisse pour ex�cuter le script.

Le script filtre et valide les entr�es afin d'�viter toutes injections et/ou XSS, r�f�rences indirectes, etc... (OWASP Top 10).

NB : ce script peut �tre am�lior� en utilisant un adapteur permettant de rendre abstrait le type de fichier. De cette mani�re, n'importe quel csv (par exemple, dans notre cas, nous avons des Tickets, mais on peut aussi  bien avoir autre chose)  pourraient �tre interpr�t� par ce script )

NB2 : aucun tests unitaires n'est fournis.

# Very very micro-application

Pour la gestion des fonctionnalit�s demand�es, se positionner � la racine.

```sh
$ php -S 0.0.0.0:8000 -t public
```

Lancer le navigateur et atteignez la page suivante : http://localhost:8000. 

# Dump de la base de donn�es r�sultante (Optionnel)

Un dump est fournit sous 'data/sql/dump.sql'.