# [MPAT](http://www.mpat.eu/)

Main MPAT project based on [Bedrock](https://roots.io/bedrock/).

## Requirements

* PHP >= 5.5
* Composer - [Install](https://getcomposer.org/doc/00-intro.md#globally) (composer executable is also provided by MPAT itself)
* WP-Cli (optional but strongly recommended when configuring MPAT in dev environment) [Install](http://wp-cli.org/#install)

## Demo

A current demo is running at http://demo.mpat.eu

## Installation

1. Clone the git repo - `git clone https://github.com/MPAT-eu/MPAT-core.git [desired_folder_name]`. `[desired_folder_name]` is optional, default value is `MPAT-core`. In next steps `desired_folder_name` refers to MPAT root folder.
2. Verify that `desired_folder_name\web` is accessible through webserver. MPAT can be configured both in apache document root (e.g.: http://localhost/desired_folder_name) and via [apache virtualhost](https://httpd.apache.org/docs/current/vhosts/examples.html), using desired_folder_name as document root of a webserver virtualhost (e.g.: mpat.dev). The choice implies few changes in env variables values. Samples are provided in "Domain related variables samples" section
3. Move in `desired_folder_name`
4. Run `php composer.phar install` to download dependencies
    * if you want to clone all repositories as git repositories, add `--prefer-source`
    * (e.g. contrib and MPAT plugins, on ubuntu 16.04.1 apt install the packages composer, php7.0-xml, zip before running composer install)
5. To get the latest Version:
    * Run `composer update`.
    * If you don't want to deal with npm, you have to make sure that the current master includes all build files, otherwise you have to build the files by your owne by running `npm run build`
    * You can skip Step 5., but then you get not the newest Version of MPAT
6. Copy `.env.example` to `.env` and update environment variables:
  * `DB_NAME` - Database name
  * `DB_USER` - Database user
  * `DB_PASSWORD` - Database password
  * `DB_HOST` - Database host
  * `WP_ENV` - Set to environment (`development`, `staging`, `production`)
  * `WP_HOME` - Full URL to the folder containing main index.php, namely `desired_folder_name\web`
  * `WP_SITEURL` - Full URL to WordPress directory, tipically can be left to ${WP_HOME}/wp
  * `WP_DOMAIN` - Domain part of the WP_HOME URL
  * `PATH_CURRENT_SITE` - Path part of the WP_HOME url with leading and trailing slashes
  * `WP_MULTISITE` - Define if wp instance has multisite capability. If database is not already properly configured, setting this variable at TRUE might lead to database errors (see at Multisite configuration section)
  * `AUTH_KEY`, `SECURE_AUTH_KEY`, `LOGGED_IN_KEY`, `NONCE_KEY`, `AUTH_SALT`, `SECURE_AUTH_SALT`, `LOGGED_IN_SALT`, `NONCE_SALT` - Generate with [wp-cli-dotenv-command](https://github.com/aaemnnosttv/wp-cli-dotenv-command) or from the [WordPress Salt Generator](https://api.wordpress.org/secret-key/1.1/salt/)
7. If webserver has not write permissions on `/desired_folder_name/web/.htaccess` but you still need pretty permalinks, copy `.htaccess.singlesite` to `.htaccess` in `/desired_folder_name/web/` folder (or follow suggestions in `wp-admin/options-permalink.php` page   
8. If needed, update your /etc/hosts file according to WP_HOME
9. In browser, access WP admin at `WP_HOME/wp/wp-admin` (http://example.com/wp/wp-admin)
10. if wordpress is running and you are logged in, activate the "MPAT Plugin" under Plugins.

## Domain related variables samples

#### if MPAT is reachable at http://localhost
     WP_HOME=http://localhost
     WP_SITEURL=${WP_HOME}/wp
     WP_DOMAIN=localhost
     PATH_CURRENT_SITE=/

#### if MPAT is reachable at http://localhost/mpat/web
     WP_HOME=http://localhost/mpat/web
     WP_SITEURL=${WP_HOME}/wp
     WP_DOMAIN=localhost
     PATH_CURRENT_SITE=/mpat/web/

#### if MPAT is reachable at http://mpat.dev
     WP_HOME=http://mpat.dev
     WP_SITEURL=${WP_HOME}/wp
     WP_DOMAIN=mpat.dev
     PATH_CURRENT_SITE=/

## Multisite configuration

MPAT relies on wordpress multisite feature to create multiple HbbTV applications.
After installation, you have to enable that feature and there are two main ways: manually in the backend UI, or using wp-cli.

1. Follow [WP documentation](http://codex.wordpress.org/Create_A_Network#Step_3:_Installing_a_Network).
2. Due to changes in the project folder structure (wp-config.php settings have been moved in application.php), when wordpress guided installation prompts to put code in your wp-config.php do not do that, instead set WP_MULTISITE variable in your .env file to TRUE (other required constants are already defined in application.php)
3. Copy lines suggested by network installation tool to  in `/desired_folder_name/web/.htaccess`. If `.htaccess` does not exist, it can be created staring from .htaccess.multisite

## Documentation

Bedrock documentation is available at [https://roots.io/bedrock/docs/](https://roots.io/bedrock/docs/).
