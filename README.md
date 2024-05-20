# Ansible role: labocbz.install_maxscale

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: SSL/TLS](https://img.shields.io/badge/Tech-SSL%2FTLS-orange)
![Tag: Mariadb](https://img.shields.io/badge/Tech-Mariadb-orange)
![Tag: MaxScale](https://img.shields.io/badge/Tech-MaxScale-orange)

An ansible role to install MaxScale on your hosts.

This Ansible role is designed to deploy and configure MariaDB MaxScale, a database proxy. It facilitates the setup of MariaDB servers that MaxScale will connect to, manages SSL settings to secure communications, and configures MaxScale's administration interface. Tasks include defining users and passwords, configuring listening ports, managing threads for performance optimization, and setting up SSL certificates to ensure secure connections.

Additionally, the role configures the web interface of MaxScale, with default login credentials set to admin/mariadb. It's worth noting that this role does not currently support the configuration of multiple services or monitors. Overall, the role automates the installation and configuration of MaxScale, providing a centralized and secure management solution for MariaDB database environments.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---

install_maxscale__admin_login: "root"
install_maxscale__admin_password: "PN$^L8zP*wm@3q"

install_maxscale__database_user_login: "maxscale"
install_maxscale__database_user_password: "PN$^L8zPM8d58Z*wm@3q"

install_maxscale__mariadb_servers:
  - { name: "server-1", port: 3306, address: "{{ hostvars['molecule-local-instance-1-install-maxscale']['ansible_default_ipv4']['address'] }}" }
  - { name: "server-2", port: 3306, address: "{{ hostvars['molecule-local-instance-2-install-maxscale']['ansible_default_ipv4']['address'] }}" }
  - { name: "server-3", port: 3306, address: "{{ hostvars['molecule-local-instance-3-install-maxscale']['ansible_default_ipv4']['address'] }}" }

install_maxscale__mariadb_server_ssl: true
install_maxscale__mariadb_server_type: "galeramon" #mariadbmon
install_maxscale__mariadb_server_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
install_maxscale__mariadb_server_ssl_client_auth: true
install_maxscale__mariadb_server_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
install_maxscale__mariadb_server_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"
install_maxscale__mariadb_server_ssl_version: "MAX" # TLSv10, TLSv11, TLSv12, TLSv13
install_maxscale__mariadb_server_ssl_verify_peer_certificate: false
install_maxscale__mariadb_server_ssl_verify_peer_host: false

install_maxscale__listener_server_port: 3307
install_maxscale__listener_server_address: "0.0.0.0"
install_maxscale__listener_server_ssl: true
install_maxscale__listener_server_ssl_client_auth: true
install_maxscale__listener_server_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
install_maxscale__listener_server_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
install_maxscale__listener_server_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"

install_maxscale__threads: "auto"
install_maxscale__threads_max: 256
install_maxscale__admin_enabled: true
install_maxscale__admin_gui: true
install_maxscale__admin_host: "0.0.0.0"
install_maxscale__admin_port: 8989
install_maxscale__admin_secure_gui: false
install_maxscale__admin_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
install_maxscale__admin_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
install_maxscale__admin_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_prepare_host__system_users:
  - login: "mysql"
    group: "mysql"
  - login: "maxscale"
    group: "maxscale"

inv_install_maxscale__admin_login: "root"
inv_install_maxscale__admin_password: "PN$^L8zP*wm@3q"

inv_install_maxscale__database_user_login: "maxscale"
inv_install_maxscale__database_user_password: "PN$^L8zPM8d58Z*wm@3q"

#inv_install_maxscale__mariadb_servers:
#  - { name: "server-1", port: 3306, address: "{{ hostvars['molecule-local-instance-1-install-maxscale']['ansible_default_ipv4']['address'] }}" }
#  - { name: "server-2", port: 3306, address: "{{ hostvars['molecule-local-instance-2-install-maxscale']['ansible_default_ipv4']['address'] }}" }
#  - { name: "server-3", port: 3306, address: "{{ hostvars['molecule-local-instance-3-install-maxscale']['ansible_default_ipv4']['address'] }}" }

inv_install_maxscale__mariadb_server_ssl: true
inv_install_maxscale__mariadb_server_type: "galeramon" #mariadbmon
inv_install_maxscale__mariadb_server_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
inv_install_maxscale__mariadb_server_ssl_client_auth: true
inv_install_maxscale__mariadb_server_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
inv_install_maxscale__mariadb_server_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"
inv_install_maxscale__mariadb_server_ssl_version: "MAX" # TLSv10, TLSv11, TLSv12, TLSv13
inv_install_maxscale__mariadb_server_ssl_verify_peer_certificate: false
inv_install_maxscale__mariadb_server_ssl_verify_peer_host: false

inv_install_maxscale__listener_server_port: 3307
inv_install_maxscale__listener_server_address: "0.0.0.0"
inv_install_maxscale__listener_server_ssl: true
inv_install_maxscale__listener_server_ssl_client_auth: true
inv_install_maxscale__listener_server_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
inv_install_maxscale__listener_server_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
inv_install_maxscale__listener_server_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"

inv_install_maxscale__threads: "auto"
inv_install_maxscale__threads_max: 256
inv_install_maxscale__admin_enabled: true
inv_install_maxscale__admin_gui: true
inv_install_maxscale__admin_host: "0.0.0.0"
inv_install_maxscale__admin_port: 8989
inv_install_maxscale__admin_secure_gui: true
inv_install_maxscale__admin_ssl_ca: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/ca-chain.pem.crt"
inv_install_maxscale__admin_ssl_cert: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.crt"
inv_install_maxscale__admin_ssl_key: "/etc/maxscale/ssl/my-mariadb-cluster.domain.tld/my-mariadb-cluster.domain.tld.pem.key"

# From local / group .yml
---

inv_install_maxscale__mariadb_servers:
  - { name: "server-1", port: 3306, address: "{{ hostvars['molecule-local-instance-1-install-maxscale']['ansible_default_ipv4']['address'] }}" }
  - { name: "server-2", port: 3306, address: "{{ hostvars['molecule-local-instance-2-install-maxscale']['ansible_default_ipv4']['address'] }}" }
  - { name: "server-3", port: 3306, address: "{{ hostvars['molecule-local-instance-3-install-maxscale']['ansible_default_ipv4']['address'] }}" }

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_maxscale"
  tags:
    - "labocbz.install_maxscale"
  vars:
    install_maxscale__admin_login: "{{ inv_install_maxscale__admin_login }}"
    install_maxscale__admin_password: "{{ inv_install_maxscale__admin_password }}"
    install_maxscale__database_user_login: "{{ inv_install_maxscale__database_user_login }}"
    install_maxscale__database_user_password: "{{ inv_install_maxscale__database_user_password }}"
    install_maxscale__mariadb_servers: "{{ inv_install_maxscale__mariadb_servers }}"
    install_maxscale__mariadb_server_ssl: "{{ inv_install_maxscale__mariadb_server_ssl }}"
    install_maxscale__mariadb_server_type: "{{ inv_install_maxscale__mariadb_server_type }}"
    install_maxscale__mariadb_server_ssl_ca: "{{ inv_install_maxscale__mariadb_server_ssl_ca }}"
    install_maxscale__mariadb_server_ssl_client_auth: "{{ inv_install_maxscale__mariadb_server_ssl_client_auth }}"
    install_maxscale__mariadb_server_ssl_cert: "{{ inv_install_maxscale__mariadb_server_ssl_cert }}"
    install_maxscale__mariadb_server_ssl_key: "{{ inv_install_maxscale__mariadb_server_ssl_key }}"
    install_maxscale__mariadb_server_ssl_version: "{{ inv_install_maxscale__mariadb_server_ssl_version }}"
    install_maxscale__mariadb_server_ssl_verify_peer_certificate: "{{ inv_install_maxscale__mariadb_server_ssl_verify_peer_certificate }}"
    install_maxscale__mariadb_server_ssl_verify_peer_host: "{{ inv_install_maxscale__mariadb_server_ssl_verify_peer_host }}"
    install_maxscale__listener_server_port: "{{ inv_install_maxscale__listener_server_port }}"
    install_maxscale__listener_server_address: "{{ inv_install_maxscale__listener_server_address }}"
    install_maxscale__listener_server_ssl: "{{ inv_install_maxscale__listener_server_ssl }}"
    install_maxscale__listener_server_ssl_client_auth: "{{ inv_install_maxscale__listener_server_ssl_client_auth }}"
    install_maxscale__listener_server_ssl_ca: "{{ inv_install_maxscale__listener_server_ssl_ca }}"
    install_maxscale__listener_server_ssl_cert: "{{ inv_install_maxscale__listener_server_ssl_cert }}"
    install_maxscale__listener_server_ssl_key: "{{ inv_install_maxscale__listener_server_ssl_key }}"
    install_maxscale__threads: "{{ inv_install_maxscale__threads }}"
    install_maxscale__admin_enabled: "{{ inv_install_maxscale__admin_enabled }}"
    install_maxscale__admin_gui: "{{ inv_install_maxscale__admin_gui }}"
    install_maxscale__admin_host: "{{ inv_install_maxscale__admin_host }}"
    install_maxscale__admin_port: "{{ inv_install_maxscale__admin_port }}"
    install_maxscale__admin_secure_gui: "{{ inv_install_maxscale__admin_secure_gui }}"
    install_maxscale__admin_ssl_ca: "{{ inv_install_maxscale__admin_ssl_ca }}"
    install_maxscale__admin_ssl_cert: "{{ inv_install_maxscale__admin_ssl_cert }}"
    install_maxscale__admin_ssl_key: "{{ inv_install_maxscale__admin_ssl_key }}"
  ansible.builtin.include_role:
    name: "labocbz.install_maxscale"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-03-03: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2024-04-03: Install done

* Role install and configure MaxScale
* You have to define a server group, but after install, you can just use the GUI to add / remove servers
* mTLS handled
* TLS/SSL handled
* Role create the user, but an Admin/root password is needed to create the account and set permissions
* Added support for Debian 11/12
* Added support for Ubuntu 22
* Added new CI and SonarQube

### 2024-05-19: New CI

* Added Markdown lint to the CICD
* Rework all Docker images
* Change CICD vars convention
* New workers
* Removed all automation based on branch

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [Deploy MaxScale 22.08 with Galera Monitor and Read Connection Router](https://mariadb.com/docs/server/deploy/maxscale-galeramon-readconnroute-mxs22-08/)
* [SETTING UP MARIADB GALERA CLUSTER](https://mariadb.com/resources/blog/getting-started-with-mariadb-galera-and-mariadb-maxscale-on-centos/)
