# Automatiser l'installation d'un CMS en BASH/PYTHON

## Produit

Wordpress

## Architecture

Le script doit supporter la dernière distribution RedHat-like stable sur deux serveurs distants joignables en SSH :

- Un serveur Linux frontal qui héberge l'application en HTTPS ([Let's Enc](https://letsencrypt.org/), [https://nip.io/](https://nip.io/), [https://traefik.me/](https://traefik.me/), etc.)
- Un serveur Linux de Backend qui héberge la base de données

Le pare-feu local de chaque serveur doit être strictement configuré.

Une architecture constituée de plusieurs serveurs de DB avec un gestionnaire de cache et un _load balancer_ devant plusieurs serveurs Web n'est pas envisagée dans cette étude de cas.

## Consignes

- abstraire un maximum les paramètres (nom de domaines, langue à installer, versions, etc.)
- abstraire au maximum la logique (fonctions, modèle de données, etc.)
- implémenter la gestion des erreurs et le logging
- implémenter un menu interactif qui permet d'interroger les serveurs sur leur statut
- le code devrait être exécuté à la demande en fournissant une source de données et/ou des paramètres en ligne de commande
- optionnellement, vous pouvez automatiser l'approvisionnement des deux hôtes qui hébergent l'application

## Livraison

- code régulièrement mis à jour sur un repository [https://github.com/bash-python-use-cases](https://github.com/bash-python-use-cases) au plus tard à la fin du module.

## Intelligence artificielle

Veuillez mentionner que votre code est original quand vous n'avez pas utilisé l'IA. 

Si vous avez utilisé l'IA, veuillez le mentionner également.

## Point de départ

[How to Install WordPress on Linux Server: A Step by Step Guide](https://hackernoon.com/how-to-install-wordpress-on-linux-server-a-step-by-step-guide)
