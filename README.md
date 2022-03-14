# Ansible
This repository contains information of Ansible

Ansible official documentation
https://docs.ansible.com/

Installation:
https://docs.ansible.com/ansible/latest/installation_guide/index.html

Installation on linux:
-------------------------
$ sudo apt-get install software-properties-common
(  software-properties-common    ,  is a base package which is required to install ansible )
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install -y ansible
+++++++++++++++++++++++++++++++++
To check ther version of ansible
$ ansible --version

Configuration Management
-----------------------------------
This is process of configuring remote servers from one point of control.

Advantages
----------------------
1) Provisioning of servers
	The applications that should be installed on server can be done very quickly from a single centralized location.

2) Idempotent 
	Configuration management tools are used to bring the server to a particular state, called as desired state. If a server already in the desired state, configuration management tools will not reconfigure that server.


Note: Cofiguration management tools cannot be used for installing OS from the scratch.
They can be used only for managing the applications on top of the OS.


COnfigutaion management tools -  Ansible, chef, puppet, salt  etc

++++++++++++++++++++++++++++++++++++++++++

Ansible  -- It is a open source configuration management tool, created using Python.
Main machine in which anisble is installed, is called as controller.
Remote severs that Ansible configures, are called as managed nodes.

Ansible uses agent less policy for configures remote servers ie Ansible is installed only on 1 machine, and we do not require any client side software to be installed on the remote serers.

Ansible performs configuration management through password less ssh.

2 Ways  ansible can used
1) adhoc commands
2) playbooks

Write the ip address of nodes in the inventory file
$ cd /etc/ansible
$ ls
$ sudo vim hosts

adhoc commands
----------------
Important modules in ansible

Syntx of adhoc commands
$ ansible  all/group_name/ipaddress -i  path_of_inventory_file -m modulename  -a  'arguments'

1) command - This module is used for executing basic linux commands on managed nodes.
            $ ansible all -i myinventory -m command -a 'free'
            
2) shell -  This module is used to execute commands which involved redirection and piping and to execute shell scripts on managed nodes.
            $ ansible  all  -m  shell -a 'ls -la > file2'
            
3) ping  --  This module is used to check if the remote server is pingable or not.
            $ ansible all -m ping.
            
4) user --  This module is used for user management like create user, setting password, assign home directory  etc
            $ ansible  all -m user  -a  'name=nageshwa  password=welcome'  ( we ger error : permission denied )
            $ ansible  all -m user  -a  'name=kiran  password=welcome'  -b  ( become , for higher privileges on managed nodes )
            $ ansible all -m user  -a 'name=Ravi password=freefree uid=1234 comment="A regular user"  home=/home/ubuntu/Ravi shell=/bin/bash' -b
            
5) copy  --  This module is used to copy the files and folders from controller to managed nodes
            $ ansible all -m  copy  -a 'src=/local/mnt/workspace/script     dest=/tmp'

6) fetch  --  This module is used to copy files and folder from managed nodes to controller
            $ ansible all  -m fetch  -a 'src=/local/mnt/workspace/script  dest=/tmp'  -b
            
7) file  --  This module is used for creating or deleting files and folders on managed nodes.
            $ ansible all  -m  file -a  'name=/local/mnt/workspace/file  state=touch' ( to create file)
            $ ansible all  -m  file -a  'name=/local/mnt/workspace/file  state=absent' (to delete file)
                state=touch   is to create files
                state=directory  is to create directory
                state=absent  is for deleting file/directory
            $  ansible all  -m  file -a  'name=file1  state=touch owner=nageshwa group=video  mode=700'  -b
                    The above command will execute  only if Anu user and Ravi  group is available in all nodes.
                    Notes:
                    File module can be used to change the ownership, group ownership and permissions on the file.
            
8) stat  --  Used to capture detailed information about files and folders present in managed nodes.
9) debug --  Used to display output of any module
10) apt   --  Used for performing package management on managed nodes ie installing softwares / upgrading repositories  etc . It works on ubuntu, debain flavours of linux. 
              $ ansible all -m apt  -a 'name=git  state=present' -b
                    state=present  is for installation
                    state=latest    for upgradation
                    state=absent    for uninstallation

11) yum  --  similar to apt module. It works on Red hat linux, centos etc
12) git  --  used to perform git version controlling on managed nodes
              $ ansible all -m  git  -a  'repo=https://github.com/nageshwar1991/Ansible.git  dest=/tmp/mygit' -b

13) replace -- This is used to replace specific text in configuration file with some other text.
              $  ansible all  -m replace  -a 'regexp=1234 replace=5678 path=/local/mnt/workspace/file'  -b

14) service  -- used for starting / stoping / restarting services on managed nodes.
                $  ansible all   -m service    -a 'name=tomcat8  state=restarted'  -b
                state=restarted is for restarting a service
                state=stopped  is for stopping a running service
                state=started  is for starting a stopped service
                
15) include  --  Used for calling child play books  from parent play book
16) uri    -- useful in checking  if remote url is reachable or not.
              $ ansible all -m   uri  -a 'url=http://videodash'

17) docker_container  --  used to execute docker  commands related to container management on managed nodes
18) docker_image  --  used to execute commands related to docker images on managed nodes.
19) docker_login  --  used to login to docker hub from managed nodes.
20) setup   --  used to capturing system information related to the managed nodes.
++++++++++++++++++++++++++++++++++++++++++++++



Play books
----------

Adhoc commands are capable of working only on one module and one set of arguments.
When we want to perform complex configuration management activities, adhoc commands will be difficult to manage.

In such scenarios, we use play books.
Play book is combination of plays.
Each play is designed to do some activity on the managed nodes.
These plays are created to work on single host or a group of hosts or all the hosts.

The main advantage of play books  is reusability.
Play books are created using  yaml files.


