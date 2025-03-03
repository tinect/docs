# Versioning & Dependencies

## Versioning & Dependencies

### Shopware 6 production template

The Shopware 6 production template enables you to build, package and deploy Shopware 6 to production shops. This template is also used to build the official packages distributed by [shopware](https://www.shopware.com/en/download).

<!-- markdown-link-check-disable -->
{% embed url="https://github.com/shopware/production" %}
<!-- markdown-link-check-enable -->

The template is optimized for production usage and contains basic development tooling. It's intended as a basis for project customizations, which are usually done by agencies.

{% hint style="info" %} If you want to contribute to the [Shopware Platform](https://github.com/shopware/platform) or develop store plugins, you should use the [development template](https://github.com/shopware/development).
{% endhint %}

### Branches and stability

In each commit a `composer.lock` is contained to ensure that the version being deployed is the version that was tested in our CI. We currently provide the following branches:

* `6.3`: stable minor and patch releases \(`v6.3.0.0-rc2`, `v6.3.0.1`, `v6.3.1.0`, `v6.1.*`, but not `v6.4.0.0`\)
* `trunk`: stable major, minor and patch releases \(`v6.3.0.0`, `v6.3.1.0`, `v6.4.0.0`, `v6.5.0.0`...\)

The `6.3` branch contains all the 6.3 releases. It's stable now and only gets non-breaking changes. \(security issues are an exception\).

The `trunk` branch contains the newest stable release, including major releases. That may result in plugins being incompatible, so be careful.

Starting with `6.3.0.0`, we use a slightly modified version of SemVer. The pattern looks like this: 6.MAJOR.MINOR.PATCH. Examples:

* 6.3.2.5 - Major=3, Minor=2, Patch=5
* 6.4.1.0 - Major=4, Minor=1, Patch=0

See also:

<!-- markdown-link-check-disable -->
{% embed url="https://www.shopware.com/en/news/shopware-6-versioning-strategy" %}
<!-- markdown-link-check-enable -->

### Requirements

To get an overview of the requirements see the following guide of your documentation first:

{% page-ref page="./../../../guides/installation/overview.md" %}

NPM and Node are only required during the build process and for development. If you don't have javascript customizations, it's not required at all. Because the storefront and admin are pre-build.

If you are using a separate build server, consider having NPM and Node as build-only requirements. Your operating application server doesn't require any of these to run Shopware 6.

### Setup and install

To set up the environment and install with a basic setup run the following commands:

```bash
# clone newest 6.3 patch version from github 
git clone --branch=6.3 https://github.com/shopware/production shopware
cd shopware

# install shopware and dependencies according to the composer.lock 
composer install

# setup the environment
bin/console system:setup
# or create .env yourself, if you need more control
# create jwt secret: bin/console system:generate-jwt-secret
# create app secret: APP_SECRET=$(bin/console system:generate-app-secret)
# create .env

# create database with a basic setup (admin user and storefront sales channel)
bin/console system:install --create-database --basic-setup

# or use the interactive installer in the browser: /recovery/install/index.php
```

### Update

To update Shopware 6 just run this:

```bash
# pull newest changes from origin
git pull origin

# the (pre|post)-(install|update)-cmd will execute all steps automatically
composer install
```

## Customization

This project is called production template because it can be used to create project specific configurations. The template provides a basic setup that is equivalent to the official distribution. If you need customization, the workflow could look like this:

* Fork template
* Make customization
* Add dependencies
* Add project specific plugins
* Update var/plugins.json \(bin/console bundle:dump, paths need to be relative to the project root\)
* Build administration/storefront
* Update composer.json and composer.lock
* Commit changes

### Development

#### Command overview

The following commands and scripts are available

**Setup/Install/Deployment**

| Command | Description |
| :--- | :--- |
| `bin/console system:setup` | Configure and create .env and optionally create jwt secret |
| `bin/console system:generate-jwt-secret` | Generates a new jwt secret |
| `bin/console system:generate-app-secret` | Outputs a new app secret. This does not update your .env! |
| `bin/console system:install` | Setup database and optional install some basic data |
| `bin/console system:update:prepare` | Run update preparations before the update. Do not update if this fails |
| `bin/console system:update:finish` | Executes the migrations and finishes the update |
| `bin/console theme:change` | Assign theme to a sales channel |

**Build**

{% hint style="info" %} Bash is required for the shell scripts. {% endhint %}

| Command | Description |
| :--- | :--- |
| `bin/console theme:compile` | Compile all assigned themes |
| `bin/build.sh` | Complete build including composer install |
| `bin/build-js.sh` | Build administration and storefront, including all plugins in `var/plugins.json`. |
| `bin/build-administration.sh` | Just build the administration. |
| `bin/build-storefront.sh` | Just build the storefront. You need to have built the administration once. |

**Dev**

Run `bin/build-js.sh` once to install the npm dependencies.

{% hint style="info" %} Bash is required for the shell scripts. {% endhint %}

| Command | Description |
| :--- | :--- |
| `bin/console theme:refresh` | Reload theme.json of active themes |
| `bin/watch-administration.sh` | Watcher for administration changes, recompile and reload page if required |
| `bin/watch-storefront.sh` | Watcher for storefront changes, recompile and reload page if required |

### Configuration

See also the `README.md` in the `config` folder of the production template.

#### Template overview

This directory tree should give an overview of the template structure.

```text
├── bin/                  # binaries to setup, build and run symfony console commands 
├── composer.json         # defines dependencies and setups autoloading
├── composer.lock         # pins all dependencies to allow for reproducible installs
├── config                # contains application configuration
│   ├── bundles.php       # defines static symfony bundles - use plugins for dynamic bundles
│   ├── etc/              # contains the configuration of the docker image
│   ├── jwt/              # secrets for generating jwt tokens - DO NOT COMMIT these secrets
│   ├── packages/         # configure packages - see: config/README.md
│   ├── secrets/          # symfony secrets store - DO NOT COMMIT these secrets
│   ├── services/         # contains some default overrides
│   ├── services.xml      # just imports the default overrides - this file should not change
│   └── services_test.xml # just imports the default overrides for tests
├── custom                # contains custom files
│   ├── plugins           # store plugins
│   ├── static-plugins    # static project specific plugins
├── docker-compose.yml    # example docker-compose
├── Dockerfile            # minimal docker image
├── phpunit.xml.dist      # phpunit config
├── public                # should be the web root
│   ├── index.php         # main entrypoint for the web application
├── README.md             # this file
├── src
│   ├── Command/*
│   ├── Kernel.php        # our kernel extension
│   └── TestBootstrap.php # required to run unit tests
└── var
    ├── log/              # log dir
    |── cache/            # cache directory for symfony
    └── plugins.json      # javascript build configuration
```

### Managing Dependencies

#### Composer

You only need to require the things you want. If you only want to run shopware 6 in headless mode, your composer.json could look like this:

```javascript
{
    "name": "acme/shopware-production",
    "type": "project",
    "license": "MIT",
    "config": {
        "optimize-autoloader": true
    },
    "prefer-stable": true,
    "minimum-stability": "stable",
    "autoload": {
        "psr-4": {
            "Shopware\\Production\\": "src/"
        }
    },
    "require": {
        "php": "~7.2",
        "composer/package-versions-deprecated": "^1.8.0",
        "shopware/core": "~v6.3.0"
    }
}
```

#### Require project plugins

If you have project specific plugins, place them under `custom/static-plugins/{YourPlugin}` and require them in your `composer.json`.

Note: The plugins needs a \(stable\) version to work with the default stability `stable`.

```bash
composer require "exampleorg/myplugin"
```

External plugins in private repositories can also be required by adding the repository to your `composer.json`.
To learn more about the usage of private repositories, see here:

<!-- markdown-link-check-disable -->
{% embed url="https://getcomposer.org/doc/05-repositories.md#using-private-repositories" %}
<!-- markdown-link-check-enable -->

#### Update shopware packages

Run the following command, to update all shopware dependencies:

```bash
composer update "shopware/*"
```

## Deployment

### Docker

The `DOCKERFILE` and docker-compose.yml service definitions should work but are still experimental.

### Storage and caches

The following directories should be shared by all app servers:

```text
.
├── config
│   ├── jwt # ro - should be written on first deployment
│   ├── secrets # rw shared - see, if you want to use it: https://symfony.com/blog/new-in-symfony-4-4-encrypted-secrets-management 
├── public
│   ├── bundles # rw shared - Written by `assets:install` / `theme:compile`, can also be initiated by the administration
│   ├── media # rw shared
│   ├── theme # rw shared - generated themes by `theme:compile/change`
│   └── thumbnail # rw shared - media thumbnails
│   └── sitemap # rw shared - generated sitemaps
├── var
│   ├── cache # rw local - contains the containers, which contains additional cache directories (twig, translations, etc)
│   ├── log # a - append only, can be change in the monlog config

ro - Readonly after deployment
rw shared - read and write access, it should be shared across the app servers
rw local - locale read and write access
```

Some of these directories like `public` can also be changed to different flysystem to host the files on s3 for example.

