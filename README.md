# Roots Example Project

This repository is an example of how to integrate and use the following projects together:

* [Bedrock](https://github.com/roots/bedrock)
* [Trellis](https://github.com/roots/trellis)
* [Sage](https://github.com/roots/sage) (with [Soil](https://github.com/roots/soil))

For more background, see this [blog post](https://roots.io/a-modern-wordpress-example/).

This project is a complete working example that's deployed on a [Digital Ocean](https://roots.io/r/digitalocean/) 512MB droplet.

You can view it at http://roots-example-project.com/.

## Instructions

This project can be cloned and re-configured to fit your needs but we highly suggest you follow in the instructions below to create your own. This will guarantee you have the latest version of Bedrock, Trellis, Sage, and Soil in case this example falls behind a little.

Here's how this example project was created:

1. Create a new project directory: `$ mkdir example.com && cd example.com`
2. Clone Trellis: `$ git clone --depth=1 git@github.com:roots/trellis.git && rm -rf trellis/.git`
3. Clone Bedrock: `$ git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git`
4. Clone Sage: `$ git clone --depth=1 git@github.com:roots/sage.git site/web/app/themes/sage && rm -rf site/web/app/themes/sage/.git`

After that your folder structure is complete and you're ready to configure the individual components.

### Bedrock

Bedrock doesn't need any additional configuration by default. There's only one custom option set in this example: `WP_DEFAULT_THEME`.

* In `site/config/application.php` add `define('WP_DEFAULT_THEME', 'sage');`

### Trellis

Trellis' [instructions](https://github.com/roots/trellis) apply here, but more specifically:

1. Make sure you have the [requirements](https://github.com/roots/trellis#requirements) all installed
2. Install the Ansible Galaxy roles: `$ cd trellis && ansible-galaxy install -r requirements.yml`
3. Configure your `wordpress_sites` - see the [Local Development Setup docs](https://roots.io/trellis/docs/local-development-setup/)

#### Staging/Production

If you also want staging/production servers, create those manually at this point.

1. Add their hostnames/IPs to `ansible/hosts/<environment>`
2. Configure their `wordpress_sites` just like #3 above and follow the [Remote Server Setup docs](https://roots.io/trellis/docs/remote-server-setup/)
3. Define your SSH `keys` to give users the ability to deploy, see the [SSH Keys docs](https://roots.io/trellis/docs/ssh-keys/)

### Provision

#### Development

1. Run `vagrant up`

#### Staging/Production

1. Provision server: `ansible-playbook server.yml -e env=<environment>`
2. Deploy your site: `./deploy.sh <environment> <site name>`

# Naming

Some notes on names used throughout this project:

* You're encouraged to rename Sage to your theme name. Just remember to rename references to it everywhere.
* Any time you see a name like "example.com", "example.dev", "roots-example-project.com", etc, it should be renamed to your project name.
* `<environment>` is a placeholder to be replaced by `staging` or `production` (for example).

### Sage/Theme

1. Install Sage's [requirements](https://github.com/roots/sage#requirements)
2. SSH into VM: `$ vagrant ssh`
3. Add the [Soil plugin](https://roots.io/plugins/soil/): `$ cd /srv/www/roots-example-project.com/current && composer require roots/soil dev-master` (note: non-development use requires a license that you can purchase on the Roots site)
4. Activate Soil: `$ wp plugin activate soil`
5. [Configure Sage](https://github.com/roots/sage#theme-development) and customize the theme as usual. At a minimum, do this on your host/local machine:

```bash
$ cd web/app/themes/sage
$ npm install
$ bower install
$ gulp
```
