# Cornerhouse Dental Practice

A Drupal 10 rebuild of the version 7 site.

### Local Development
This repo is set up to use podman compose with my own `nginx-php` container and an official `mariadb` container.

To get started clone this repo locally and create a `web/sites/default/settings.local.php` file
```php
<?php

$databases['default']['default'] = array (
  'database' => 'drupal10',
  'username' => 'drupal10',
  'password' => 'drupal10',
  'prefix' => '',
  'host' => 'db',
  'port' => '3306',
  'isolation_level' => 'READ COMMITTED',
  'driver' => 'mysql',
  'namespace' => 'Drupal\\mysql\\Driver\\Database\\mysql',
  'autoload' => 'core/modules/mysql/src/Driver/Database/mysql/',
);

$settings['trusted_host_patterns'] = [
  '^localhost$',
];
```

For migrating data from the legacy site, that repo should reside in a sibling directory to this project, *e.g. under a common `Code` directory*.

Setup with the following commands:

```bash
podman compose up -d

source .envrc
# The above gives access to `drush` and `composer` commands for woring in the D10
# site and a `drush7` command for the D7 one.

composer install
drush site:install --existing-config -y
```

then the D10 site will be available at [http://localhost:8000](http://localhost:8000) while the D7 one is at [http://localhost:8001](http://localhost:8001).

Modules and themes should be added with `composer require`, installed using `drush` or the admin UI, then the updated configuration exported.
