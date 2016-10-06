# wordpress-ansible

### Generate ssh key:
TODO: brief description for generating purpose

    ssh-copy-id -i ~/ssh/id_rsa.pub user@hostname

### Encrypt your sensitive playbook
Create text file for storing encryption key then encrypt your sudo password. In this playbook, I put variable ``ansible_become_pass``, specific sudo password for ``wordpress-ubuntu`` host inside ``host_vars``.

      echo '2016$10$05' > ~/.vault_pass.txt

      ansible-vault encrypt host_vars/wordpress-ubuntu --vault-password-file ~/.vault_pass.txt


***Note***
If you want to decrypt ``host_vars/wordpress-ubuntu``, type:

    ansible-vault decrypt host_vars/wordpress-ubuntu --vault-password-file ~/.vault_pass.txt

### Execute your playbook

    ansible-playbook playbook.yml -i hosts --vault-password-file ~/.vault_pass.txt


## References
* [How to automate installing wordpress on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible)
