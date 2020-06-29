Pre requisites 
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

About the playbook
-------------------------

The aim of this playbook is to deploy Kubernetes through Rancher on target VMs, install Helm, install Jenkins, install Gitlab, install Nexus.
Nexus is installed on K8, Jenkins and Gitlab on VM.
To set up the VM inventory please change in ```hosts``` file. 

**[master]** -> IPs of master node <br />
**[worker]** -> IPs of worker node <br />
**[rke]** -> Node where rke and kubectl will be installed <br />
**[gitlab]** -> Where Gitlab and Jenkins is installed <br />

**[[master:vars]]** -> user and ssh key to connect to master nodes. Same as variables <sudo_user> and <key_folder>/<key_name>
**[[worker:vars]]** -> user and ssh key to connect to worker nodes. Same as variables <sudo_user> and <key_folder>/<key_name>

Logs are generated in setup-log.log in the project directory. The location can be modified in ansible config file.

Known limitations
-------------------------

The VMs must already exist. <br />
The VMs must have same password. <br />
Firewall rules are common right now for master and worker nodes. <br />


Usage
-------------------------

```
ansible-playbook setup.yml -i hosts -u root --private-key <key_path> --extra-vars "sudo_user=<desired_sudo_user_name> sudoers=<desired_sudo_user_name> upassword=<desired_password_forsudo_user> 
ssh_password_of_nodes=<vm_password> rke_force_configure=false" 
--skip-tags "debug_info"
``` 

Ansible Config File
-------------------------

./ansible.cfg

Variables
-------------------------

```
remote_user : User already present on the target machines
resolv_nameservers : Array of  Nameserver IPs
sudoers : Array of Sudo user to be configured
upassword : Password of sudo usser to be configured
sudo_user : User with passswordless sudo access on the VMs, usually one from sudoers variable
kernel_modules_for_rke : Array of Kernel modules required for RKE
firewall_ports : Array of Firewall ports that needs to be open for RKE
key_folder : Location where ssh key for the sudo user should be generated
key_name : Name of ssh key to be generated
ssh_password_of_nodes : SSH password of the K8 nodes, required to copy ssh keys from RKE host machine
rke:
  services:
    kube_api:
      service_cluster_ip_range: CIDR IP range for any services created on Kubernetes
      pod_security_policy: Boolean Pod security policy
      always_pull_images: Boolean Whether to always pull images
      extra_args:
        anonymous-auth: Boolean
    kube_controller:
      cluster_cidr: CIDR pool used to assign IP addresses to pods in the cluster
      service_cluster_ip_range: IP range for any services created on Kubernetes, same as kube_api : service_cluster_ip_range
    kubelet:
      cluster_domain: Base domain for the cluster
      cluster_dns_server: IP address for the DNS service endpoint
      fail_swap_on: false
  network:
    plugin: Specify network plugin-in (canal, calico, flannel, weave, or none)
  authentication:
    strategy: x509
  ssh_key_path: ssh key path to connect to nodes, same as key_folder/key_name
  ssh_agent_auth: Boolean
  authorization:
    mode: Either rbac or none
  ignore_docker_version: Boolean
  kubernetes_version: The Kubernetes version used
  cluster_name: Name of the Kubernetes cluster  
  restore:
    restore: Boolean
    snapshot_name: String

kubectl_url: Download url for kubectl
rke_dir: Directory where files related to RKE will be genetrated
rke_force_configure: Boolean
```

Templates
-------------------------

- dns_role/templates/resolv.conf.j2
- rke_role/templates/cluster.j2