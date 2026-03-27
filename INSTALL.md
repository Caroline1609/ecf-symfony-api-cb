# Procédure d'installation de Symfony

## Prérequis

- [000-default.conf](000-default.conf)
- [Dockerfile](Dockerfile)
- [docker-compose.yml](docker-compose.yml)



## Installation

### Création du Conteneur Docker

*Ouvrez le terminal intégré de VS Code et exécutez :*



```bash
# Lancer les conteneurs en arrière-plan
docker compose up -d
```

```bash
# Accéder au conteneur Symfony (remplacer [names] par le nom de votre conteneur)
docker exec -it [names] bash
```

### Symfony

```bash
# Installe le squelette minimal de Symfony 8 dans le dossier courant (.)
composer create-project symfony/skeleton:"8.0.*" .
```

```bash
# Installe API Platform
composer require api
```

```bash
# Installe le générateur de code (Maker Bundle) uniquement pour le développement
composer require symfony/maker-bundle --dev
```

## Modifications de configuration

* **Fichier :** `/config/routes/api_platform.yaml`
    * [ ] Commenter la ligne `prefix`.
* **Fichier :** `/config/packages/api_platform.yaml`
    ```yaml
    formats:
        json: ['application/json']
    ```
* **Fichier :** `.env`
    * Commenter la ligne `DATABASE_URL` existante.
    * Ajouter la nouvelle configuration (à adapter selon votre environnement) :

    ```env
    DATABASE_URL="mysql://user:user@db:3306/db_myapi?serverVersion=11.8.5-MariaDB&charset=utf8mb4"
    ```


## Controle de la base de donnée

Connectez-vous à votre base de données avec vos identifiants que ce trouve dans le docker compose.

```bash
# Creation de table 
php bin/console make:entity
```
Remplissez les champs demandés pour créer votre entité.



```bash
#Preparation a la migration
php bin/console make:migration
```

```bash
#Migration
php bin/console make:migration
php bin/console doctrine:migrations:migrate
```

## Test de l'API

Dans la méthode Post, il faut ajouter dans le body de la requete les champs suivants : 

```json
{
  "name": "Pineapple",
  "variety": "Victoria",
  "primaryColor": "yellowgreen",
  "lifeTime": 20,
  "price": 3.99
}
```

Et cliquer sur "Execute" pour envoyer la requete.