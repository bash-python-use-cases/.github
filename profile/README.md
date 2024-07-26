# Automatiser l'installation d'un CMS en BASH/PYTHON

## Application

[Wordpress](https://wordpress.org/download/)

## Architecture de l'application

Le script doit supporter la dernière distribution RedHat-like stable sur deux serveurs distants joignables en SSH :

- [ ] Un serveur Linux frontal qui héberge l'application en HTTPS ([Let's Enc](https://letsencrypt.org/), [https://nip.io/](https://nip.io/), [https://traefik.me/](https://traefik.me/), etc.).
- [ ] Un serveur Linux de Backend qui héberge la base de données.

Le pare-feu local de chaque serveur doit être strictement configuré :

- [ ] Le serveur frontal doit être accessible en HTTPS uniquement.
- [ ] Le serveur de DB doit être uniquement accessible du serveur frontal.
- [ ] Pour des raisons d'infrastructure, on ouvrira aussi les ports de gestion et de d'infrastructure comme SSH ou DNS.

Une architecture constituée de plusieurs serveurs de DB avec un gestionnaire de cache et un _load balancer_ devant plusieurs serveurs Web n'est pas envisagée dans cette étude de cas. Dans ce cas, c'est le load balancer frontal qui s'occupe du chiffrement et du routage public.

L'administration courante du serveur, l'ajout d'élément d'infrastucture, de sécurité ou de surveillance (WAF/IDS/APM/SIEM/AV) ne fait pas partie de l'énoncé.

## Consignes et fonctionnalités attendues

- [ ] Abstraire un maximum les paramètres (nom de domaines, langue à installer, versions, etc.).
- [ ] Abstraire au maximum la logique (fonctions, modèle de données, etc.).
- [ ] Implémenter la gestion des erreurs et le logging.
- [ ] Implémenter un menu interactif qui permet d'interroger les serveurs sur leur statut.
- [ ] Le code devrait être exécuté à la demande en fournissant une source de données et/ou des paramètres en ligne de commande.
- [ ] Le client final reçoit un url, un nom d'utilisateur et un mot de passe l'invitant à se connecter à son nouveau serveur Wordpress.
- [ ] La configuration doit être sécurisée selon les bonnes pratiques (mysql_secure, moindre privilèges, secrets forts, chiffrement asymétriques, et pourquoi pas audit OWASP/OSCAP automatisé, etc.).
- [ ] Le script devrait fonctionner sur une seule machine avec les bons paramètres.
- [ ] Optionnellement, vous pouvez automatiser l'approvisionnement des deux hôtes qui hébergent l'application.
- [ ] Le choix d'installation de plugins ou de thèmes supplémentaires peut être envisagée.
- [ ] Si nécessaire, vous pouvez diviser votre script en plusieurs fichiers.

## Livraison du code source

- [ ] Code régulièrement mis à jour sur un repository [https://github.com/bash-python-use-cases](https://github.com/bash-python-use-cases) au plus tard à la fin du module.
- [ ] README.md explique l'usage du script et sa démonstration fonctionnelle.
- [ ] Toujours collaboratif.
- [ ] Seul par défaut.
- [ ] En équipe de deux, chacun s'occupant d'un serveur et collaborant pour fournir une solution unifiée.
- [ ] En équipe de plus de deux si vous organisez votre travail à l'aide d'un tableau [Github Projects](https://docs.github.com/fr/issues/planning-and-tracking-with-projects/learning-about-projects/quickstart-for-projects).
- [ ] Chacun doit montrer ses contributions personnelles par ses commits.

## Intelligence artificielle et citation des sources

Veuillez mentionner que votre code est original quand vous n'avez pas utilisé l'IA. Si vous avez utilisé l'IA, veuillez le mentionner également. Il s'agit d'un choix que vous devriez faire dès que vous commencez votre projet.

Prenez garde de ne pas vous laisser distraire par l'IA. D'ailleurs, on est souvent seul face à l'IA. Le bon usage de l'IA exige non seulement déjà une bonne connaissance du sujet mais aussi des compétences nouvelles et spécifiques.

Dans tous les cas, veuillez faire confiance à votre propre intelligence et à celle de vos propres collaborateurs. Cela en vaut la peine.

Enfin, merci de citer vos sources d'inspiration.

## Validation de la solution

Une seule commande qui déploie automatiquement une application Wordpress prête à l'emploi : le client (l'animateur de la formation) reçoit un message avec url, un nom d'utilisateur et un mot de passe l'invitant à se connecter à son nouveau serveur Wordpress en HTTPS. Celui-ci arrive directement dans le Dashboard d'administration de l'application.

## Améliorations et alternatives

Veuillez considérer les remarques ci-dessus sur l'architecture et l'administration de l'application.

Les alternatives de déploiement pourraient être :

- Ansible playbooks
- Terraform plans
- DockerCompose
- Kubernetes manifests (custom images, Helm Charts, Kustomize,...)

## Compétences et matériel nécessaire

- Une ou deux instances linux à "spawner" locale ou dans le cloud.
- Les rudiments de l'administration système Linux.
- Les rudiments du scripting Bash et Python.
- Les rudiments git/github et vi.

## Point de départ

[How to Install WordPress on Linux Server: A Step by Step Guide](https://hackernoon.com/how-to-install-wordpress-on-linux-server-a-step-by-step-guide)
