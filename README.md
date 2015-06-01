# Roots Example Project

This repository is an example of how to integrate and use the following projects together:

* [Bedrock](https://github.com/roots/bedrock)
* [bedrock-ansible](https://github.com/roots/bedrock-ansible)
* [Sage](https://github.com/roots/sage) (with [Soil](https://github.com/roots/soil))

For more background, see this [blog post](https://roots.io/a-modern-wordpress-example/).

This project is a complete working example that's deployed on a [Digital Ocean](https://roots.io/r/digitalocean/) 512MB droplet.

You can view it at http://roots-example-project.com/.

## Instructions

This project can be cloned and re-configured to fit your needs but we highly suggest you follow in the instructions below to create your own. This will guarantee you have the latest version of Bedrock, bedrock-ansible, Sage, and Soil in case this example falls behind a little.

Here's how this example project was created:

1. Create a new project directory: `$ mkdir example.com && cd $_`
2. Clone bedrock-ansible: `$ git clone --depth=1 git@github.com:roots/bedrock-ansible.git ansible && rm -rf ansible/.git`
3. Clone Bedrock: `$ git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git`
4. Clone Sage: `$ git clone --depth=1 git@github.com:roots/sage.git site/web/app/themes/sage && rm -rf site/web/app/themes/sage/.git`
5. Move `Vagrantfile` to root: `$ mv ansible/Vagrantfile .` and update the [ANSIBLE_PATH](https://github.com/roots/roots-example-project.com/blob/master/Vagrantfile#L6) to `'ansible'`

After that your folder structure is complete and you're ready to configure the individual components.

### Bedrock

Bedrock doesn't need any additional configuration by default. There's only one custom option set in this example: `WP_DEFAULT_THEME`.

* In `site/config/application.php` add `define('WP_DEFAULT_THEME', 'sage');`

### Sage/Theme

1. Install Sage's [requirements](https://github.com/roots/sage#requirements)
2. Add Soil: `$ cd site && composer require roots/soil`
3. [Configure Sage](https://github.com/roots/sage#theme-development) and customize the theme as usual. At a minimum:

```bash
$ cd web/app/themes/sage
$ npm install
$ bower install
$ gulp
```

### bedrock-ansible

bedrock-ansible's [instructions](https://github.com/roots/bedrock-ansible) apply here, but more specifically:

1. Make sure you have the [requirements](https://github.com/roots/bedrock-ansible#requirements) all installed
2. Install the Ansible Galaxy roles: `$ cd ansible && ansible-galaxy install -r requirements.yml`
3. Configure your `wordpress_sites`: [docs](https://github.com/roots/bedrock-ansible#wp-sites) and follow our [example](https://github.com/roots/roots-example-project.com/blob/master/ansible/group_vars/development)

#### Staging/Production

If you also want staging/production servers, create those manually at this point.

1. Add their hostnames/IPs to `ansible/hosts/<environment>`
2. Configure their `wordpress_sites` just like above in #3 and follow our [example](https://github.com/roots/roots-example-project.com/blob/master/ansible/group_vars/production)
3. Define your `github_ssh_keys` to give users the ability to deploy. Follow our [example](https://github.com/roots/roots-example-project.com/blob/master/ansible/group_vars/production#L3-L9) and read the [Wiki](https://github.com/roots/bedrock-ansible/wiki/SSH-Keys).


### Provision

#### Development
1. Run `vagrant up`

#### Staging/Production
1. Provision server: `ansible-playbook -i hosts/<environment> server.yml`
2. Deploy your site: `./deploy.sh <environment> <site name>`

# Naming

Some notes on names used throughout this project:

* You're encouraged to rename Sage to your theme name. Just remember to rename references to it everywhere.
* Any time you see a name like "example.com", "example.dev", "roots-example-project.com", etc, it should be renamed to your project name.
* `<environment>` is a placeholder to be replaced by `staging` or `production` (for example).
