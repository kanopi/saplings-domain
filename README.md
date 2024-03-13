# Saplings Domain

## Overview

This recipe installs base Domain Access module and configuration.

## Features

- Commonly used Domain modules
- An example default domain - adjust this config for your project
- An example microsite domain (example_microsite) and its domain aliases
- An example top-level-domain (example_toplevel) and its domain aliases

## Getting Started

To start using Saplings Theme, follow these steps:

1. In your project: `fin composer require kanopi/saplings-domain`
2. [Configuring Drupal to Apply Recipes](https://snippets.cacher.io/snippet/263fab3a292182ff3e34)
3. [Apply the recipe](https://snippets.cacher.io/snippet/263fab3a292182ff3e34#F0_MH--applying-a-recipe)
4. Validate the recipe on your project
5. [Unpack the recipe](https://snippets.cacher.io/snippet/263fab3a292182ff3e34#F0_MH--unpacking-a-recipe) (to move its composer requirements into your project's composer.json file)
6. Complete domain configuration - see Configuration section below
7. In your project: `fin composer remove kanopi/saplings-domain`
7. `fin drush cex -y`
8. `git checkout -b [mybranch]`
8. `git add .`
9. `git commit -m "applies saplings-domain recipe`
10. `git push`
11. Create and submit PR
   
## Patches

Once you have applied and unpacked this recipe, add the following patches to your project's main composer.json file and then run `fin composer install`:

        "patches": {
            "drupal/domain": {
                "Use core route provider with addExtraCacheKeyPart for route caching?": "https://www.drupal.org/files/issues/2023-05-09/domain-route-provider-addextracachekeypart-3359253-02.patch",
                "Domain Entity module compatibility issue causes hasDomainPermissions() to return nothing": "https://www.drupal.org/files/issues/2024-03-06/domain-3426236-2.patch"
            },
            "drupal/domain_menus": {
                "Errors on Group menus": "https://www.drupal.org/files/issues/2024-01-31/domain_menus-3418527-1.patch"
            }
        }

## Notes

This recipe does not install the `domain_language` module. 

If your project is enabling Drupal translation, you should require it and enable it manually and add the following patch to your project's composer.json:

        "patches": {
            "drupal/domain_language": {
                "DomainLanguageOverrider service arguments": "https://www.drupal.org/files/issues/2023-09-26/domain_language-3388388-7.patch"
            }
        }

Without this patch, you won't be able to configure different language preferences per domain without getting a fatal error. If all your domains have the same languages and the same default language, you don't need this patch.

## Configuration
1. Go to admin/config/domain
2. Edit the default domain and change the config to reflect the URL for your project
3. Go back to admin/config/domain
4. Select Aliases from the option list for your default domain
5. Edit the aliases to reflect the URL of your docksal and pantheon sites
6. Go to /admin/config/domain/entities/node and enable domain access for each of your project's content types
7. Go to /admin/config/domain/settings - if you want to Enable Login restriction, enable it at the bottom of this form and also enable Assign Domain to User
8. Go to /admin/config/domain/menu/access/settings - if you want separate menus per domain, and you want them to be auto-configured when you add future domains enable them here. You can also just assign menus to certain domains, so this is optional.
9. Go to /admin/config/domain/domain_menus - set options here as needed for your project
10. Later, after all this config is deployed to your canonical site, go to /admin/config/domain/domain_access_logo if you want different logos per domain (because you have to upload files for this, you should do this config on your canonical site and then synch the database down to your local so you can export the config there)
11. Go to /admin/config/domain/entities - we have pre-enabled common entity types like users, nodes and terms, but you may want to enable it for entities for your project, e.g, Sitewide Alerts

## Resources

See [this cacher](https://snippets.cacher.io/snippet/6a7201959cdb2290d3c0) on working with Domains and Domain aliases (for local, multidevs, dev, test and live). It is Pantheon-centric, but the information about configuring domain aliases is relevant despite hosting platform.

