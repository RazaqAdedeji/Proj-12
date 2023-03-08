## ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

1. Jenkins job enhancement

- Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

'sudo mkdir /home/ubuntu/ansible-config-artifact'

- Change permissions to this directory, so Jenkins could save files there

'chmod -R 0777 /home/ubuntu/ansible-config-artifact'

![alt text](./images/1%20mkdir%20conf-art.png)

- Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![alt text](./images/2%20install%20copy%20artifact.png)

- Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch)

![alt text](./images/3%20edit%20readme.png)

![alt text](./images/4%20test%20build.png)

2. Refactor Ansible code by importing other playbooks into site.yml

- Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

- Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

- Move common.yml file into the newly created static-assignments folder.

- Inside site.yml file, import common.yml playbook.

![alt text](./images/5%20common%20plybk.png)

![alt text](./images/6%20common%20del.png)

![alt text](./images/7%20site%20yml.png)

![alt text](./images/8%20run%20plybk.png)

![alt text](./images/9%20run%20plybk%20cont.png)

- Make sure that wireshark is deleted on all the servers by running wireshark --version

![alt text](./images/10%20unistal%20wireshark%20conf.png)

![alt text](./images/11%20unistal%20wireshark%20conf.png)

3. Configure UAT Webservers with a role ‘Webserver’

![alt text](./images/Screen%20Shot%202023-03-08%20at%202.40.41%20AM.png)

- To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.

- Create the directory/files structure manually

![alt text](./images/12%20folder%20structure.png)

- Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers

In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.

![alt text](./images/13%20edit%20role%20path.png)

4. Reference ‘Webserver’ role

5. Commit & Test

![alt text](./images/14%20git%20push%20n%20pull.png)

- Now run the playbook against your uat inventory and see what happens:

'sudo ansible-playbook -i /home/ubuntu/ansible-config-artifact/inventory/uat.yml /home/ubuntu/ansible-config-artifact/playbooks/site.yml'

![alt text](./images/15%20plybk%20succesful.png)

- You should be able to see both of your UAT Web servers configured and you can try to reach them from your browser:

'http://<Web1-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php'

![alt text](./images/16%20webpage.png)

