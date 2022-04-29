# Ansible-Basics

## Ansible Basics ->
1) Ansible don't need any agent to connect to different servers or machines, it only need ssh or powershell connection
2) You can create cluster for servers in ansible.ini (investory file) with [server cluster name] and under that can assign different servers


## Inventory_parameters ->
1) ansible_host
2) ansible_connection
3) ansible_port
4) ansible_user (by fefault - root)
5) ansible_ssh_pass -> password
    you can define like ->
    web   ansible_host=server1  ansible_connection=ssh    ansible_user=root   ansible_ssh_pass = asnkaadsn@ams
    db    ansible_host=server2  ansible_connection=winrm  ansible_user=admin
    mail  ansible_host=server3  ansible_connection=ssh    ansible_user=root
    web2  ansible_host=server4  ansible_connection=winrm  ansible_user=admin

# Make an inventory file ->
1) cat > inventory.txt
2) not to run it use "ansible servername -m ping -i inventory.txt"
3) To refer to the inventory file server or groups, you need to add cluster_names"_nodes"

## Ansible Playbook ->
1) It can be as easy as writing some commands at vm's
2) but it can be as complicated as writing workflow and resources, connection between several vm's
3) Different scrpit or work done by tasks are modules, ex -> yum, service, script, command etc.
4) To run ansible playbook -> $ ansible-playbook playbook.yml 
5) For example -> 
####
name: Play 2
hosts: could be any server name from your inventory_file
tasks:
  - name: Install web service
    yum:
      name: httpd
      state: present
  - name: Start web server
    service:
      name: httpd
      state: started
##Ansible Modules
1) System Modules - for system related funtions and task
2) Commands - Execute commands on hosts
3) files -> to work with files
4) database-> work wit hdatabases
5) cloud -> work with cloud providers ex. docker, aws
6) windows-> use ansible on windows
7) script: executes a script on remote machine
example -> 
    name: "Execute a script on all web server nodes"
    hosts: web_nodes
    tasks:
        name: Execute a script on all web server nodes
        script: /tmp/install_script.sh

8) service -> is used for services up and down, restart
9) User -> to create a user
10) lineinfile -> name suggests it's funtion
for example->
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Create a new web user'
            user:
                name: web_user
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present
