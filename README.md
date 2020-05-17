# Usage

ansible-playbook users.yml -i hosts -u root --private-key <key_path> --extra-vars "sudoers=ananya upassword=my_password"