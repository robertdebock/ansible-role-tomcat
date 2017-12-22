Tomcat
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-tomcat)

Provides Apache Tomcat 7, 8 (default) or 9 for your system.

Requirements
------------

Access to a repository containing packages, likely on the internet.

Role Variables
--------------


- tomcat_version: 7, 8 or 9, default: 8
- tomcat_directory: a path where to unpack, default: /opt
- tomcat_user: the user to run tomcat as, default: tomcat
- tomcat_group: the group to run tomcat as, default: tomcat
- tomcat_xms: 512M
- tomcat_xmx: 1024M
- tomcat_war_url: The URL to a WAR to deploy, optional.
- tomcat_non_ssl_connector_port: The clear text tcp port, default: 8080
- tomcat_ssl_connector_port: The encrypted tcp port, default: 8443
- tomcat_shutdown_port: The shutdown server port, default: 8005
- tomcat_ajp_port: The AJP clear text tcp port: 8009


Dependencies
------------

- robertdebock.java

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Example Playbook
----------------

```
- hosts: servers

  roles:
    - role: robertdebock.tomcat
      tomcat_directory: /usr/local
```

You can also install multiple instances:

```
- hosts: servers

  roles:
    - role: robertdebock.tomcat
      tomcat_version: 7
      tomcat_directory: /opt/myapp1
    - role: robertdebock.tomcat
      tomcat_version: 8
      tomcat_directory: /opt/myapp2
```

To deploy an application (war) into tomcat:

```
- hosts: servers

  roles:
    - role: robertdebock.tomcat
      tomcat_war_url: http://download.rundeck.org/war/rundeck-2.10.2.war
```

Install this role using `galaxy install robertdebock.tomcat`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
