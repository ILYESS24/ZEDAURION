# 🚀 Déploiement Zed Collab sur Fly.io

Guide complet pour déployer Zed Collab sur **Fly.io** - excellent pour les applications Rust avec WebSockets.

## 🎯 Pourquoi Fly.io ?

**Fly.io est PARFAIT pour Zed Collab** car :

- ✅ **Rust natif** : Supporte parfaitement les binaires Rust
- ✅ **WebSockets** : Excellente gestion des connexions temps réel
- ✅ **PostgreSQL** : Supporte les bases de données externes
- ✅ **Multi-régions** : Déploiement mondial
- ✅ **CLI moderne** : Interface en ligne de commande puissante
- ✅ **Gratuit** : Parfait pour commencer (généreux free tier)

## 📋 Prérequis

- [fly.io](https://fly.io) : Créez un compte gratuit
- [Fly CLI](https://fly.io/docs/getting-started/installing-flyctl/) : Installez l'outil en ligne de commande
- Votre repo GitHub `ILYESS24/ZEDAURION`

## 🚀 Déploiement Rapide (10 minutes)

### Étape 1 : Installation et authentification

```bash
# Installez Fly CLI
curl -L https://fly.io/install.sh | sh

# Authentifiez-vous
fly auth login

# Vérifiez que ça marche
fly status
```

### Étape 2 : Préparer votre application

```bash
# Allez dans votre repo
cd path/to/zed-main

# Initialisez Fly.io dans votre projet
fly launch --no-deploy

# Fly va créer fly.toml automatiquement
```

### Étape 3 : Configurer PostgreSQL

```bash
# Créez une base de données PostgreSQL
fly postgres create

# Attachez-la à votre app
fly postgres attach <postgres-app-name>

# Fly injecte automatiquement DATABASE_URL
```

### Étape 4 : Variables d'environnement

```bash
# Définissez les variables d'environnement
fly secrets set API_TOKEN="$(openssl rand -hex 32)"
fly secrets set HTTP_PORT="10000"
fly secrets set DATABASE_MAX_CONNECTIONS="20"
fly secrets set INVITE_LINK_PREFIX="https://zed.dev/invite/"
fly secrets set ZED_ENVIRONMENT="production"
fly secrets set RUST_LOG="info"
fly secrets set LOG_JSON="true"
```

### Étape 5 : Configuration fly.toml

Fly aura créé un `fly.toml`. Vérifiez qu'il ressemble à ça :

```toml
app = "zed-collab"
primary_region = "ord"  # ou votre région préférée

[build]
  dockerfile = "./Dockerfile"
  context = "."

[env]
  HTTP_PORT = "10000"
  DATABASE_MAX_CONNECTIONS = "20"
  INVITE_LINK_PREFIX = "https://zed.dev/invite/"
  ZED_ENVIRONMENT = "production"
  RUST_LOG = "info"
  LOG_JSON = "true"

[http_service]
  internal_port = 10000
  force_https = true

[[vm]]
  size = "shared-cpu-1x"  # ou "performance-1x" pour plus de puissance
```

### Étape 6 : Premier déploiement

```bash
# Déployez !
fly deploy

# Suivez les logs
fly logs

# Ouvrez dans le navigateur
fly open
```

## 🔧 Configuration Avancée

### Health Check

Ajoutez dans `fly.toml` :
```toml
[http_service]
  internal_port = 10000
  force_https = true

  [http_service.checks]
    [http_service.checks.health]
      port = 10000
      type = "http"
      interval = "10s"
      timeout = "5s"
      method = "GET"
      path = "/healthz"
```

### Scaling

```bash
# Augmentez les ressources
fly scale vm shared-cpu-2x

# Ou passez à performance
fly scale vm performance-1x
```

### Multi-régions

```toml
[[vm]]
  size = "shared-cpu-1x"
  regions = ["ord", "lax", "iad"]  # Chicago, Los Angeles, Washington DC
```

## 📊 Ressources et Tarifs

### Free Tier (Généreux)
- 3 shared-cpu-1x VMs
- 160GB de stockage
- Bande passante illimitée
- PostgreSQL gratuit (limité)

### Tarifs
- **Shared CPU** : 1.25$ par GB RAM/mois
- **Dedicated CPU** : 5$ par vCPU/mois
- **PostgreSQL** : 0.000034$ par GB/heure

### Comparaison avec Railway

| Aspect | Fly.io | Railway |
|--------|--------|---------|
| **Rust Support** | ✅ Excellent | ✅ Bon |
| **WebSockets** | ✅ Excellent | ✅ Bon |
| **Free Tier** | ✅ Généreux | ❌ Limité |
| **CLI** | ✅ Puissant | ⚠️ Basique |
| **Multi-régions** | ✅ Facile | ❌ Complexe |
| **Tarifs** | 🟢 Moins cher | 🟡 Plus cher |

## 🚨 Résolution de Problèmes

### Build échoue
```bash
# Vérifiez les logs de build
fly logs --app <app-name>

# Redéployez avec cache vidé
fly deploy --no-cache
```

### Connexion DB échoue
```bash
# Vérifiez DATABASE_URL
fly secrets list

# Testez la connexion DB
fly postgres connect
```

### Port incorrect
```bash
# Vérifiez le port interne
fly status
```

## ✅ Vérification du Déploiement

```bash
# Status de l'app
fly status

# Health check
curl https://<your-app>.fly.dev/healthz

# Logs temps réel
fly logs -f
```

## 📁 Fichiers créés

- `fly.toml` : Configuration Fly.io
- Base de données PostgreSQL automatiquement configurée

## 🔗 Liens Utiles

- [Fly.io Dashboard](https://fly.io/dashboard)
- [Documentation Fly.io](https://fly.io/docs)
- [Fly.io Rust Guide](https://fly.io/docs/rust/)

## 🎉 Avantages de Fly.io pour Zed

1. **Rust First** : Construit par des développeurs Rust
2. **Edge Computing** : Proche de vos utilisateurs
3. **WebSockets natifs** : Parfait pour collaboration temps réel
4. **PostgreSQL intégré** : Configuration simple
5. **CLI moderne** : DX exceptionnelle

**Fly.io est idéal pour Zed Collab !** 🚀
