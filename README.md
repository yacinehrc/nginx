# Serveur Web Nginx — Docker

![Bureau Ubuntu](https://github.com/user-attachments/assets/befcf761-b290-4b41-9631-2aa6c15ab36c)

> Déploiement d'un serveur web Nginx conteneurisé via Docker Compose sur une machine virtuelle Ubuntu — avec page HTML/CSS personnalisée servie en local sur le port 8080.

---

## Introduction

Ce projet consiste à déployer un **serveur web Nginx** dans un conteneur **Docker** sur une machine virtuelle **Ubuntu**, en utilisant **Docker Compose** pour orchestrer le service. L'objectif était de découvrir la conteneurisation, de comprendre le fonctionnement d'un serveur web et de servir une page HTML statique accessible depuis le navigateur via l'adresse IP de la VM. Le projet a été réalisé entièrement en ligne de commande.

---

## Fonctionnalités

- **Conteneur Nginx** : serveur web isolé dans un conteneur Docker, image officielle `nginx:latest`
- **Mapping de port** : le port 8080 de la machine hôte est mappé sur le port 80 du conteneur
- **Volume monté** : le dossier `./html` local est monté en lecture seule dans le conteneur (`/usr/share/nginx/html:ro`)
- **Redémarrage automatique** : le conteneur redémarre toujours automatiquement (`restart: always`)
- **Page HTML personnalisée** : page de validation "Serveur web maintenant fonctionnel !" avec CSS dédié

---

## Démonstration

### Mise à jour du système

Avant toute installation, mise à jour des paquets du système Ubuntu.

```bash
sudo apt update && sudo apt upgrade -y
```

![Commande apt update](https://github.com/user-attachments/assets/8f6740e9-2e0b-49ff-86d6-041d5fd807a4)

![Résultat apt upgrade](https://github.com/user-attachments/assets/5daf1de1-054a-4d51-8613-59f78b84b28f)

### Installation de Docker et Docker Compose

```bash
sudo apt install docker.io docker-compose -y
```

![Commande installation Docker](https://github.com/user-attachments/assets/3d0c05e0-935f-49d7-84e0-f78af374e746)

![Docker installé avec succès](https://github.com/user-attachments/assets/ad250183-d11c-4495-b095-0c1b54563382)

### Création de la structure de dossiers

```bash
mkdir -p ~/srv-nginx/html
cd ~/srv-nginx
mkdir -p html
```

![Création du dossier srv-nginx](https://github.com/user-attachments/assets/ec6cd28f-3a56-4751-9901-b368cc7d3eb6)

![Navigation dans srv-nginx](https://github.com/user-attachments/assets/02de3d14-83d6-49d0-a122-f8a6c011dcad)

### Configuration du docker-compose.yml

```bash
nano docker-compose.yml
```

Le fichier `docker-compose.yml` définit le service Nginx avec le port, le volume et la politique de redémarrage.

![Contenu du docker-compose.yml](https://github.com/user-attachments/assets/c2b61824-8e61-4150-8c7c-58d6990d4920)

```yaml
version: '3.8'

services:
  web:
    image: nginx:latest
    container_name: serveur-web
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    restart: always
```

### Démarrage du conteneur

```bash
sudo docker-compose up -d
```

```bash
sudo docker compose up -d
```

![Lancement docker-compose up -d](https://github.com/user-attachments/assets/ca61638f-9d74-4882-86d9-da54964cf6f3)

![Lancement docker compose up -d](https://github.com/user-attachments/assets/195bd9a3-1add-45bc-baf0-22760004639c)

Docker télécharge l'image Nginx, crée le réseau `srv-nginx_default` et démarre le conteneur `serveur-web`.

![Pull complet et conteneur démarré](https://github.com/user-attachments/assets/a8df39cb-3f1a-4ad1-917e-dbd29ffc6845)

### Vérification de la structure de fichiers

Le dossier `srv-nginx` contient le `docker-compose.yml` et le dossier `html`.

![Dossier personnel avec srv-nginx](https://github.com/user-attachments/assets/823bdd37-bb89-4fe2-8e4b-b270627cac9b)

![Contenu de srv-nginx](https://github.com/user-attachments/assets/9c3274d6-c659-473c-8475-a03ef9d31438)

### Création de la page HTML

```bash
nano ~/srv-nginx/html/index.html
```

![Commande nano index.html](https://github.com/user-attachments/assets/8201a221-8109-4a15-a83c-ce1ff8660db1)

![Contenu de index.html](https://github.com/user-attachments/assets/ef80c98d-eb14-4294-aa2c-e59ed7c2acd8)

Le fichier CSS est également créé pour styliser la page de validation.

![Contenu de style.css](https://github.com/user-attachments/assets/e9b41493-2eaf-48f2-a516-ab72cabf6bab)

Le dossier `html` contient les deux fichiers servis par Nginx.

![Dossier html avec index.html et style.css](https://github.com/user-attachments/assets/bc8a9a33-52ac-4d77-b0b5-5048b91bca8f)

### Récupération de l'adresse IP

```bash
ip a
```

![Commande ip a](https://github.com/user-attachments/assets/9f195ced-e3c7-4f5e-afad-5ce9ef4bdd18)

L'adresse IP de la VM est `192.168.91.129`, le serveur est accessible sur `http://192.168.91.129:8080`.

![Résultat ip a avec l'IP 192.168.91.129](https://github.com/user-attachments/assets/79858e9d-8960-4a48-83ea-06167d58982e)

### Résultat final — Page servie par Nginx

La page HTML s'affiche dans le navigateur à l'adresse `http://192.168.91.129:8080`, confirmant que le serveur Nginx fonctionne correctement dans son conteneur Docker.

![Page web affichée dans le navigateur](https://github.com/user-attachments/assets/ace91972-1852-4317-92f7-1ada795bf348)

---

## Conclusion

Ce projet a permis de découvrir les bases de la **conteneurisation** avec Docker et d'orchestrer un service web avec **Docker Compose**. La mise en place du volume monté, du mapping de port et de la politique de redémarrage automatique constitue une introduction concrète aux bonnes pratiques de déploiement d'applications en environnement isolé.

---

*Réalisé par [Yacine Harrache](https://github.com/yacinehrc) — BTS SIO SLAM | EPSI Lille*
