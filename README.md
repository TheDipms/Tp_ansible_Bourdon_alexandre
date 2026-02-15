Ansible PostgreSQL Docker TP

Ce projet implémente l'automatisation de l'installation et de la configuration d'un serveur PostgreSQL sur une architecture conteneurisée. Le projet utilise Ansible pour configurer des nœuds (conteneurs Docker) simulant des serveurs distants.
🏗️ Architecture

L'infrastructure est composée de 3 nœuds simulés par Docker :

    web1 / web2 : Serveurs d'application.

    db1 : Serveur de base de données (exécute PostgreSQL).

    Note : Les conteneurs agissent comme des hôtes distants via SSH. PostgreSQL est installé en tant que service système à l'intérieur du conteneur ansible-db1.

🚀 Fonctionnalités du rôle PostgreSQL

    Installation des paquets PostgreSQL.

    Configuration du service (démarrage et activation).

    Création d'une base de données (optionnel).

    Création automatique de tables via le module postgresql_query.

🛠️ Configuration du Rôle
Dépendances requises

Pour que le module PostgreSQL d'Ansible fonctionne sur le nœud db1, les paquets suivants sont installés par le playbook :

    python3-psycopg2 (sur le host distant)

    community.postgresql (collection Ansible)

Exemple de tâche : Création d'une table

La création de table s'effectue en utilisant l'authentification Peer (locale) de PostgreSQL :
YAML

- name: Créer la table utilisateurs
  community.postgresql.postgresql_query:
    db: postgres
    login_user: postgres
    query: |
      CREATE TABLE IF NOT EXISTS utilisateurs (
          id SERIAL PRIMARY KEY,
          nom VARCHAR(100),
          email VARCHAR(100) UNIQUE NOT NULL
      )
  become: true
  become_user: postgres
  when: inventory_hostname == 'db1'

📋 Utilisation

    Lancer l'infrastructure Docker :
    Bash

    docker ps  # Vérifier que ansible-db1 est "Up"

    Exécuter le Playbook :
    Bash

    ansible-playbook playbooks/main.yml
    test Unitaires
    ansible-playbook test_db.yml
    ansible-playbook test_nginx.yml 
    test_node.yml

