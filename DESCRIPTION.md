Sur la vm je créé le dossier gitlab

mkdir gitlab-docker-server
cd gitlab-docker-server
Ensuite, dans le fichier docker-compose.yml dans le dossier gitlab, j'ai mis l'addresse de ma vm 

version: '3.7'
volumes:
  logs:
    driver: local
  config:
    driver: local
  data:
    driver: local
  runner:
    driver: local
services:
  gitlab:
    restart: unless-stopped
    image: gitlab/gitlab-ce:latest
    container_name: gitlab
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        gitlab_rails['gitlab_shell_ssh_port'] = 2222
        external_url 'http://<ip>'
    ports:
      - '80:80'
      - '2222:22'
    volumes:
      - 'data:/var/opt/gitlab'
      - 'config:/etc/gitlab'
      - 'logs:/var/log/gitlab'
  gitlab-runner:
    restart: unless-stopped
    image: gitlab/gitlab-runner
    container_name: gitlab-runner
    volumes:
      - '/var/run/docker.sock:/var/run/docker.sock'
      - 'runner:/etc/gitlab-runner'
      - 'config:/etc/gitlab'
Lancez la commande suivante pour lancer les conteneurs :

docker-compose up -d 
