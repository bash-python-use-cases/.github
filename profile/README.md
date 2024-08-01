# Use cases

## Objectif

L'objectif de cet exercice est d'automatiser le déploiement d'une application WordPress sur deux serveurs distants en utilisant des scripts Bash et Python. Les opérateurs devront configurer et déployer un serveur frontal pour l'application web et un serveur backend pour la base de données, en suivant les meilleures pratiques de DevOps. Les scripts doivent gérer la configuration du serveur, l'installation de logiciels nécessaires, la gestion des mots de passe et la configuration de WordPress. L'installation se réalise sur des serveurs distants déjà approvisionnés.

## Contexte

Vous devez automatiser le déploiement de WordPress sur deux serveurs distincts :

1. **Serveur Frontal** : Hébergera l'application WordPress et sera accessible via HTTPS.
2. **Serveur Backend** : Hébergera la base de données MySQL.

Vous utiliserez des scripts Bash et Python pour accomplir cette tâche. Les scripts doivent prendre en charge les opérations suivantes :

- Mise à jour des serveurs et installation des paquets requis.
- Configuration de la base de données sur le serveur backend.
- Installation et configuration de WordPress sur le serveur frontal en HTTPS.
- Utilisation de `paramiko` pour les connexions SSH dans le script Python.
- Génération et gestion sécurisée des mots de passe.
- Vérification de l'état des services et de l'application WordPress.
- Réalisation d'audits de sécurité.

## Exigences

### Scripts à écrire

1. **deploy_wordpress.sh** : Script Bash pour déployer WordPress.
2. **deploy_wordpress.py** : Script Python pour déployer WordPress.
3. **wp_cli_wrapper.sh** : Script Bash pour gérer les commandes `wp-cli`.

### Fonctionnalités des scripts

- **deploy_wordpress.sh** :
  - Met à jour le système et installe les paquets nécessaires.
  - Configure la base de données sur le serveur backend.
  - Installe et configure le serveur web (Apache ou Nginx) et WordPress sur le serveur frontal.
  - Utilise des variables d'environnement et des fichiers de configuration YAML.
  - Génère des mots de passe forts si non fournis.
  - Vérifie l'état des services et de l'application WordPress.
  - Réalise des audits de sécurité.

- **deploy_wordpress.py** :
  - Fonctionne de manière similaire à `deploy_wordpress.sh` mais en utilisant Python et `paramiko` pour les connexions SSH.

- **wp_cli_wrapper.sh** :
  - Utilise `wp-cli` pour installer et gérer WordPress, ses thèmes et ses plugins.

### Variables de configuration

Les scripts doivent prendre en charge les variables suivantes, définies via un fichier de configuration YAML ou des variables d'environnement :

- `FRONTEND_IP`: Adresse IP du serveur frontal.
- `BACKEND_IP`: Adresse IP du serveur backend.
- `DOMAIN`: Nom de domaine pour le site WordPress.
- `DB_NAME`: Nom de la base de données WordPress.
- `DB_USER`: Utilisateur de la base de données.
- `DB_PASSWORD`: Mot de passe de l'utilisateur de la base de données.
- `DB_ROOT_PASSWORD`: Mot de passe root de la base de données.
- `WP_ADMIN_USER`: Nom d'utilisateur de l'administrateur WordPress.
- `WP_ADMIN_PASSWORD`: Mot de passe de l'administrateur WordPress.
- `WP_ADMIN_EMAIL`: Email de l'administrateur WordPress.
- `WP_THEME`: Thème WordPress à installer.
- `WP_LANGUAGE`: Langue de WordPress.
- `SSH_KEY_PATH`: Chemin de la clé SSH pour les connexions.
- `WEB_SERVER`: Serveur web à utiliser (Apache ou Nginx).
- `LOGFILE`: Chemin du fichier de log.

## Tâches

1. **Écrire et tester les scripts :** Vous devez écrire les scripts décrits ci-dessus et les tester pour vous assurer qu'ils fonctionnent correctement.
2. **Créer un fichier de configuration YAML :** Créez un fichier `config.yml` avec les variables de configuration nécessaires.
3. **Utiliser les scripts pour déployer WordPress :** Utilisez les scripts pour déployer WordPress sur les deux serveurs.
4. **Vérifier l'état des services :** Assurez-vous que les services sont en cours d'exécution et que WordPress est accessible.
5. **Réaliser un audit de sécurité :** Utilisez les scripts pour effectuer un audit de sécurité sur les serveurs.

