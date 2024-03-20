Dans la démonstration on voudrait appuyer sur la valeur ajouté d'un serveur Redis fonctionnant comme un serveur cache couplé avec dynamoDB.

Suite à une GET requête le serveur demande en premiers temps à la base de donnée Redis, si l'information existe dans le cache Redis, le serveur termine la requête sinon le serveur demande à la base de donnée persistante dynamoDB ensuite, enregistre la réponse dans le cache Redis et enfin termine la requête.

Au démarrage de serveur, le cache ( base de donnée Redis ) est vide puisque il fonctionne sur une mémoire volatile. On suppose qu'on a 1000 requêtes qui demande un item parmi les items enregistré dans dynamoDB.

Nous avons comparé les temps de réponses du serveur dynamoDB sans Redis et avec Redis. On remarque qu'au début les temps de réponses sont assez élevé c'est la période au bout de laquelle Redis enregistre les réponses des requêtes dans le cache. et après avoir enregistré la plupart des requêtes, les temps de réponses diminue significativement. de 5x10-3 à 1x10-3 c'est 5 fois plus de performance.

