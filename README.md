# Kontrolgruppen

## Creating a new “Kontrolgruppen” project

```
composer create-project --repository='{"type":"vcs","url":"https://github.com/aakb/kontrolgruppen"}' 'kontrolgruppen/main:dev-feature/skeleton' kontrolgruppen-skeleton
cd kontrolgruppen-skeleton
docker-compose pull
docker-compose up --detach
docker-compose exec phpfpm bin/console doctrine:migrations:diff
docker-compose exec phpfpm bin/console doctrine:migrations:migrate --no-interaction
docker-compose exec -T mariadb mysql --user=db --password=db db <<< "INSERT INTO fos_user(username, username_canonical, email, email_canonical, password, enabled, roles) VALUES ('super-admin', 'super-admin', 'super-admin@example.com', 'super-admin@example.com', 'password', 1, '[]')"
yarn install
yarn build
echo "http://0.0.0.0:$(docker-compose port nginx 80 | cut -d: -f2)"
```

## Install

### Check out submodules
````sh
git submodule update --init --recursive
````

### Build
```sh
composer install --no-dev -o
bin/console doctrine:migrations:migrate
```

### Install CKEditor

```sh
php bin/console ckeditor:install
php bin/console assets:install public
```

See `https://ckeditor.com/latest/samples/toolbarconfigurator/index.html#advanced` for editor tools.

## Development

See [README-develop.md](README-develop.md) for information on setting up for development.

## Todo
Maybe we should use the composer-merge plugin from wikimedia, where we will have a composer-dev.json which overrides the 
main composer.json. In this dev composer json the repo to the core-bundle package is defined as a path, but in the main
composer file the repo is defined as a vcs.

The composer merge plugin should be added under the require-dev section in the main composer file, so it will only 
run when --no-dev is not present when running composer install. 

This will replace the initializing and updating of submodules when deploying to production, because the required kontrolgruppen 
package will be fetched from vcs instead.

## Release procedure.

Build production assets. They will be added to public/prod.

```sh
yarn build
```

Commit the built files to git.

Tag the release.
