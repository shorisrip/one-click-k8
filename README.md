# Pre requisites 
-------------------------
 
* Ensure k8 master and worker vms are created 
* Ensure k8 master and worker vms have same password

On your local machine, or the ansible host machine:
* Ensure ssh connectivity to all the target machines
* ```sudo yum -y install epel-repo```
* ```sudo yum -y install ansible```
* ```ansible --version```
* ```ssh-keygen``` 
* ```ssh-copy-id <all_the_target_machine>```
* ```cd <project_path>```

# About the playbook

The aim of this playbook is to deploy Kubernetes through Rancher on target VMs and install Helm. It :
- Creates a sudo user with ```<desired_sudo_user_name>``` and ```<desired_password_for_sudo_user>``` on rke host and the nodes 
- Configures DNS , installs required version of docker, required kernel modules, required networking rules for RKE 
- Generate and copy ssh key pair from RKE vm to k8 nodes with sudo user
- Installs RKE, K8 and Helm with sudo user  

* ##### To set up the VM inventory please change in ```hosts``` file. 
    - [master] -> IPs of master node
    - [worker] -> IPs of worker node
    - [rke] -> Node where rke and kubectl will be installed

* ##### To customize the k8 deployment parameters 
    - Copy the file ```roles/rke_role/vars/main.yml``` to pwd , edit and pass as variable file while running the playbook.


# Known limitations

The VMs must already exist.
The VMs must have same password.
RKE and Kubectl are installed in the same VM (preferably k8 master vm)


# Usage
-------------------------

```
ansible-playbook setup.yml -i hosts -u root --private-key <key_path> --extra-vars "sudoers=<desired_sudo_user_name> upassword=<desired_password_for_sudo_user> ssh_password_of_nodes=<vm_password>" 
--skip-tags "debug_info"
``` 