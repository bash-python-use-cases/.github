# Automatiser l'installation d'un CMS en BASH/PYTHON

## Application

[Wordpress](https://wordpress.org/download/)

## Architecture

Le script doit supporter la dernière distribution RedHat-like stable sur deux serveurs distants joignables en SSH :

- Un serveur Linux frontal qui héberge l'application en HTTPS ([Let's Enc](https://letsencrypt.org/), [https://nip.io/](https://nip.io/), [https://traefik.me/](https://traefik.me/), etc.)
- Un serveur Linux de Backend qui héberge la base de données

Le pare-feu local de chaque serveur doit être strictement configuré :

- Le serveur frontal doit être accessible en HTTPS uniquement.
- Le serveur de DB doit être uniquement acessible du serveur frontal.
- Pour des raisons d'infrastructure, on ouvrira aussi les ports de gestion et de d'infrastructure comme SSH ou DNS.

Une architecture constituée de plusieurs serveurs de DB avec un gestionnaire de cache et un _load balancer_ devant plusieurs serveurs Web n'est pas envisagée dans cette étude de cas. Dans ce cas, c'est le load balancer frontal qui s'occupe du chiffrement et du routage public.

L'administration courante du serveur, l'ajout d'élément d'infrastucture, de sécurité ou de surveillance (WAF/IDS/APM/SIEM/AV) ne fait pas partie de l'énoncé.

## Consignes / fonctionnalité

- abstraire un maximum les paramètres (nom de domaines, langue à installer, versions, etc.)
- abstraire au maximum la logique (fonctions, modèle de données, etc.)
- implémenter la gestion des erreurs et le logging
- implémenter un menu interactif qui permet d'interroger les serveurs sur leur statut
- le code devrait être exécuté à la demande en fournissant une source de données et/ou des paramètres en ligne de commande
- le client reçoit un url, un nom d'utilisateur et un mot de passe l'invitant à se connecter à son nouveau serveur Worpress
- la configuration doit être sécurisée selon les bonnes pratiques (mysql_secure, moindre privilèges, secrets forts, chiffrement asymétriques, et pourquoi pas audit OWASP/OSCAP automatisé, etc.)
- optionnellement, vous pouvez automatiser l'approvisionnement des deux hôtes qui hébergent l'application
- le choix d'installation de plugins ou de thèmes supplémentaires peut être envisagée
- si nécessaire, vous pouvez diviser votre script en plusieurs fichiers

## Livraison

- Code régulièrement mis à jour sur un repository [https://github.com/bash-python-use-cases](https://github.com/bash-python-use-cases) au plus tard à la fin du module.
- README.md explique l'usage du script et sa démonstration fonctionnelle.
- Toujours collaboratif.
- Seul par défaut.
- En équipe de deux, chacun s'occupant d'un serveur et collaborant pour fournir une solution unifiée.
- En équipe de plus de deux si vous organisez votre travail à l'aide d'un tableau [Github Projects](https://docs.github.com/fr/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects). Chacun doit montrer ses contributions par ses commits.

## Intelligence artificielle

Veuillez mentionner que votre code est original quand vous n'avez pas utilisé l'IA. 

Si vous avez utilisé l'IA, veuillez le mentionner également.

Il s'agit d'un choix que vous devriez faire dès que vous commencez votre projet.

Prenez garde de ne pas vous laisser distraire par l'IA. D'ailleurs, on est souvent seul face à l'IA. Le bon usage de l'IA exige non seulement déjà une bonne connaissance du sujet mais aussi des compétences nouvelles et spécifiques.

Dans tous les cas, veuillez faire confiance à votre propre intelligence et à celle de vos propres collaborateurs. Cela en vaut la peine.

## Validation

Une seule commande qui déploie automatiquement une application Wordpress prête à l'emploi : le client (l'animateur de la formation) reçoit un message avec url, un nom d'utilisateur et un mot de passe l'invitant à se connecter à son nouveau serveur Worpress. Celui-ci arrive directement dans le Dashboard de l'application.

## Améliorations et alternatives

Veillez les remarques ci-dessus sur l'architecture.

Les alternatives de déploiement pourraient être :

- Ansible playbooks
- Terraform plans
- DockerCompose
- Kubernetes manifests (custom images, Helm Charts, Kustomize,...)

## Point de départ

[How to Install WordPress on Linux Server: A Step by Step Guide](https://hackernoon.com/how-to-install-wordpress-on-linux-server-a-step-by-step-guide)
