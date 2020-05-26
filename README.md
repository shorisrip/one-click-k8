# Pre requisites 
-------------------------
* ```sudo yum -y install epel-repo```
* ```sudo yum -y install ansible```
* ```ansible --version```
* ```ssh-keygen``` 
* ```ssh-copy-id <all_the_target_machine>```

# Usage
-------------------------
```ansible-playbook setup.yml -i hosts -u root --private-key <key_path> --extra-vars "sudoers=ananya upassword=my_password" --skip-tags "debug_info"```