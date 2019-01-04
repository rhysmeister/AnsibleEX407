# AnsibleEX407
Study notes for the Ansible EX407 Exam

# Study points for the exam

To help you prepare, the exam objectives highlight the task areas you can expect to see covered in the exam. Red Hat reserves the right to add, modify, and remove exam objectives. Such changes will be made public in advance.

You should be able to:

* Understand core components of Ansible
  * Inventories
  * Modules
  * Variables
  * Facts
  * Plays
  * Playbooks
  * Configuration files
* Install and configure an Ansible control node

##### Install Ansible with yum
```
sudo yum install ansible
```
##### Install with pip
```
sudo pip install ansible
```

  * Install required packages
  * Create a static host inventory file
  * Create a configuration file
* Configure Ansible managed nodes
  * Create and distribute SSH keys to managed nodes

##### Generate ssh key on master node
```
ansibleuser@host> ssh-keygen -t rsa
```
##### Copy public key to servers
```
ansibleuser@host> ssh-copy-id ansibleuser@server1
ansibleuser@host> ssh-copy-id ansibleuser@server2
ansibleuser@host> ssh-copy-id ansibleuser@server3
```
##### Test ssh setup
```
ansible all -m ping
```

  * Configure privilege escalation on managed nodes
  * Validate a working configuration using ad-hoc Ansible commands
* Create simple shell scripts that run ad hoc Ansible commands
* Use both static and dynamic inventories to define groups of hosts
* Utilize an existing dynamic inventory script
* Create Ansible plays and playbooks
  * Know how to work with commonly used Ansible modules
  * Use variables to retrieve the results of running commands
  * Use conditionals to control play execution
  * Configure error handling
  * Create playbooks to configure systems to a specified state
* Use Ansible modules for system administration tasks that work with:
  * Software packages and repositories
  * Services
  * Firewall rules
  * File systems
  * Storage devices
  * File content
  * Archiving
  * Scheduled tasks
  * Security
  * Users and groups
* Create and use templates to create customized configuration files
* Work with Ansible variables and facts
* Create and work with roles

##### Create an skeleton role with ansible-galaxy
```
ansible-galaxy init myrole
```

* Download roles from an Ansible Galaxy and use them

[Ansible Galaxy](https://galaxy.ansible.com/docs/)

##### Install a role from Galaxy
```
ansible-galaxy install geerlingguy.docker
```
##### Install a role from github
```
ansible-galaxy install ansible-galaxy install https://github.com/rhysmeister/role/role.tar.gz
```
##### Install multiple roles from a files
```
ansible-galaxy install -r requirements.yml
```

##### Example of a requirements.yml file
```
# from galaxy
- src: yatesr.timezone

# from GitHub
- src: https://github.com/bennojoy/nginx

# from GitHub, overriding the name and specifying a specific tag
- src: https://github.com/bennojoy/nginx
  version: master
  name: nginx_role

# from a webserver, where the role is packaged in a tar.gz
- src: https://some.webserver.example.com/files/master.tar.gz
  name: http-role

# from Bitbucket
- src: git+http://bitbucket.org/willthames/git-ansible-galaxy
  version: v1.4

# from Bitbucket, alternative syntax and caveats
- src: http://bitbucket.org/willthames/hg-ansible-galaxy
  scm: hg

# from GitLab or other git-based scm
- src: git@gitlab.company.com:mygroup/ansible-base.git
  scm: git
  version: "0.1"  # quoted, so YAML doesn't parse this as a floating-point value
  ```

* Manage parallelism
* Use Ansible Vault in playbooks to protect sensitive data

[Ansible Vault Documentation](https://docs.ansible.com/ansible/2.4/vault.html)

##### Create a file with vault
```
ansible-vault create variables.yml
```
##### Edit an encrypted file with vault
```
ansible-vault edit variables.yml
```
##### Change the password used to encrypt a file
```
ansible-vault rekey variables.yml
```
##### Encrypting existing files
```
ansible-vault encrypt variables.yml
```
##### Decrypting files
```
ansible-vault decrypt variables.yml variables2.yml
```
##### Use encrypt_string to create encrypted variables to embed in yaml
```
ansible-vault encrypt_string --vault-id a_password_file 'foobar' --name 'the_secret'
```

* Use provided documentation to look up specific information about Ansible modules and commands

##### Use ansible-doc to lookup module documentation
```
ansible-doc lineinfile
```
##### List available modules
```
ansible-doc --list
```
##### Show a summary of the module options
```
ansible-doc -s firewalld
```

As with all Red Hat performance-based exams, configurations must persist after reboot without intervention.
