# TD
## Connect to serv
```
ssh -i ~/Bureau/Cours/pasRSA/id_rsa centos@bastien.moronval.takima.cloud 
```

## Ping avec les host dans un inventory
```
ansible all -m ping --private-key=~/Bureau/Cours/pasRSA/id_rsa -u centos -i ansible/inventories/setup.yml
```

## Install apache
```
ansible all -m yum -a "name=httpd state=present" --private-key=~/Bureau/Cours/pasRSA/id_rsa -u centos -i ansible/inventories/setup.yml --become
```

## Create HTML
```
ansible all -m shell -a 'echo "<htlm><h1>Hello CPE</h1></html>" >> /var/www/html/index.html' --private-key=~/Bureau/Cours/pasRSA/id_rsa -u centos -i ansible/inventories/setup.yml --become
```

## Run apache
```
ansible all -m service -a "name=httpd state=started" --private-key=~/Bureau/Cours/pasRSA/id_rsa -u centos -i ansible/inventories/setup.yml --become
```

# TP
## Ping
```
ansible all -i ansible/inventories/setup.yml  -m ping
```

## Get Os distrub
```
ansible all -i ansible/inventories/setup.yml  -m setup -a "filter=ansible_distribution*"
```

## Remove Apache
```
ansible all -i ansible/inventories/setup.yml  -m yum -a "name=httpd state=absent" --become
```

## Execute playbook
```
ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml --syntax-check
ansible-playbook -i ansible/inventories/setup.yml ansible/playbook.yml
```

## Install docker
```
ansible-playbook -i ansible/inventories/setup.yml ansible/setup_docker.yml --syntax-check
ansible-playbook -i ansible/inventories/setup.yml ansible/setup_docker.yml
```

## Création des rôle
```
ansible-galaxy init ansible/roles/docker --force
ansible-galaxy init ansible/roles/network --force
ansible-galaxy init ansible/roles/database --force
ansible-galaxy init ansible/roles/api --force
ansible-galaxy init ansible/roles/front --force
```

