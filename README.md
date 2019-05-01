# MOLGENIS Ansible

>note supported MOLGENIS versions: 8.x 

Create your MOLGENIS VM with Ansible version 2.7.x or higher.

Execute for roll out:

```bash
ansible-playbook -i inventory.ini playbook.yml **optional: [ --extra-vars "molgenis_version=x.x.x" ]**
```

You can toggle a number of settings. You can use ```--extra-vars``` flag to set them. Currently we support the flags below: 
* molgenis_version | *specify specific version of MOLGENIS to deploy*

> note: multiple vars can be set space separated. Example: ```--extra-vars "molgenis_version=8.0.3"```.

## Actions after running the playbook
What you need to do after this installation.

- SSH
  - Authorised keys
- SSL
  - Certificates
- Firewall

## SSH-configuration
There are some prerequisites to run this playbook.

- You have to have root-privileges on the target machine
- You have to create a key pair for host key signing and put them in the molgenis_ca folder. You also need to add the public key to the known_hosts file

## Roles:
- preinstall | *initialize configuration (setup repos for instance)*
- httpd | *add a webserver, in this case Apache*
- elasticsearch | *add elasticsearch*
- postgres | *add a database, in this case postgresql*
- minio | *add a minio instance to the system*
- tomcat | *add MOLGNENIS-OPS-TOMCAT, to setup tomcat*
- molgenis | *deploy MOLGENIS artifact on system*
- postinstall | *additional steps to configure the server*

## Role: [ preinstall ] - Initialize configuration
Configures repositories and installs all the packages needed to run the following roles on the system. 

Specific configuration done by the preinstall role is:
* Synchronise NTP

## Role: [ httpd ] - Add httpd
Install the service httpd and enable it on boot time.
* Install certificates on host
  >note: you need to have access to the backup server on the machine you are running Ansible on 
* Configure HTTPD 
  >note: with hostname located in the inventory.ini file

## Role: [ elasticsearch ] - Add ElasticSearch
The installation of elasticsearch consists of 3 major steps:

* Configure Elasticsearch
* Configure system to work with Elasticsearch
* Install Elasticsearch 

### Copy default configuration
* Copy jvm.options
* Copy elasticsearch.yml

### Configure system configuration
* Configure 'pam-limits'
* Configure sysctl to set 'vm.swappenis'

### Install elasticsearch
* Install the Elasticsearch repository 
* Install elasticsearch:5.5.1

## Role: [ postgres ] - Add PostgreSQL
The installation of PostgreSQL consists of 5 major steps:

### Install PostgreSQL
* Create the repository
* Install PostgreSQL RPM

### Copy default configuration
* Install extra packages to use PSQL remote
* Copy default config to OS (pg_hba.conf)
* Start and enable PostgreSQL

### Configure database
* Create 'molgenis' database in PSQL
* Create 'molgenis' user in PSQL
* Create 'molgenis'-scheme in PSQL
* Grant right on 'molgenis'-scheme with user 'molgenis' 

## Role: [ minio ] - Add Minio
There are 2 steps that are performed when running this role.

* Install minio
* Enable minio

## Role: [ tomcat ] - Add Tomcat
There are 2 steps that are performed when running this role.

* Install tomcat
* Start and enable tomcat

## Role: [ molgenis ] - Add MOLGENIS 
Install MOLGENIS on the system

## Role: [ postinstall ] - Post installation configuration
* yum versionlock is performed on MOLGENIS rpm's

