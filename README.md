# Deploy ic-webapp
## Liste des roles
* docker_role : https://github.com/lianhuahayu/docker_role.git
    * installe tous les outils necessaire pour utiliser docker et docker-compose
* odoo_role : https://github.com/lianhuahayu/odoo_role.git
    * lance deux conteneurs (odoo et postgres)
* pgadmin_role : https://github.com/Yellow-carpet/pgadmin_role.git
    * lance un conteneur (pgadmin liée à un database postgres)
* ic-webapp_role : https://github.com/omarpiotr/ic-webapp_role.git
    * lance un conteneur ic-webapp depuis l'image créer et stockée sur dockerhub 

## example d'utilisation 

* ec2: 
    * odoo : 54.221.36.222 / ec2-54-221-36-222.compute-1.amazonaws.com
    * pg-admin : 34.229.74.135 / ec2-34-229-74-135.compute-1.amazonaws.com


```bash
# install requirements
ansible-galaxy install -r roles/requirements.yml

# deploy odoo
ansible-playbook -i hosts.yml playbook_odoo.yml \
    -e ansible_connection='ssh' \
    -e ansible_host='54.221.36.222' \
    --private-key '/home/ubuntu/.ssh/capge_projet_kp.pem'

# deploy pgamdin
ansible-playbook -i hosts.yml playbook_pgadmin.yml \
    -e ansible_connection='ssh' \
    -e ansible_host='34.229.74.135' \
    -e host_db='54.221.36.222' \
    --private-key '/home/ubuntu/.ssh/capge_projet_kp.pem'

# (exemple 1) deploy ic-webapp
ansible-playbook -i hosts.yml playbook_ic-webapp.yml \
    -e ansible_connection='ssh' \
    -e ansible_host='34.229.74.135' \
    -e odoo_url='http://ec2-54-221-36-222.compute-1.amazonaws.com:8069' \
    -e pgadmin_url='http://ec2-34-229-74-135.compute-1.amazonaws.com:5050' \
    --private-key '/home/ubuntu/.ssh/capge_projet_kp.pem'

# (exemple 2) deploy ic-webapp
ansible-playbook -i hosts.yml playbook_ic-webapp.yml \
    -e ansible_connection='ssh' \
    -e ansible_host='34.229.74.135' \
    -e odoo_url='http://ec2-54-221-36-222.compute-1.amazonaws.com:8069' \
    -e pgadmin_url='http://ec2-34-229-74-135.compute-1.amazonaws.com:5050' \
    -e ic_webapp_image='lianhuahayu/ic-webapp:1.0' \
    -e ic_webapp_port='8080' \
    --private-key '/home/ubuntu/.ssh/capge_projet_kp.pem'

# (exemple 3) deploy ic-webapp
ansible-playbook -i hosts.yml playbook_ic-webapp.yml \
    -e ansible_connection='ssh' \
    -e ansible_host='34.229.74.135' \
    -e odoo_url='http://54.221.36.222:8069' \
    -e pgadmin_url='http://34.229.74.135:5050' \
    --private-key '/home/ubuntu/.ssh/capge_projet_kp.pem'


    
```