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
