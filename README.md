# New Relic Deploy Logs #

This example will show you how you can automatically log changes to your site into [New Relic's Deployments Page](https://docs.newrelic.com/docs/apm/applications-menu/events/deployments-page) when the workflow fires on Pantheon. This can be quite useful for keeping track of all your performance improvements!

This script uses a couple clever tricks to get data about the platform. First of all it uses the `pantheon_curl()` command to fetch the extended metadata information for the site/environment, which includes the New Relic API key. It also uses data within the git repository on the platform to pull out deploy tag numbers and log messages.

> **Note:** This example will work for all Pantheon sites once the bundled [New Relic APM Pro feature](https://pantheon.io/features/new-relic) is activated, regardless of service level.

## Requirements

While these scripts can be downloaded individually, they are meant to work with Composer. See the installation in the next section.

- Quicksilver script projects and the script name itself should be consistent in naming convention.
- README should include a recommendation for types of hooks and stages that the script should run on.
  - For example, "This script should run on `clone_database` and the `after` stage.
  - Provide a snippet that can be pasted into the `pantheon.yml` file.

### Installation

You should [Activate New Relic Pro](https://pantheon.io/docs/new-relic/#activate-new-relic-pro) within your site dashboard.

This project is designed to be included from a site's `composer.json` file, and placed in its appropriate installation directory by [Composer Installers](https://github.com/composer/installers).

In order for this to work, you should have the following in your composer.json file:

```json
{
  "require": {
    "composer/installers": "^1"
  },
  "extra": {
    "installer-paths": {
      "web/private/scripts/quicksilver": ["type:quicksilver-script"]
    }
  }
}
```

The project can be included by using the command:

`composer require pantheon-quicksilver/new-relic-deploy:^1`

If you are using one of the example PR workflow projects ([Drupal 8](https://www.github.com/pantheon-systems/example-drops-8-composer), [Drupal 9](https://www.github.com/pantheon-systems/drupal-project), [WordPress](https://www.github.com/pantheon-systems/example-wordpress-composer)) as a starting point for your site, these entries should already be present in your `composer.json`.

### Example `pantheon.yml`

Here's an example of what your `pantheon.yml` would look like if this were the only Quicksilver operation you wanted to use.

```yaml
api_version: 1

workflows:
  deploy:
    after:
      - type: webphp
        description: Log to New Relic
        script: private/scripts/new_relic_deploy.php
  sync_code:
    after:
      - type: webphp
        description: Log to New Relic
        script: private/scripts/new_relic_deploy.php
```
