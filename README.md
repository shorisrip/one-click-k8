# Pre requisites 
-------------------------
* k8 master and worker vms are created 
* k8 master and worker vms have same password
* ```sudo yum -y install epel-repo```
* ```sudo yum -y install ansible```
* ```ansible --version```
* ```ssh-keygen``` 
* ```ssh-copy-id <all_the_target_machine>```
* ```cd <project_path>```

# Usage
-------------------------
```ansible-playbook setup.yml -i hosts -u root --private-key <key_path> --extra-vars "sudoers=<desired_sudo_user_name> upassword=<desired_password_forsudo_user> ssh_password_of_nodes=<vm_password>" 
--skip-tags "debug_info"```