# MPATistrano configuration

This document describe concisely the required step to configure a remote environment to benefit of the capistrano magic functionalities to deploy MPAT fast and effectively.

Any kind of customization coming from bedrock developers or MPAT developers is built on top of [Capistrano](http://capistranorb.com/), so please have a look at its documentation whenever needed.

## Prerequisites
- Ruby >= 1.9 (local machine)
- bundler (`gem install bundler` after installed ruby)
- GIT (remote machine)
- SVN (remote machine), required by composer to download contributed plugins and wordpress core from wordpress official repository
- composer (installed globally on the remote machine)
- define ssh keys in remote environment and register them in gitlab (user by composer on the remote to checkout projects) **OR** configure ssh keys locally (already done if developer…) and use ssh forward agent feature, enabled by default in the sample rb script

## Configuration
while configuring the whole thing, it is wise to set capistrano log level to debug. in `<stage>.rb` add the line
```
set :log_level, :debug
```
This would overwrite the default value set in deploy.rb
```
set :log_level, :info
```
1. create a new deploy config file like follow `<document_root>/config/deploy/<stage>.rb`
2. Before your first deploy, run `bundle exec cap <stage> deploy:check` to create the necessary folders/symlinks.
3. in remote env, manually create `<document_root>/shared/.env` file starting from `<project_root>/.env.example` file (attached below)
4. in remote env, manually copy a valid `.htaccess` (multisite version) in `<document_root>/shared/web/.htaccess`
5. back to local env, run `bundle exec cap <stage> deploy`
6. solve all the errors (you might skip this step if no error... very unlikely)
7. back to point 5 and 6 until deploy process is completed with a message like 
``` 
DEBUG[012ebf74] Command: echo "Branch your-beautiful-branch (at 11e28a1) deployed as release 20170313161206 by you" >> /var/www/html/revisions.log
INFO[012ebf74] Finished in 0.074 seconds with exit status 0 (successful).
```
8. in remove env, change webserver document root to `<document_root>/current`
9. Deploy like there's no tomorrow.

## Usage

* Deploy: `cap production deploy`
* Rollback: `cap production deploy:rollback`

Composer support is built-in so when you run a deploy, `composer install` is automatically run. Capistrano has a great [deploy flow](http://www.capistranorb.com/documentation/getting-started/flow/) that you can hook into and extend it.

### Deploy specific tag
coming soon...
