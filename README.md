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
sudo docker compose up -d
```
<img width="1919" height="907" alt="Capture d&#39;écran 2026-07-11 093432" src="https://github.com/user-attachments/assets/4657b6c0-c8eb-4dd1-9823-2b34bbe9f171" />

Docker télécharge l'image Nginx, crée le réseau `srv-nginx_default` et démarre le conteneur `serveur-web`.

### Vérification de la structure de fichiers

Le dossier `srv-nginx` contient le `docker-compose.yml` et le dossier `html`.

<img width="1919" height="907" alt="Capture d&#39;écran 2026-07-11 093852" src="https://github.com/user-attachments/assets/811df8de-8491-46e1-b6e2-67528199c58e" />

### Création de la page HTML

```bash
nano ~/srv-nginx/html/index.html
```

<img width="1919" height="910" alt="Capture d&#39;écran 2026-07-11 094136" src="https://github.com/user-attachments/assets/282b532e-2e68-46a3-8b5c-96e757c654ba" />

Vous y mettez ensuite le code HTML que vous souhaitez.

<img width="1919" height="903" alt="Capture d&#39;écran 2026-07-11 094543" src="https://github.com/user-attachments/assets/eddda6e6-0a2b-4e5d-9b12-b51c0f189561" />

<img width="1919" height="908" alt="Capture d&#39;écran 2026-07-11 094242" src="https://github.com/user-attachments/assets/ca5db8d1-01fc-4788-97fc-02011694f8f7" />

Le fichier CSS est également créé pour styliser la page de validation.

```bash
nano ~/srv-nginx/html/style.css
```

<img width="1919" height="903" alt="Capture d&#39;écran 2026-07-11 094543" src="https://github.com/user-attachments/assets/575b6908-0abf-4d60-9fa6-28ab2fb73e72" />

<img width="1919" height="907" alt="Capture d&#39;écran 2026-07-11 094335" src="https://github.com/user-attachments/assets/80127b77-f90c-490d-8dec-3c9a161740de" />

Le dossier `html` contient les deux fichiers servis par Nginx.

<img width="1919" height="907" alt="Capture d&#39;écran 2026-07-11 094335" src="https://github.com/user-attachments/assets/3bb2afdb-0df6-47e4-b0e2-3e523a71ca9f" />

### Récupération de l'adresse IP

```bash
ip a
```

<img width="1919" height="903" alt="Capture d&#39;écran 2026-07-11 094617" src="https://github.com/user-attachments/assets/fffe4fff-899e-4f2f-a620-7c3cd0b435cd" />

L'adresse IP de la VM est `192.168.91.129`, le serveur est accessible sur `http://192.168.91.129:8080`.

<img width="1919" height="913" alt="Capture d&#39;écran 2026-07-11 094630" src="https://github.com/user-attachments/assets/fd2bff87-86ac-46aa-a9ad-934e4700699e" />

### Résultat final — Page servie par Nginx

La page HTML s'affiche dans le navigateur à l'adresse `http://192.168.91.129:8080`, confirmant que le serveur Nginx fonctionne correctement dans son conteneur Docker.

<img width="182" height="37" alt="Capture d&#39;écran 2026-07-11 094704" src="https://github.com/user-attachments/assets/d8563198-d707-447b-aa1f-1666205f6638" />

<img width="1919" height="988" alt="Capture d&#39;écran 2026-07-11 094717" src="https://github.com/user-attachments/assets/1416fc9d-9d07-49d6-afaf-46da2dda98c2" />

---

## Conclusion

Ce projet a permis de découvrir les bases de la **conteneurisation** avec Docker et d'orchestrer un service web avec **Docker Compose**. La mise en place du volume monté, du mapping de port et de la politique de redémarrage automatique constitue une introduction concrète aux bonnes pratiques de déploiement d'applications en environnement isolé.

---

*Réalisé par [Yacine Harrache](https://github.com/yacinehrc) — BTS SIO SLAM | EPSI Lille*
