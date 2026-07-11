# Serveur Web Nginx — Docker

<img width="1919" height="988" alt="Capture d&#39;écran 2026-07-11 094717" src="https://github.com/user-attachments/assets/cd145866-2ecc-4aef-9bcd-57d304348e03" />

> Déploiement d'un serveur web Nginx conteneurisé via Docker Compose sur une machine virtuelle Ubuntu — avec page HTML/CSS personnalisée servie en local sur le port 8080.

---

## Introduction

Ce projet consiste à déployer un **serveur web Nginx** dans un conteneur **Docker** sur une machine virtuelle (ici, le projet sera réalisé avec la distribution **Ubuntu**), en utilisant **Docker Compose** pour orchestrer le service. L'objectif était de découvrir la conteneurisation, de comprendre le fonctionnement d'un serveur web et de servir une page HTML statique accessible depuis le navigateur via l'adresse IP de la VM. Le projet a été réalisé entièrement en ligne de commande.

> 💡 **Prérequis — Installation d'Ubuntu :** Si vous partez de zéro, un guide complet d'installation d'Ubuntu Desktop (en VM VMware ou sur PC physique) est disponible ici : **[yacinehrc/Ubuntu-GNOME-Desktop](https://github.com/yacinehrc/Ubuntu-GNOME-Desktop)**

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

<img width="1919" height="912" alt="Capture d&#39;écran 2026-07-11 092844" src="https://github.com/user-attachments/assets/6ae07359-58aa-4f40-a660-32864dc7e682" />

### Installation de Docker et Docker Compose

```bash
sudo apt install docker.io docker-compose -y
```

<img width="1919" height="913" alt="Capture d&#39;écran 2026-07-11 093017" src="https://github.com/user-attachments/assets/532d8921-fe0b-42da-975c-edc3e01c7595" />

### Création de la structure de dossiers

```bash
mkdir -p ~/srv-nginx/html
cd ~/srv-nginx
mkdir -p html
```

<img width="1919" height="909" alt="Capture d&#39;écran 2026-07-11 093046" src="https://github.com/user-attachments/assets/6a033d9c-81e5-496d-a255-46293f59b31b" />

<img width="1919" height="916" alt="Capture d&#39;écran 2026-07-11 093106" src="https://github.com/user-attachments/assets/f3feb777-a828-442f-8b2d-ce4ba0ca509a" />

<img width="1919" height="903" alt="Capture d&#39;écran 2026-07-11 093124" src="https://github.com/user-attachments/assets/db981d5f-4147-43cf-a471-19e4784c219b" />

### Configuration du docker-compose.yml

```bash
nano docker-compose.yml
```

Le fichier `docker-compose.yml` définit le service Nginx avec le port, le volume et la politique de redémarrage.

<img width="1919" height="909" alt="Capture d&#39;écran 2026-07-11 093155" src="https://github.com/user-attachments/assets/44a64570-68e9-4bd1-90e6-df2c1da5943e" />

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

<img width="1919" height="918" alt="Capture d&#39;écran 2026-07-11 093329" src="https://github.com/user-attachments/assets/064caee5-6cec-4889-a19c-7767fa5d4cb1" />

### Démarrage du conteneur

```bash
sudo docker <img width="1919" height="907" alt="Capture d&#39;écran 2026-07-11 093432" src="https://github.com/user-attachments/assets/4657b6c0-c8eb-4dd1-9823-2b34bbe9f171" />
compose up -d
```

```bash
sudo docker compose up -d
```

![Uploading Capture d'écran 2026-07-11 093432.png…]()

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
