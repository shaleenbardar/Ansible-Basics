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
## Ansible Modules
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

## Ansible Variables
Stores information that varies with each hosts
1) Variable be used-> "vars"
2) Used by ""{{ variable_name }}""
3) If you use variables with a string you need '' at start  and end of the string
for exaple -> 
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    vars:
        car_model: 'BMW M3'
        country_name: 'USA'
        title: 'Systems Engineer'
    tasks:
        -
            name: 'Print my car model'
            command: 'echo "My car''s model is {{ car_model }}"'
        -
            name: 'Print my country'
            command: 'echo "I live in the {{ country_name }}"'
        -
            name: 'Print my title'
            command: 'echo "I work as a {{ title }}"'

## Conditionals
1) "When"
2) use "=="
3) Could also use one condition as an output and an input to other task with "register directive"
for example-> 
-
    name: 'Add name server entry if not already entered'
    hosts: localhost
    tasks:
        -
            shell: 'cat /etc/resolv.conf'
            register: command_output
        -
            shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
            when: 'command_output.stdout.find("10.0.250.10") == -1'


## LOOPS
1) used by loops
2) accessed by {{ item }}
Anaother way to use loops will be "with_items"
for example->
-
    name: 'Install required packages'
    hosts: localhost
    vars:
        packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt
    tasks:
        -
            yum: 'name={{ item }} state=present'
            with_items: '{{ packages }}'

## Roles
# ansible-galaxy install mysql
# to list all roles-> $ ansible-galaxy list, $ ansible-config dump | grep ROLE
To Create roles automatically -> 
1) Make a roles directory
2) Now cd roles, then "$ ansible-galaxy init mysql_db"
3) roles created successfully
