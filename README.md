# wordpress-ansible
This playbook is intended to install Wordpress application based on LEMP (Linux, EngineX, MySQL and PHP).

The linux server distribution is based on Ubuntu 16.04 and Ansible version 2.1.2.0

For Ubuntu 14.04 version, you can see on branch WordpressUbuntu14.04

## Writing Playbook
### Generate ssh key:
Use locally available keys to authorize logins on a remote machine

    ssh-copy-id -i ~/ssh/id_rsa.pub user@hostname

### Encrypt your sensitive playbook
Create text file for storing encryption key then encrypt your sudo password. In this playbook, I put variable ``ansible_become_pass``, specific sudo password for ``wordpress-ubuntu`` host inside ``host_vars``.

      echo '2016$10$05' > ~/.vault_pass.txt

      ansible-vault encrypt host_vars/wordpress-ubuntu --vault-password-file ~/.vault_pass.txt


***Note***
If you want to decrypt ``host_vars/wordpress-ubuntu``, type:

    ansible-vault decrypt host_vars/wordpress-ubuntu --vault-password-file ~/.vault_pass.txt

## Roles
#### server
Since apache2 has already installed on my Ubuntu server, so I need to stop the service and restart nginx. See notify part in ``roles/server/tasks/main.yml``.

#### php
Due to prevention of PHP to attempt to execute the closest file it can find if a PHP file does not match exactly, ``cgi.fix_pathinfo`` has to be set 0. However, the ``lineinfile`` append to the last line every time the playbook is executed. Therefore one task name ``checking whether cgi.fix_pathinfo value is 1`` to prevent that behaviour.

#### mysql
Pretty clear here. Only encrypting ``defaults\main.yml`` because it contains sensitive information like *DB_PASSWORD*

#### wordpress
This roles downloading latest wordpress then install into nginx www directory, in this case ``/usr/share/nginx/``. Then copying the nginx configuration from template in order to make wordpress default website.

## Execute your playbook

    ansible-playbook playbook.yml -i hosts --vault-password-file ~/.vault_pass.txt


## References
* [How to automate installing wordpress on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible)
* [How to install LEMP](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-14-04?utm_source=legacy_reroute)
