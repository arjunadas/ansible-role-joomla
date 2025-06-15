данный плейбук: install_joomla.yml

устанавливает стек:
- nginx:1.18
- php:8.1
- mysql:8
- joomla:5.1

проверено на ОС:
Ubuntu 22.04

нпже описаны шаги, которые необходимо выполнить

1) в файле install_joomla.yml
указать ip адрес целевой машины
```
joomla_ip: 192.168.0.85
```

2) далее по инструкции:
```
git clone https://github.com/arjunadas/ansible-role-joomla.git

cd ansible-role-joomla

# install requirements
ansible-galaxy collection install community.mysql

# this example of an inventory file
cat > inventory.yml << EOF
all:
  hosts:
    web01:
      ansible_host: 192.168.0.85 # write your address, where we will install the role.

  vars:
    ansible_user: admin1 # write your username and password
    ansible_password: 123456 # write your username and password
    ansible_python_interpreter: auto_silent
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
EOF

# ansible vault is used in this role

PASS_FOR_VAULT='123' # write your password for vault

echo "$PASS_FOR_VAULT" > .vault_pass.txt
	
cat > joomla.vault << EOF
---
mysql_user_name: admin
mysql_user_password: LNGsgS4rxC7t7KmLaP9q
priv: '*.*:ALL'
mysql_root_username: root
mysql_root_password: iDaeKZDGlU5vBLsaoZvt
EOF

# encrypt vault
ansible-vault encrypt joomla.vault --vault-password-file .vault_pass.txt

# start play
ansible-playbook -i inventory.yml install_joomla.yml -e "@joomla.vault" --vault-password-file .vault_pass.txt  
```
