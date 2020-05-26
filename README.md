# Usage

ansible-playbook setup.yml -i hosts -u root --private-key <key_path> --extra-vars "sudoers=ananya upassword=my_password" --skip-tags "debug_info"