# Composer template for Drupal projects

# Projets et dossiers disponibles

| Dossier | Version | Technologies | Langages | Pré-requis | Identifiants | Init | Build | Backup |
|---|---|---|---|---|---|---|---|---|
| /datas | - | SQLite | SQL | SQLiteStudio | - | - | - | - |
| /docker | - | docker | Docker | - | - | - | - | - |
| /drupal_8| 8.9.7 | Php/Symfony | Php 7.x | >= Php 7.4 (avec OPCache) | drupal - Drupal2022 | ❌ | ✅ | ❌ |
| /drupal_8_ens | 8.9.7  | Php/Symfony | Php 7.x | >= Php 7.4 (avec OPCache) | drupal - Drupal2022 | ✅ | ❌ | ✅ |
| /drupal_9 | 9.4.9 | Php/Symfony | Php 8.x | >= Php 7.4 (avec OPCache) | drupal - Drupal2022 | ❌ | ✅ | ❌ |
| /drupal_10 | 10.0.x-dev | Php/Symfony | Php 8.x | >= Php 8.1.0 (avec OPCache) | drupal - Drupal2022 | ❌ | ✅ | ❌ |
| /modules | - | Php/Symfony | Php 7.x | - | - | - | - | - |
| drupal_sources_LTS | - | - | - | - | - | - | - | - |

Si besoin, voici les fichiers sources LTS avec les fichier de traductions **FR** ajoutés.

- Drupal 8 : `/drupal_sources_LTS/drupal_8_LTS.zip`
- Drupal 9 : `/drupal_sources_LTS/drupal_9_LTS.zip`
- Drupal 10 : `/drupal_sources_LTS/drupal_10_LTS.zip`

# Démarrage d'un projet

Il est possible de démarrrer une projet avec la commande ci-dessous dans le dossier dauns projet.

```
php -S localhost:8005
```

# Liste de path

- Traductions : `/sites/default/files/translations/`
- Base de données : `/sites/default/files/databases/bdd.sqlite`

# Guides et dépannages

Follow the steps below to update your core files.

1. Run `composer update drupal/core webflo/drupal-core-require-dev symfony/* --with-dependencies` to update Drupal Core and its dependencies.
1. Run `git diff` to determine if any of the scaffolding files have changed. 
   Review the files for any changes and restore any customizations to 
  `.htaccess` or `robots.txt`.
1. Commit everything all together in a single commit, so `web` will remain in
   sync with the `core` when checking out branches or running `git bisect`.
1. In the event that there are non-trivial conflicts in step 2, you may wish 
   to perform these steps on a branch, and use `git merge` to combine the 
   updated core files with your customized files. This facilitates the use 
   of a [three-way merge tool such as kdiff3](http://www.gitshah.com/2010/12/how-to-setup-kdiff-as-diff-tool-for-git.html). This setup is not necessary if your changes are simple; 
   keeping all of your modifications at the beginning or end of the file is a 
   good strategy to keep merges easy.

## Generate composer.json from existing project

With using [the "Composer Generate" drush extension](https://www.drupal.org/project/composer_generate)
you can now generate a basic `composer.json` file from an existing project. Note
that the generated `composer.json` might differ from this project's file.


## FAQ

### Should I commit the contrib modules I download?

Composer recommends **no**. They provide [argumentation against but also 
workrounds if a project decides to do it anyway](https://getcomposer.org/doc/faqs/should-i-commit-the-dependencies-in-my-vendor-directory.md).

### Should I commit the scaffolding files?

The [drupal-scaffold](https://github.com/drupal-composer/drupal-scaffold) plugin can download the scaffold files (like
index.php, update.php, …) to the web/ directory of your project. If you have not customized those files you could choose
to not check them into your version control system (e.g. git). If that is the case for your project it might be
convenient to automatically run the drupal-scaffold plugin after every install or update of your project. You can
achieve that by registering `@composer drupal:scaffold` as post-install and post-update command in your composer.json:

```json
"scripts": {
    "post-install-cmd": [
        "@composer drupal:scaffold",
        "..."
    ],
    "post-update-cmd": [
        "@composer drupal:scaffold",
        "..."
    ]
},
```
### How can I apply patches to downloaded modules?

If you need to apply patches (depending on the project being modified, a pull 
request is often a better solution), you can do so with the 
[composer-patches](https://github.com/cweagans/composer-patches) plugin.

To add a patch to drupal module foobar insert the patches section in the extra 
section of composer.json:
```json
"extra": {
    "patches": {
        "drupal/foobar": {
            "Patch description": "URL or local path to patch"
        }
    }
}
```
### How do I switch from packagist.drupal-composer.org to packages.drupal.org?

Follow the instructions in the [documentation on drupal.org](https://www.drupal.org/docs/develop/using-composer/using-packagesdrupalorg).

### How do I specify a PHP version ?

Currently Drupal 8 supports PHP 5.5.9 as minimum version (see [Drupal 8 PHP requirements](https://www.drupal.org/docs/8/system-requirements/drupal-8-php-requirements)), however it's possible that a `composer update` will upgrade some package that will then require PHP 7+.

To prevent this you can add this code to specify the PHP version you want to use in the `config` section of `composer.json`:
```json
"config": {
    "sort-packages": true,
    "platform": {"php": "5.5.9"}
},
```
