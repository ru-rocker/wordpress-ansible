# wordpress-ansible

Generate ssh key:

    ssh-copy-id -i ~/ssh/id_rsa.pub user@hostname

Encrypt your sensitive playbook
* Create text file for storing encryption key

      echo '2016$10$05' > ~/.vault_pass.txt

* Encrypt your password sudo

      ansible-vault encrypt host_vars/wordpress-ubuntu --vault-password-file ~/.vault_pass.txt


Execute your playbook

    ansible-playbook playbook.yml -i hosts --vault-password-file ~/.vault_pass.txt


## References
* [How to automate installing wordpress on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-automate-installing-wordpress-on-ubuntu-14-04-using-ansible)
