# To run this project
```sh
docker-compose run --rm composer install
docker-compose run --rm composer update
docker-compose run --rm npm install

docker-compose up -d
docker-compose exec app php artisan key:generate
docker-compose exec app php artisan migrate:fresh --seed

docker-compose run --rm npm run watch

// remember to clear cache on browser (Ctrl + F5)
```

# History of commonly used commands (Run on Windows Powershell)

## Download Laravel dependencies 
```shell
docker run --rm -v ${PWD}:/app composer install
```

## Init Laravel project
```shell
docker-compose run --rm composer  create-project laravel/laravel laravel-icommerce
docker-compose up -d
docker-compose exec app php artisan key:generate
docker-compose down
```

## Init model and controllers
```shell
docker-compose exec app php artisan make:controller IndexController
docker-compose exec app php artisan make:controller UserController

docker-compose exec app php artisan make:model Category --migration
docker-compose exec app php artisan make:model Tag --migration
docker-compose exec app php artisan make:model Post --migration

docker-compose exec app php artisan make:migration create_post_tag_table
docker-compose exec app php artisan make:migration AddForeignKeysToPostsTable
docker-compose exec app php artisan make:migration AddLeadToPostsTable

docker-compose exec app php artisan migrate
```

## Install `laravel/jetstream` for UI (laravel/inertia) and Security
```shell
# Inertia has no client-side routing, nor does it require an API. 
# Simply build controllers and page views like you've always done!
# https://inertiajs.com/
# https://jetstream.laravel.com/1.x/stacks/inertia.html#introduction

docker run --rm -v ${PWD}:/app composer require laravel/jetstream

# Install Jetstream with the Inertia stack...
docker-compose exec app php artisan jetstream:install inertia
docker-compose exec app php artisan migrate
docker-compose run --rm npm install

# Components files will be available under the folder 
# ./resources/views/vendor/jetstream/components
docker-compose exec app php artisan vendor:publish --tag=jetstream-views

# Run either in "development" mode (config file: package.json)
# or in "watch" mode the page refresh as soon as you hit save file 
docker-compose run --rm npm run dev
docker-compose run --rm npm run watch

# Update Vue pages under folder
# ./resources/js/Pages/

```

## Install `barryvdh/laravel-ide-helper` for Visual Studio Code
```shell
docker-compose run --rm composer require --dev barryvdh/laravel-ide-helper
```

## Install `nuwave/lighthouse` as GraphQL server
```shell
docker-compose run --rm composer require nuwave/lighthouse
docker-compose exec app php artisan vendor:publish --tag=lighthouse-schema

docker-compose run --rm composer require mll-lab/laravel-graphql-playground

# Console here:
# http://localhost/graphql-playground

# Doc: 
# https://lighthouse-php.com/4.18/getting-started/installation.html
```

## Install `tcg/voyager` as free admin page
```shell
docker run --rm -v ${PWD}:/app composer require tcg/voyager
docker-compose exec app php artisan migrate:reset
docker-compose exec app php artisan voyager:install --with-dummy
```


## Seed data or testing
```shell
# Create a model factory using Faker
docker-compose exec app php artisan make:factory PostFactory

# Seed objects to our database
docker-compose exec app php artisan db:seed

# Migration down everything, and then migration up again
docker-compose exec app php artisan migrate:fresh
docker-compose exec app php artisan migrate:fresh --seed
```

## Install laravel/telescope for debug assistance
```shell
docker-compose run --rm composer require laravel/telescope

docker-compose exec app php artisan telescope:install
docker-compose exec app php artisan migrate

# Console here:
# http://localhost/telescope

# Doc: 
# https://laravel.com/docs/8.x/telescope

```

## Install Vue/Apollo as GraphQl client
```shell
docker-compose run --rm npm install vue-apollo graphql apollo-boost graphql-tag
```

## Utils

```shell
docker-compose exec app php artisan --help

docker-compose exec app php artisan migrate
docker-compose exec app php artisan cache:clear
docker-compose exec app php artisan config:cache

# Update dependencies (configuration file: composer.json) 
docker-compose run --rm composer update

# Open command prompt
docker-compose exec app php artisan tinker
```

