<p align="center"><img src="https://raw.githubusercontent.com/srowlands/govcms-drupal-vm/master/docs/images/drupal-vm-logo.png" alt="Drupal VM Logo" /></p>

[![Build Status](https://travis-ci.org/srowlands/govcms-drupal-vm.svg?branch=master)](https://travis-ci.org/srowlands/govcms-drupal-vm) [![Documentation Status](https://readthedocs.org/projects/drupal-vm/badge/?version=latest)](http://docs.drupalvm.com)

govCMS Drupal VM is a development environment based off [Drupal VM](http://www.drupalvm.com/), built with Vagrant + Ansible.

This project aims to make spinning up a local or remote govCMS test/development environment using a standardised toolset.

It will install the following on an Ubuntu 14.04 (by default) linux VM:

  - Apache 2.4.x
  - PHP 5.5.x (configurable)
  - MySQL 5.5.x
  - Drush (configurable)
  - govCMS 2.x
  - Optional:
    - Varnish 4.x
    - Apache Solr 4.10.x (configurable)
    - Selenium, for testing your sites via Behat
    - Memcached
    - XHProf, for profiling your code
    - XDebug, for debugging your code
    - Adminer, for accessing databases directly
    - Pimp my Log, for easy viewing of log files
    - MailHog, for catching and debugging email

It should take 5-10 minutes to build or rebuild the VM from scratch on a decent broadband connection.

Please read through the rest of this README and the [Drupal VM Wiki](https://github.com/geerlingguy/drupal-vm/wiki) for help getting Drupal VM configured and integrated with your development workflow.

## Documentation

Full Drupal VM documentation is available at http://docs.drupalvm.com/

## Customizing the VM

This codebase comes preconfigured for the govCMS distribution. There are a couple places where you can customize the VM for your needs:

  - `config.yml`: Contains variables like the VM domain name and IP address, PHP and MySQL configuration, etc.
  - `drupal.make.yml`: Contains configuration for the Drupal core version, modules, and patches that will be downloaded on Drupal's initial installation (more about [Drush make files](https://www.drupal.org/node/1432374)).

## Quick Start Guide

This Quick Start Guide will help you quickly build a govCMS site on the Drupal VM using the Drupal.org release of govCMS.

### 1 - Install dependencies (VirtualBox, Vagrant, Ansible)

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads) (Drupal VM also works with Parallels or VMware, if you have the [Vagrant VMware integration plugin](http://www.vagrantup.com/vmware)).
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html).
  3. [Mac/Linux only] Install [Ansible](http://docs.ansible.com/intro_installation.html).

Note for Windows users: *Ansible will be installed inside the VM, and everything will be configured internally (unlike on Mac/Linux hosts). See [JJG-Ansible-Windows](https://github.com/geerlingguy/JJG-Ansible-Windows) for more information.*

Note for Linux users: *If NFS is not already installed on your host, you will need to install it to use the default NFS synced folder configuration. See guides for [Debian/Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-14-04), [Arch](https://wiki.archlinux.org/index.php/NFS#Installation), and [RHEL/CentOS](https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-centos-6).*

Note on versions: *Please make sure you're running the latest stable version of Vagrant, VirtualBox, and Ansible, as the current version of Drupal VM is tested with the latest releases. As of June 2015: Vagrant 1.7.2, VirtualBox 4.3.26, and Ansible 1.9.2.*

### 2 - Build the Virtual Machine

  1. Download this project and put it wherever you want.
  2. Create a local directory (/var/www/govcms-vm) where govCMS will be installed. You may change this location in `config.yml` (`local_path`, inside `vagrant_synced_folders`).
  3. Open Terminal, cd to this directory (containing the `Vagrantfile` and this README file).
  4. [Mac/Linux only] Install Ansible Galaxy roles required for this VM: `$ sudo ansible-galaxy install -r provisioning/requirements.txt --force`
  5. Type in `vagrant up`, and let Vagrant do its magic.

Note: *If there are any errors during the course of running `vagrant up`, and it drops you back to your command prompt, just run `vagrant provision` to continue building the VM from where you left off. If there are still errors after doing this a few times, post an issue to this project's issue queue on GitHub with the error.*

### 3 - Configure your host machine to access the VM.

  1. [Edit your hosts file](http://www.rackspace.com/knowledge_center/article/how-do-i-modify-my-hosts-file), adding the line `192.168.88.88  drupalvm.dev` so you can connect to the VM. Alternatively, you can install a Vagrant plugin to automatically add and remove the entry from your hosts file; run `vagrant plugin install vagrant-hostsupdater`. The plugin will also add in all other hosts defined via `apache_vhosts`.
  2. Open your browser and access [http://govcms.dev/](http://govcms.dev/). The default login for the admin account is `admin` for both the username and password.

## Extra software/utilities

By default, this VM includes the extras listed in the `config.yml` option `installed_extras`:

    installed_extras:
      - adminer
      - mailhog
      - memcached
      - pimpmylog
      # - solr
      # - selenium
      - varnish
      - xdebug
      - xhprof

If you don't want or need one or more of these extras, just delete them or comment them from the list. This is helpful if you want to reduce PHP memory usage or otherwise conserve system resources.

## Using Drupal VM

Drupal VM is built to integrate with every developer's workflow. Many guides for using Drupal VM for common development tasks are available on the [Drupal VM Wiki](https://github.com/geerlingguy/drupal-vm/wiki):

  - [Syncing Folders](http://docs.drupalvm.com/en/latest/extras/syncing-folders/)
  - [Connect to the MySQL Database](http://docs.drupalvm.com/en/latest/extras/mysql/)
  - [Use Apache Solr for Search](http://docs.drupalvm.com/en/latest/extras/solr/)
  - [Use Drush with Drupal VM](http://docs.drupalvm.com/en/latest/extras/drush/)
  - [Use Drupal Console with Drupal VM](http://docs.drupalvm.com/en/latest/extras/drupal-console/)
  - [Use Varnish with Drupal VM](http://docs.drupalvm.com/en/latest/extras/varnish/)
  - [Use MariaDB instead of MySQL](http://docs.drupalvm.com/en/latest/extras/mariadb/)
  - [View Logs with Pimp my Log](http://docs.drupalvm.com/en/latest/extras/pimpmylog/)
  - [Profile Code with XHProf](http://docs.drupalvm.com/en/latest/extras/xhprof/)
  - [Debug Code with XDebug](http://docs.drupalvm.com/en/latest/extras/xdebug/)
  - [Catch Emails with MailHog](http://docs.drupalvm.com/en/latest/extras/mailhog/)
  - [Test with Behat and Selenium](http://docs.drupalvm.com/en/latest/extras/behat/)
  - [PHP 7 on Drupal VM](http://docs.drupalvm.com/en/latest/other/php-7/)
  - [Drupal 6 Notes](http://docs.drupalvm.com/en/latest/other/drupal-6/)

## Other Notes

  - To shut down the virtual machine, enter `vagrant halt` in the Terminal in the same folder that has the `Vagrantfile`. To destroy it completely (if you want to save a little disk space, or want to rebuild it from scratch with `vagrant up` again), type in `vagrant destroy`.
  - When you rebuild the VM (e.g. `vagrant destroy` and then another `vagrant up`), make sure you clear out the contents of the `drupal` folder on your host machine, or Drupal will return some errors when the VM is rebuilt (it won't reinstall Drupal cleanly).
  - You can change the installed version of Drupal or drush, or any other configuration options, by editing the variables within `config.yml`.
  - Find out more about local development with Vagrant + VirtualBox + Ansible in this presentation: [Local Development Environments - Vagrant, VirtualBox and Ansible](http://www.slideshare.net/geerlingguy/local-development-on-virtual-machines-vagrant-virtualbox-and-ansible).
  - Learn about how Ansible can accelerate your ability to innovate and manage your infrastructure by reading [Ansible for DevOps](https://leanpub.com/ansible-for-devops).

## License

This project is licensed under the MIT open source license.

## About the Author

[Jeff Geerling](http://jeffgeerling.com/), owner of [Midwestern Mac, LLC](http://www.midwesternmac.com/), created this project in 2014 so he could accelerate his Drupal core and contrib development workflow. This project, and others like it, are also featured as examples in Jeff's book, [Ansible for DevOps](https://leanpub.com/ansible-for-devops).
