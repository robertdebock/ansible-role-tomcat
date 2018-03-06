Tomcat
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-tomcat)

Provides Apache Tomcat 7, 8 (default) or 9 for your system.

Requirements
------------

These requirements can help you prepare your system for this role:
- robertdebock.bootstrap
- robertdebock.java
- robertdebock.haveged

Role Variables
--------------

You can install multiple instances and multiple versions. This configuration is defined in the variable "tomcat_layout". If you do not use the tomcat_layout, defaults will be used. See defaults/main.yml for the default values.

This is the default tomcat_layout:

```
tomcat_layout:
  - name: tomcat
    directory: /opt
    version: 8.5
    user: tomcat
    group: tomcat
    xms: 512M
    xmx: 1024M
    non_ssl_connector_port: 8080
    ssl_connector_port: 8443
    shutdown_port: 8005
    ajp_port: 8009
```

See "Example Playbooks" for futher details.

Dependencies
------------

These loose dependencies are available.

- robertdebock.bootstrap
- robertdebock.java
- robertdebock.haveged

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Example Playbook
----------------

The simplest form:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.java
    - role: robertdebock.haveged
    - role: robertdebock.tomcat
```

And here is a heavily customized installation:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.java
    - role: robertdebock.haveged
    - role: robertdebock.tomcat
      tomcat_layout:
        - name: appone
          directory: /opt/appone
          version: 7
          user: appone
          group: appone
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8080
          ssl_connector_port: 8443
          shutdown_port: 8005
          ajp_port: 8009
          wars:
            - url: https://tomcat.apache.org/tomcat-6.0-doc/appdev/sample/sample.war
        - name: apptwo
          directory: /opt/apptwo
          version: 8
          user: tomcat
          group: tomcat
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8081
          ssl_connector_port: 8444
          shutdown_port: 8006
          ajp_port: 8010
        - name: appthree
          directory: /opt/appthree
          version: 9
          user: appthree
          group: appthree
          xms: 512M
          xmx: 1024M
          non_ssl_connector_port: 8082
          ssl_connector_port: 8445
          shutdown_port: 8007
          ajp_port: 8011
```

Install this role using `galaxy install robertdebock.tomcat`.

License
-------

Apache License, Version 2.0

Author Information
------------------

Robert de Bock <robert@meinit.nl>