## Livrables

- Les scripts `deploy_wordpress.sh`, `deploy_wordpress.py`, et `wp_cli_wrapper.sh`.
- Un fichier `config.yml` de configuration.
- Un fichier README.md expliquant comment utiliser les scripts.
- Un fichier log de l'exécution des scripts.

## Évaluation

Vous serez évalués sur :

- La qualité et la clarté du code.
- La conformité aux meilleures pratiques de sécurité.
- La documentation et les instructions fournies dans le README.
- La réussite du déploiement et de la configuration de WordPress.
- La capacité à vérifier l'état des services et à réaliser des audits de sécurité.

## Ressources

- [Documentation WP-CLI](https://wp-cli.org/)
- [Documentation Paramiko](http://docs.paramiko.org/en/stable/)
- [YAML 1.2 Specification](https://yaml.org/spec/1.2/spec.html)

## Menu interactif attendu

```bash
bash deploy_wordpress.sh ./config.yml help
Usage: deploy_wordpress.sh [config_file] [option]
Options:
  deploy                   Deploy WordPress application
  status                   Check status of frontend, backend server and Worpress
  audit                    Perform security audit
  wp                       Use wp-cli wrapper for WordPress management
  load                     Load a different configuration file
  help                     Display this help message
  quit                     Exit the script

Environment Variables:
  FRONTEND_IP              (required) IP address of the frontend server
  BACKEND_IP               (required) IP address of the backend server
  DOMAIN                   (required) Domain name for the WordPress site
  DB_NAME                  (default: wordpress) Name of the WordPress database
  DB_USER                  (default: wp_user) Database user for WordPress
  DB_PASSWORD              (generated if not set) Database user password
  DB_ROOT_PASSWORD         (generated if not set) Database root password
  WP_ADMIN_USER            (default: admin) WordPress admin username
  WP_ADMIN_PASSWORD        (generated if not set) WordPress admin password
  WP_ADMIN_EMAIL           (default: admin@example.com) WordPress admin email
  WP_THEME                 (default: twentynineteen) WordPress theme
  WP_LANGUAGE              (default: fr_FR) WordPress language
  SSH_KEY_PATH             (default: ~/.ssh/id_rsa) SSH private key path
  WEB_SERVER               (default: apache) Web server (apache or nginx)
  LOGFILE                  (default: deploy_wordpress.log) Log file path
```


## Préparer la station de contrôle et les cibles

Sur le contrôleur :

```bash
ssh-keygen -t rsa -b 4096 -f wpadmin -q -N ""
chmod 600 wpadmin
cat wpadmin.pub
```

Sur les cibles (avec cloud-init par exemple)

```bash
#!/bin/bash

# Vérifie si l'utilisateur est root
if [ "$(id -u)" -ne 0 ]; then
    echo "Ce script doit être exécuté en tant que root."
    exit 1
fi

# Variables
USERNAME="wpadmin"
SSH_DIR="/home/${USERNAME}/.ssh"
AUTHORIZED_KEYS="${SSH_DIR}/authorized_keys"
PUBLIC_KEY_CONTENT="ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCn08nrZkYK5EB3RahBN1l10r74H1Er6U0lBmUlRrRJ/PYkQP3F+dLTsj58CRErL0NkMEvyzRszn1gD1VEPaWX2oj8/n0m6Q1lCknEGfyxZaiFGUw6IGxiq/jiscp9oMyYMfvkAqACJJgX8D7G/G/x807zu+1Ts6KlqQoQITkOq+DU5qhc4cN91Z/osoLDgetck3wd18LVNQjWWxtr953/Ftq6hs+P22Lf3zerLpPJkfhXRQXmQ6+SfWYD7guvYqyNLR4Ea/eYTskyYwQRM3dQv95+PP8x2V6OzO8BSCpx0MGxe7BGTmTZ9eS17qLDjhJ752k5gJJvxEQYrzaTXhz/k9FUTHX6ZORSMCCuF3aEn2SrjA/NIwOPuuscLJuriLM0tnrjbpf0q8xK4CQj536sCzsZ2TtpmpNeAGvH7VitWp0Mz1bacUtD0RJfLtCyf1SzPVKHKKQ+n7/aD9PdN9jBjNFFsN4nc6bV92oD4iHlV7nUOeC35nu+UX/lCjCPIkJ4J4jKTZbLcLP+uBWy1LiYNWb3EBu+NenzgfU+9Sfr/Azrk352eVks6dqGdQxmXvSDsTAMi2eqBWCjRCrt1qO9nLLTNAoaZbP25Hunv1V3cla5QQczJoUPyiDH5Ak9lTtMsHorkeGhpHAnpBAwu3p7O6XPS/gQw7ao31R90NTtjqw== francois@osx00.local"

# Créer l'utilisateur wpadmin s'il n'existe pas
if ! id -u "$USERNAME" > /dev/null 2>&1; then
    echo "Création de l'utilisateur ${USERNAME}..."
    useradd -m -s /bin/bash "$USERNAME"
    echo "${USERNAME} ALL=(ALL) NOPASSWD: ALL" > "/etc/sudoers.d/${USERNAME}"
    chmod 440 "/etc/sudoers.d/${USERNAME}"
else
    echo "L'utilisateur ${USERNAME} existe déjà."
fi

# Créer le répertoire .ssh s'il n'existe pas
if [ ! -d "$SSH_DIR" ]; then
    echo "Création du répertoire ${SSH_DIR}..."
    mkdir -p "$SSH_DIR"
    chown "$USERNAME":"$USERNAME" "$SSH_DIR"
    chmod 700 "$SSH_DIR"
fi

# Ajouter la clé publique au fichier authorized_keys
if [ ! -f "$AUTHORIZED_KEYS" ]; then
    echo "Création du fichier ${AUTHORIZED_KEYS} et ajout de la clé publique..."
    echo "$PUBLIC_KEY_CONTENT" > "$AUTHORIZED_KEYS"
    chown "$USERNAME":"$USERNAME" "$AUTHORIZED_KEYS"
    chmod 600 "$AUTHORIZED_KEYS"
else
    if ! grep -q "$PUBLIC_KEY_CONTENT" "$AUTHORIZED_KEYS"; then
        echo "Ajout de la clé publique au fichier ${AUTHORIZED_KEYS}..."
        echo "$PUBLIC_KEY_CONTENT" >> "$AUTHORIZED_KEYS"
        chown "$USERNAME":"$USERNAME" "$AUTHORIZED_KEYS"
        chmod 600 "$AUTHORIZED_KEYS"
    else
        echo "La clé publique est déjà présente dans ${AUTHORIZED_KEYS}."
    fi
fi

echo "Configuration de l'utilisateur ${USERNAME} terminée."
```

Pour se connecter à partir de la station de contrôle :

```bash
ssh -i wpadmin -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null wpadmin@frontend
```

## Installation du Backend

- Définition des variables de configuration
  ```bash
  DB_NAME="wordpress"
  DB_USER="wp_user"
  DB_PASSWORD="dbpassword"
  DB_ROOT_PASSWORD="rootdbpassword"
  FRONTEND_IP="51.158.120.145"
  ```
- Installation des services nécessaires
- Configuration de MariaDB pour écouter sur toutes les interfaces réseau
- Démarrage et activation de Firewalld
- Configuration des règles de pare-feu
- Démarrage et activation de MariaDB
- Sécurisation de l'installation MySQL
- Création de la base de données et des utilisateurs MySQL

```bash
# Set the database name, username, and passwords
DB_NAME="wordpress"
DB_USER="wp_user"
DB_PASSWORD="dbpassword"
DB_ROOT_PASSWORD="rootdbpassword"

# Set the IP address of the frontend server
FRONTEND_IP="51.158.120.145"

# Install MariaDB server and firewalld
sudo dnf install -y mariadb-server firewalld

# Configure MariaDB to listen on all IP addresses
sudo sed -i "s/^#bind-address.*/bind-address=0.0.0.0/g" /etc/my.cnf.d/mariadb-server.cnf

# Start and enable firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Allow incoming connections from the frontend server on port 3306
sudo firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="${FRONTEND_IP}/32" port port="3306" protocol="tcp" accept"

# Allow SSH connections
sudo firewall-cmd --permanent --add-service=ssh

# Reload the firewall rules
sudo firewall-cmd --reload

# Start and enable MariaDB
sudo systemctl start mariadb
sudo systemctl enable mariadb

# Secure the MariaDB installation
echo -e '\ny\n$DB_ROOT_PASSWORD\n$DB_ROOT_PASSWORD\ny\ny\ny\ny\n' | sudo mysql_secure_installation

# Create the database, user, and grant privileges
sudo mysql -p${DB_ROOT_PASSWORD} -e "CREATE DATABASE ${DB_NAME};CREATE USER '${DB_USER}'@'${FRONTEND_IP}' IDENTIFIED BY '${DB_PASSWORD}';CREATE USER '${DB_USER}'@'localhost' IDENTIFIED BY '${DB_PASSWORD}';GRANT ALL PRIVILEGES ON ${DB_NAME}.* TO '${DB_USER}'@'${FRONTEND_IP}';GRANT ALL PRIVILEGES ON ${DB_NAME}.* TO '${DB_USER}'@'localhost';FLUSH PRIVILEGES;"
```

## Installation du Frontend

- Définition des variables de configuration
  ```bash
  FRONTEND_IP="51.158.120.145"
  DOMAIN="apache.${FRONTEND_IP}.nip.io"
  WP_ADMIN_EMAIL="goffinet@goffinet.eu"
  ```
- Installation des dépôts et des paquets nécessaires
- Démarrage et activation de Firewalld
- Configuration des règles de pare-feu
- Création et configuration du fichier de virtual host pour Apache
- Démarrage et activation d'Apache
- Configuration de HTTPS avec Certbot
- Configuration des politiques SELinux pour Apache

```bash
# This script installs and configures Apache web server with PHP support, sets up a virtual host, and obtains an SSL certificate using Let's Encrypt.

FRONTEND_IP="51.158.120.145"  # The IP address of the frontend server.
DOMAIN="apache.${FRONTEND_IP}.nip.io"  # The domain name for the virtual host.
WP_ADMIN_EMAIL="goffinet@goffinet.eu"  # The email address of the WordPress admin.

# Install required packages
sudo dnf install epel-release -y
sudo dnf install -y httpd php php-mysqlnd php-fpm php-json php-gd php-xml php-mbstring wget unzip firewalld certbot python3-certbot-apache

# Start and enable firewalld
sudo systemctl start firewalld
sudo systemctl enable firewalld

# Configure firewall rules
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload

# Create virtual host configuration file
cat << EOF > /tmp/$DOMAIN.conf
<VirtualHost *:80>
  ServerName $DOMAIN
  DocumentRoot /var/www/html
  ErrorLog /var/log/httpd/${DOMAIN}-error_log
  CustomLog /var/log/httpd/${DOMAIN}-access_log common
</VirtualHost>
EOF

# Copy virtual host configuration file to Apache's conf.d directory
sudo cp /tmp/$DOMAIN.conf /etc/httpd/conf.d/$DOMAIN.conf

# Create Apache Log File
sudo touch /var/log/httpd/${DOMAIN}-error_log ; sudo touch /var/log/httpd/${DOMAIN}-access_log

# Start and enable Apache
sudo systemctl start httpd
sudo systemctl enable httpd

# Obtain SSL certificate using Let's Encrypt
sudo certbot --apache -d $DOMAIN --non-interactive --agree-tos --email $WP_ADMIN_EMAIL

# Configure SELinux boolean values for Apache
getsebool -a | grep -E "^httpd_(unified|can_network_connect)?(_db)?\s"
sudo setsebool -P httpd_can_network_connect 1
sudo setsebool -P httpd_can_network_connect_db 1
sudo setsebool -P httpd_unified 1
getsebool -a | grep -E "^httpd_(unified|can_network_connect)?(_db)?\s"
```

## Installation de wp-cli et configuration de WordPress

- Définition des variables de configuration
  ```bash
  FRONTEND_IP="51.158.120.145"
  DOMAIN="apache.${FRONTEND_IP}.nip.io"
  WP_ADMIN_EMAIL="goffinet@goffinet.eu"
  ```
- Téléchargement et installation de WP-CLI 
- Configuration des permissions du répertoire web
- Téléchargement et configuration de WordPress
- Création du fichier de configuration WordPress
- Installation de WordPress 
- Installation et activation de la langue WordPress
- Installation et activation du thème WordPress

```bash
# Set the IP addresses and domain name
FRONTEND_IP="51.158.120.145"
BACKEND_IP="163.172.169.55"
DOMAIN="apache.${FRONTEND_IP}.nip.io"

# Set the database details
DB_NAME="wordpress"
DB_USER="wp_user"
DB_PASSWORD="dbpassword"

# Set the WordPress admin details
WP_ADMIN_USER="admin"
WP_ADMIN_PASSWORD="admin1234admin"
WP_ADMIN_EMAIL="goffinet@goffinet.eu"

# Set the WordPress language and theme
WP_LANGUAGE="fr_FR"
WP_THEME="twentynineteen"

# Download and install WP-CLI
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar 
php wp-cli.phar --info 
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/bin/wp

# Set permissions for Apache
sudo chown apache:apache -R /var/www/html
sudo chmod 755 -R /var/www/html

# Download WordPress core
sudo -u apache wp core download --path=/var/www/html

# Create the wp-config.php file
sudo -u apache wp config create --dbname=${DB_NAME} --dbuser=${DB_USER} --dbpass=${DB_PASSWORD} --path=/var/www/html --dbhost=${BACKEND_IP}

# Install WordPress
sudo -u apache wp core install --url=$DOMAIN --title='WordPress Site' --admin_user=$WP_ADMIN_USER --admin_password=$WP_ADMIN_PASSWORD --admin_email=$WP_ADMIN_EMAIL --skip-email --path=/var/www/html/

# Install and activate the specified language
sudo -u apache wp core language install $WP_LANGUAGE --path=/var/www/html/
sudo -u apache wp core language activate $WP_LANGUAGE --path=/var/www/html/

# Install and activate the specified theme
sudo -u apache wp theme install $WP_THEME --path=/var/www/html/
sudo -u apache wp theme activate $WP_THEME --path=/var/www/html/
```

## Audit

```bash
sudo yum install -y openscap-scanner scap-security-guide
sudo oscap xccdf eval --profile xccdf_org.ssgproject.content_profile_anssi_bp28_intermediary --results /tmp/results.xml /usr/share/xml/scap/ssg/content/ssg-rhel9-ds.xml
sudo cat /tmp/results.xml
```

## Analyse du script `deploy_wordpress.sh`

Ce script Bash permetra de déployer une application WordPress sur deux serveurs distants : un serveur frontend pour héberger l'application web et un serveur backend pour la base de données. Il offre également des fonctionnalités de gestion et de vérification de l'état des serveurs et de l'application. Voici une description détaillée de ses fonctionnalités :

### 1. Définition des variables de configuration par défaut

- Le script commence par définir une variable `CONFIG_FILE` pour le fichier de configuration YAML à utiliser, en prenant comme valeur par défaut `config.yml`.

### 2. Génération de mots de passe forts

- La fonction `generate_password` utilise OpenSSL pour générer des mots de passe aléatoires et sécurisés.

### 3. Parsing du fichier YAML et configuration des variables d'environnement

- La fonction `parse_yaml` utilise Python pour lire le fichier YAML et exporter les variables d'environnement.
- La fonction `load_config` charge les configurations à partir du fichier YAML en utilisant `parse_yaml` ou en utilisant les variables d'environnement uniquement si le fichier YAML n'est pas trouvé.

### 4. Chargement des variables d'environnement

- La fonction `load_env_vars` charge les variables d'environnement et définit des valeurs par défaut pour celles qui ne sont pas spécifiées.

### 5. Fonctions utilitaires

- `log`: Journalise les messages avec un horodatage.
- `ssh_execute_command`: Exécute des commandes SSH sur un serveur distant.
- `check_services_status`: Vérifie l'état des services sur un serveur.
- `check_wordpress_status`: Vérifie l'état de l'application WordPress en envoyant une requête HTTP.

### 6. Déploiement du serveur backend

- La fonction `setup_backend` installe et configure MariaDB et Firewalld sur le serveur backend. Elle crée également la base de données et les utilisateurs nécessaires pour WordPress.

### 7. Déploiement du serveur frontend

- La fonction `setup_frontend` installe et configure le serveur web (Apache ou Nginx), PHP, et d'autres dépendances. Elle configure également SSL avec Certbot et configure WordPress à l'aide de WP-CLI.

### 8. Audit de sécurité

- La fonction `perform_audit` exécute un audit de sécurité OWASP/OSCAP sur les serveurs.

### 9. Affichage de l'aide et du menu interactif

- `display_help`: Affiche un message d'aide avec les options disponibles.
- `display_menu`: Affiche un menu interactif pour que l'utilisateur puisse choisir une action.

## 10. Fonction principale

- `main`: Gère les différentes options du script (déploiement, vérification de l'état, audit, gestion avec WP-CLI, chargement d'un fichier de configuration, affichage de l'aide).
