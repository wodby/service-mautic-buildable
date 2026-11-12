# Buildable Mautic on Wodby

This service is the [mautic](https://github.com/wodby/service-mautic) service built from the app's repository: its
guidance applies to settings, data, workers, installation and updates. This page covers what differs.

## Image

The image is built from the app's Git repository on top of [wodby/mautic-php](https://github.com/wodby/mautic-php):
PHP-FPM and Apache with the same Wodby setup as the prebuilt Mautic image, but no Mautic. The service's versions are
PHP versions. The repository is a Mautic Composer project, started from the
[Mautic boilerplate](https://github.com/wodby/mautic-boilerplate), Mautic's recommended project with a
`composer.lock`.

The build:

- fails without `composer.json` and `composer.lock`, or with a committed `config/local.php`, Mautic's own settings
  file, which is kept in the files directory;
- runs `composer install --no-dev` with Node.js available, so the project's Composer scripts install the npm packages
  and build Mautic's assets;
- copies the result onto wodby/mautic-php and runs `mautic-prepare-app`, which adds Wodby's settings file and links the
  directories Mautic writes to the files volume, as in the prebuilt image.

Mautic's version comes from `composer.lock`: new image revisions bring PHP, Apache and Wodby's scripts, not a new
Mautic. Updating Mautic means `composer update "mautic/*" --with-all-dependencies`, committing the lock file and
deploying; the deployment runs the database migrations.

## Plugins and themes

Plugins and themes are installed with Composer into `docroot/plugins/` and `docroot/themes/`, or committed there. After
every deployment the **Install or upgrade Mautic** action runs `mautic-update`, as the mautic service does, which ends
with `mautic:plugins:reload`: plugins new in the repository are registered and installed ones are updated.

## Workers

The `mautic-buildable-cron` and `mautic-buildable-worker` workers run the app's own image, so the repository's plugins
are loaded there too.
