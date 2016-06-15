# Drupal Install Profile Tools

After cloning this repo cd into the directory and run the init.sh script to create default variables, configuration file and inventory. Then modify the vars/defaults.yml, ansible.cfg and hosts files to suit your needs. You may also specify a different hosts file in the ansible.cfg file or make other tweaks in that file as needed.

All files created by the init script are safe to delete or modify. If you delete them and want to reference them in the future, just see the files in the examples directory.

Override the default variables on a per-site basis by adding additional files to the vars directory. Name each vars file for the site's domain. For example:

 - vars/example.com.yml
 - vars/example.dev.yml

This allows you to specify different options for live and dev sites, or any other differences per domain.

Create a git repo in the vars directory to keep track of your changes, it will be ignored from this project's repo. This will allow you to keep up to date with changes to the roles and playbooks while keeping your variables separate.

To run a playbook just specify the domain as an extra variable. For example:

`ansible-playbook drupal8_full.yml -e "domain=example.com"`

## Composer

All playbooks and roles assume your Drupal 8 site was originally created with the drupal-project Composer template available at:

https://github.com/drupal-composer/drupal-project

You can run the drupal8_new.yml playbook to create a new project.

`ansible-playbook drupal8_new.yml -e "domain=example.com"`

## Ignore files

Make sure the gitignore file in your site's repository excludes the following files:

 - web/sites/default/settings.php
 - web/sites/default/services.yml
 - web/sites/default/settings.local.php
 
That's necessary since we're rebuilding and reinstalling a lot. The roles included here handle the creation and management of those files. Later on when you're ready to go live you can include those files since you won't be rebuilding any more.

## Playbooks:

 - **drupal8_full:** Removes all files and rebuilds from the specified git repo then installs the site. Make sure any changes to your files have been committed and pushed!
 - **drupal8_reinstall:** Reinstalls the site without removing the files or rebuilding the code base.
 - **drupal8_reset:** Uninstalls the site and prepares it to be reinstalled. Useful when testing manual installation.
 - **drupal8_new:** Creates a new Drupal 8 codebase from the drupal-project Composer template.

## Roadmap:

 - Better documentation for variables.
 - Most roles have D8 prefixing them, so D7 versions may be done as well.
 - Share common handlers. Still using handlers in roles directories since that wasn't working for a while when Ansible 2.0 was released.
 - Use Ansible Composer module instead of shell. It's not working at all for me locally, but shell works fine for now.
