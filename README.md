# Roots Example Project

This repository is an example of how to integrate and use the following projects together:

* [Bedrock](https://github.com/roots/bedrock)
* [Trellis](https://github.com/roots/trellis)
* [Sage](https://github.com/roots/sage) (with [Soil](https://github.com/roots/soil))

For more background, see this [blog post](https://roots.io/a-modern-wordpress-example/).

This project is a complete working example that's deployed on a [Digital Ocean](https://roots.io/r/digitalocean/) 512MB droplet.

You can view it at http://roots-example-project.com/.

## Requirements

Make sure all dependencies have been installed before moving on:

* [Ansible](http://docs.ansible.com/ansible/intro_installation.html#latest-releases-via-pip) >= 2.0.0.2
* [Virtualbox](https://www.virtualbox.org/wiki/Downloads) >= 4.3.10
* [Vagrant](http://www.vagrantup.com/downloads.html) >= 1.5.4
* [vagrant-bindfs](https://github.com/gael-ian/vagrant-bindfs#installation) >= 0.3.1 (Windows users may skip this)
* [vagrant-hostmanager](https://github.com/smdahlen/vagrant-hostmanager#installation)

## Instructions

Here's how this example project was created:

1. Create a new project directory: `$ mkdir example.com && cd example.com`
2. Clone Trellis: `$ git clone --depth=1 git@github.com:roots/trellis.git && rm -rf trellis/.git`
3. Clone Bedrock: `$ git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git`
4. Clone Sage: `$ git clone --depth=1 git@github.com:roots/sage.git site/web/app/themes/sage && rm -rf site/web/app/themes/sage/.git`

```shell
example.com/      # → Root folder for the project
├── trellis/      # → System management & deployment
└── site/         # → A Bedrock-based WordPress site
    └── web/
        ├── app/  # → WordPress content directory (themes, plugins, etc.)
        └── wp/   # → WordPress core (don't touch!)
```

## Local development setup

1. **Clone this repository** into a working directory (e.g., `~/Sites`)
  ```shell
  $ git clone git@github.com:roots/roots-example-project.com.git
  ```

2. **Install external Ansible roles/packages**
  ```shell
  # @ roots-example-project.com/trellis
  $ ansible-galaxy install -r requirements.yml
  ```

3. **Install theme components**
  ```shell
  # @ roots-example-project.com/site/web/app/themes/sage
  $ npm install
  $ bower install
  $ gulp
  ```

4. **Fire up the server** (be patient, but watch the console––it may prompt for your system password)
  ```shell
  # @ roots-example-project.com/trellis
  $ vagrant up
  ```
  _Note: to shut down the server:_ `vagrant halt`

5. **Test the install** at [roots-example-project.dev](http://roots-example-project.dev/)

# Remote server setup (staging/production)

### Provision server:
```shell
# @ roots-example-project.com/trellis
$ ansible-playbook server.yml -e env=<environment>
```

### Deploy:
```shell
# @ roots-example-project.com/trellis
./deploy.sh <environment> roots-example-project.com

# OR
ansible-playbook deploy.yml -e "site=roots-example-project.com env=<environment>"
```

**To rollback a deploy:**
```shell
ansible-playbook rollback.yml -e "site=roots-example-project.com env=<environment>"
```

# Theme development

In **development**, run gulp in _watch_ mode for live updates at [localhost:3000](http://localhost:3000). **Important**: always use the [roots-example-project.dev](http://roots-example-project.dev/wp/wp-admin/) URL to access the WordPress admin.
```shell
# @ roots-example-project.com/site/web/app/themes/sage
$ gulp watch
```

**Production** assets (minified CSS, JavaScript, images, fonts, etc.) need to be compiled. Run gulp with the `--production` flag. The resulting files will be saved in `themes/sage/dist/`. Never edit files in the `dist` directory.

```shell
# @ roots-example-project.com/site/web/app/themes/sage
$ gulp --production
```
