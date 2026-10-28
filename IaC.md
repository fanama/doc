# Infrastructure as Code & Déploiement Automatisé sur Linode Cloud

---

## 📋 Table des matières
1. [Vision d'Ensemble & Architecture](#1-vision-densemble--architecture)
2. [Étape 1 : Infrastructure as Code (Terraform)](#2-étape-1--infrastructure-as-code-terraform)
3. [Étape 2 : Conteneurisation (Docker & Compose)](#3-étape-2--conteneurisation-docker--compose)
4. [Étape 3 : Pipeline CI/CD (GitHub Actions)](#4-étape-3--pipeline-cicd-github-actions)
5. [Résultats Opérationnels & Synthèse](#5-résultats-opérationnels--synthèse)

---

## 1. Vision d'Ensemble & Architecture

L'objectif de cette architecture est de transformer l'administration système manuelle en un processus totalement automatique, reproductible et sécurisé.

### Schéma du Pipeline

```
[ Développeur ]
      │
      ▼ Push sur 'main'
[ GitHub Actions (CI/CD) ]
      │
      ├─► 1. Build & Push Image Docker ──► [ GitHub Container Registry (GHCR) ]
      │
      └─► 2. Provisionnement / Config  ──► [ Terraform ]
                                                 │
                                                 ▼ (Création Cloud)
                                           [ Linode VPS (Ubuntu 24.04) ]
                                                 │ (Firewall 22, 80, 443)
                                                 ▼ (Deploy SSH)
                                           [ Docker Compose ]
```

---

## 2. Étape 1 : Infrastructure as Code (Terraform)

Au lieu de créer un serveur à la main dans l'interface cloud, l'infrastructure est décrite sous forme de code déclaratif avec Terraform.

### Points clés :
* **Instance VPS Linode** : Création automatique d'un serveur Ubuntu 24.04 à Paris (`fr-par`).
* **Linode Cloud Firewall** : Filtrage strict du trafic au niveau du réseau cloud (ports 22, 80, 443 uniquement).
* **Reproductibilité** : Capacité de recréer l'ensemble de l'infrastructure en une commande (`terraform apply`).

### Extrait du code (`main.tf`) :
```hcl
resource "linode_instance" "web_server" {
  label           = "app-production"
  image           = "linode/ubuntu24.04"
  region          = "fr-par"
  type            = "g6-standard-1"
  authorized_keys = [var.ssh_public_key]
}

resource "linode_firewall" "web_firewall" {
  label = "web-server-firewall"
  
  inbound {
    label    = "allow-ssh-web"
    action   = "ACCEPT"
    protocol = "TCP"
    ports    = "22, 80, 443"
    ipv4     = ["0.0.0.0/0"]
  }

  linodes = [linode_instance.web_server.id]
}
```

---

## 3. Étape 2 : Conteneurisation (Docker & Compose)

L'application et ses dépendances sont isolées dans des conteneurs légers pour éliminer tout problème d'environnement ("ça marche sur ma machine").

* **Dockerfile** : Packaging de l'application en image légère Alpine.
* **Docker Compose** : Orchestration des conteneurs (App Web + Base de données PostgreSQL).
* **Persistance** : Utilisation de volumes Docker pour conserver la base de données après redémarrage.

### Extrait `docker-compose.yml` :
```yaml
version: '3.8'

services:
  app:
    image: ${DOCKER_IMAGE}
    container_name: production_app
    restart: always
    ports:
      - "80:3000"
    depends_on:
      - db

  db:
    image: postgres:16-alpine
    container_name: production_db
    restart: always
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

---

## 4. Étape 3 : Pipeline CI/CD (GitHub Actions)

Chaque `git push` sur la branche principale déclenche l'exécution du pipeline d'intégration et de déploiement continu.

1. **Build** : Compilation de l'image Docker avec un tag unique basés sur le hash du commit Git (`github.sha`).
2. **Push** : Envoi de l'image sur le registre privé GitHub (`ghcr.io`).
3. **Déploiement SSH** : Connexion automatique au serveur Linode, récupération de la nouvelle image et relance sans interruption via `docker compose up -d`.

---

## 5. Résultats Opérationnels & Synthèse

* **100% Automatisé** : Aucune intervention manuelle requise sur le serveur.
* **Zéro Dérive d'Infrastructure** : Tout l'état du serveur et du réseau est tracé dans Git.
* **Déploiement Rapide** : Mises à jour applicatives appliquées en quelques secondes.
