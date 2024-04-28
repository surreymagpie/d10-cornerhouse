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

then execute the following commands:

```bash
podman compose up -d
source .envrc
composer install
drush site:install --existing-config -y
```

then the site will be available at [http://localhost:8000](http://localhost:8000).

Modules and themes should be added with `composer require`, installed using `drush` or the admin UI, then the updated configuration exported.
