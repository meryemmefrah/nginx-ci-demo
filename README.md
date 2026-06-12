# Nginx CI/CD Demo

Projet de démonstration d'une pipeline CI/CD avec GitHub Actions.
L'application est un simple serveur Nginx conteneurisé qui sert une page HTML.

## Structure du projet
.

├── .github/workflows/ci.yml   # Pipeline GitHub Actions

├── Dockerfile                 # Image Docker basée sur nginx:alpine

├── nginx.conf                 # Configuration Nginx

├── index.html                 # Page servie

└── README.md

## Schéma de la pipeline
Push / Run manuel

│

▼

┌──────────────────┐

│  Récupérer code  │  (actions/checkout)

└────────┬─────────┘

▼

┌──────────────────┐

│ Vérifier fichiers│  (Dockerfile, nginx.conf, index.html présents)

└────────┬─────────┘

▼

┌──────────────────┐

│  Build image     │  (docker build)

└────────┬─────────┘

▼

┌──────────────────┐

│  Test config     │  (nginx -t : config valide ?)

└────────┬─────────┘

▼

✅ / ❌

## Explication

La pipeline se déclenche manuellement (`workflow_dispatch`). Elle exécute un job
`build-test` sur une machine Ubuntu, en 4 étapes :

1. **Récupérer le code** : clone le dépôt sur le runner.
2. **Vérifier la présence des fichiers** : échoue si un fichier essentiel manque.
3. **Build de l'image Docker** : construit l'image à partir du Dockerfile.
4. **Tester la configuration Nginx** : `nginx -t` valide la syntaxe de `nginx.conf`.

Si une étape échoue, la pipeline s'arrête et passe au rouge. Cela a été vérifié
en cassant volontairement `nginx.conf` (capture d'écran ci-jointe).