Tomcat
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-tomcat)

Provides Apache Tomcat 7, 8 (default) or 9 for your system.

Requirements
------------

Downloaded releases of tomcat in the `files` directory. Hint:
```
cd files
wget http://www-eu.apache.org/dist/tomcat/tomcat-7/v7.0.82/bin/apache-tomcat-7.0.82.tar.gz
wget http://www-eu.apache.org/dist/tomcat/tomcat-8/v8.5.24/bin/apache-tomcat-8.5.24.tar.gz
wget http://www-eu.apache.org/dist/tomcat/tomcat-9/v9.0.2/bin/apache-tomcat-9.0.2.tar.gz
```

Also place all war files into the `files` directory.

Role Variables
--------------

tomcat_layout:
  - name: tomcat # Any name you'd like to identify the application with.
    directory: /opt # Set a unique directory for each installation.
    version: 7 # Either 7, 8 or 9.
    user: tomcat
    group: tomcat
    xms: 512M
    xmx: 1024M
    non_ssl_connector_port: 8080
    ssl_connector_port: 8443
    shutdown_port: 8005
    ajp_port: 8009

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
