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

##### Relevant links

[Automate ssh-copy-id with a list of hosts](http://www.youdidwhatwithtsql.com/ssh-copy-id-automation-with-a-list-of-hosts/2412/)
[Automate ssh-copy-id with a numbered hostname format](http://www.youdidwhatwithtsql.com/automate-ssh-copy-id-with-numbered-hosts/2400/)

  * Configure privilege escalation on managed nodes

##### Configure passwordless sudo on Redhat / CentOS

```
sudo useradd ansible
sudo passwd ansible
sudo visudo
```

Add the following line and save the file...

```
ansible ALL=(ALL)       NOPASSWD:ALL
```

##### Configure sudo with password on Redhat CentOS

This method is more secure. Follow the steps above but enter this line when in visudo...

```
ansible ALL=(ALL)       ALL
```

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
    [yum module](https://docs.ansible.com/ansible/latest/modules/yum_module.html)
    [yum_repository module](https://docs.ansible.com/ansible/latest/modules/yum_repository_module.html)
    [package module](https://docs.ansible.com/ansible/latest/modules/package_module.html)
    [pip module](https://docs.ansible.com/ansible/latest/modules/pip_module.html#pip-module)
    [easyinstall module](https://docs.ansible.com/ansible/latest/modules/easy_install_module.html#easy-install-module)

    [List of packaging modules](https://docs.ansible.com/ansible/latest/modules/list_of_packaging_modules.html)
  * Services
    * [service module](https://docs.ansible.com/ansible/latest/modules/service_module.html)
    * [systemd module](https://docs.ansible.com/ansible/latest/modules/systemd_module.html)
    * [service_facts module](https://docs.ansible.com/ansible/latest/modules/service_facts_module.html)
  * Firewall rules
    * [firewalld module](https://docs.ansible.com/ansible/latest/modules/firewalld_module.html)
    * [ufw module](https://docs.ansible.com/ansible/latest/modules/ufw_module.html)
    * [iptables module](https://docs.ansible.com/ansible/latest/modules/iptables_module.html)
  * File systems
    * [List of file modules](https://docs.ansible.com/ansible/latest/modules/list_of_files_modules.html)
  * Storage devices
  * File content
    [List of file modules](https://docs.ansible.com/ansible/latest/modules/list_of_files_modules.html)
  * Archiving
    * [archive module](https://docs.ansible.com/ansible/latest/modules/archive_module.html)
    * [unarchive module](https://docs.ansible.com/ansible/latest/modules/unarchive_module.html)
  * Scheduled tasks
    * [cron module](https://docs.ansible.com/ansible/latest/modules/cron_module.html)
    * [at module](https://docs.ansible.com/ansible/latest/modules/at_module.html)
  * Security
  * Users and groups
    * [user module](https://docs.ansible.com/ansible/latest/modules/user_module.html)
    * [group module](https://docs.ansible.com/ansible/latest/modules/group_module.html)
* Create and use templates to create customized configuration files
  * [template module](https://docs.ansible.com/ansible/latest/modules/template_module.html)
  * [templating jinja2](https://docs.ansible.com/ansible/latest/user_guide/playbooks_templating.html)

##### Generate a /etc/hosts file with a Jinja template

The following template....

```
# {{ ansible_managed }}
127.0.0.1   localhost
::1         localhost ip6-localhost ip6-loopback

# The following lines are desirable for IPv6 capable hosts.
fe00::0     ip6-localnet
ff00::0     ip6-mcastprefix
ff02::1     ip6-allnodes
ff02::2     ip6-allrouters

# Network nodes as generated through Ansible.
{% for host in play_hosts %}
{{ hostvars[host]['ansible_eth1']['ipv4']['address'] }}  {{ host }}
{% endfor %}
```

Will produce something like this...

```
# Ansible managed
127.0.0.1   localhost
::1         localhost ip6-localhost ip6-loopback

# The following lines are desirable for IPv6 capable hosts.
fe00::0     ip6-localnet
ff00::0     ip6-mcastprefix
ff02::1     ip6-allnodes
ff02::2     ip6-allrouters

# Network nodes as generated through Ansible.
192.168.44.101  cnode1
192.168.44.102  cnode2
```

* Work with Ansible variables and facts
  * [setup module](https://docs.ansible.com/ansible/latest/modules/setup_module.html)

##### List all facts on the localhost
```
ansible localhost -m setup
```

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

See --forks option https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html#cmdoption-ansible-playbook-f

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

##### Additional Tips

1. If selinux is enabled install libselinux-python before using file/copy/template modules.
