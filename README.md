
# [TrendWatching Premium](https://premium.trendwatching.com)

# Overview

TrendWatching Premium uses a combination of WordPress-related tools to make development and deployment easier. This doc will help you get the site up and running on your local machine.

Basic stack:
- [WordPress](https://wordpress.org/)
- [Trellis](https://roots.io/trellis/)
- [Bedrock](https://roots.io/bedrock/)
- [Sage](https://roots.io/sage/)

# Getting Started

There are two git repositories you'll need to clone in order to setup the site on your machine.

- Premium-next (this repo) - includes Sage (for the theme) and Bedrock (for better WordPress development)
- [Core](https://github.com/trendwatching/Core) – includes Trellis, for environment configuration and deployment.

[Trellis](https://roots.io/trellis/docs/installing-trellis/) relies on a number of packages in order to function correctly which can be seen below:
> Before you proceed, make sure you have the following installed: 
> * [Vagrant](https://www.vagrantup.com/downloads.html)  >= 2.1.0
> * [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Vagrant boxes are essentially VMs) >= 4.3.10
> * [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW)>= 2.4.0
> * [SequelPro](https://sequelpro.com/download)
> * [Node.js](https://nodejs.org/en/download/)
> * [NVM](https://github.com/nvm-sh/nvm) - You will need to use Node 4.5.0 for Gulp.
> 
> Run `vagrant -v` from the terminal to check it's installed. 

The above packages are required for Trellis to function properly. VirtualBox and Vagrant provide the virtual machine in which the development resides whilst Ansible provides the configuration to build and provision those VMs as well as the staging/production servers. 

Node is used to run frontend tasks runners such as Gulp and NVM allows you to change the version of Node for that instance of the console. The use of Gulp is defined by the Sage theme and requires Node 4.5.0 - higher versions may result in node package not resolving. SequelPro is a database explorer and can be replaced by another tool. 

### Setup
1. Create a directory on your machine with an appropriate name (e.g. `trendwatching-premium`):

  ```sh
  mkdir trendwatching-premium && cd trendwatching-premium
  ```

2. Clone this repo into a subdirectory named `site`: 

  ```sh
  git clone git@github.com:trendwatching/Premium-next.git site
  ```

3. Clone the Core repo into a subdirectory named `trellis`:

  ```sh
  git clone git@github.com:trendwatching/Core.git trellis
  ```

4. Your directory structure should now look like this:

  ```sh
  trendwatching-premium/
  ├── trellis/
  └── site/ 
  ```

5. Next, install the Ansible Galaxy roles from inside the `trellis` subdirectory:

  ```sh
  cd trellis && ansible-galaxy install -r requirements.yml
  ```

6. When you're ready to start development, run `vagrant up` from the `trellis` subdirectory to setup and provision your development environment.
7. Once provisioning is complete, you should be able to access the site by visiting `https://premium.trendwatching.local`. 

Additionally, for front-end development, you'll need to install the [dependencies for Sage](https://github.com/roots/sage/blob/8.5.4/README.md#theme-installation) which uses [gulp](http://gulpjs.com/) as its build system and [Bower](http://bower.io/) to manage front-end packages.


1. Install gulp and Bower globally with `npm install -g gulp bower`
2. Navigate to the theme directory, then run `npm install`
3. Run `bower install`
4. Within the theme folder (`web/app/themes/trendwatching-premium`) run `gulp` to start the default task which covers all asset types. 

If you run into any trouble during installation, consult the official [Trellis](https://roots.io/trellis/docs/installing-trellis/) and [Sage](https://github.com/roots/sage/blob/8.5.4/README.md#theme-installation) docs for help. 

You will need to import a database in order for use the local site correctly. First connect to the database using Sequel Pro with the below details (type SSH) and then navigate to `File > import` inside Sequel Pro:
| Field | Value |
|--|--|
| MySQL Host | 127.0.0.1 |
| Username | premium_trendwatching_com |
| Password | <check 1Password> |
| Database | n/a |
| Port | 3306 |
| SSH Host | 127.0.0.1 |
| SSH User | vagrant |
| SSH Key | /Username/trendwatching-premium/trellis/.vagrant/machines/default/virtualbox/private_key |
| SSH Port | 2222 |


### Vagrant

Basic usage:

`vagrant ssh` – SSH into the virtual machine

`vagrant up` – start the virtual machine

`vagrant halt` – halt the virtual machine

`vagrant destroy` – destroy the virtual machine (only the VirtualBox machine instance, running `vagrant up` again will rebuild the machine)

`vagrant provision` – re-provision the virtual machine after a configuration change

`vagrant reload` – reload the virtual machine
