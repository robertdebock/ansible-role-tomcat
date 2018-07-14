Tomcat
=========

[![Build Status](https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.org/robertdebock/ansible-role-tomcat)

Provides Apache Tomcat 7, 8 (default) or 9 for your system.

Context
-------
This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/tomcat.png "Dependency")

Requirements
------------

These requirements can help you prepare your system for this role:
- [robertdebock.bootstrap](https://travis-ci.org/robertdebock/ansible-role-bootstrap)
- [robertdebock.java](https://travis-ci.org/robertdebock/ansible-role-java)
- [robertdebock.haveged](https://travis-ci.org/robertdebock/ansible-role-haveged) (optional, requires robertdebock.epel.)

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

- tomcat_version7: Refer to an upstream version of Tomcat 7.
- tomcat_version8: Refer to an upstream version of Tomcat 8.
- tomcat_version85: Refer to an upstream version of Tomcat 8.5.
- tomcat_version9: Refer to an upstream version of Tomcat 9.
- tomcat_mirror: Where to download from.
- tomcat_name: The default name for the Tomcat instance.
- tomcat_directory: Where to install.
- tomcat_version: What version to install.
- tomcat_user: The user to run under.
- tomcat_group: The group to run under.
- tomcat_xms: Memory size xms.
- tomcat_xmx: Memory size xmx.
- tomcat_non_ssl_connector_port: TCP port.
- tomcat_ssl_connector_port: TCP port.
- tomcat_shutdown_port: TCP port.
- tomcat_ajp_port: TCP port.

# This role allows multiple installations of Apache Tomcat, each in their own
# location, potentially of different version.
# This is done by defining a "tomcat_layout" where "name:" is a unique
# identifier of an instance.
# The default tomcat_layout is one instance using the defaults described
# in defaults/main.yml.
tomcat_layout:
  - name: "{{ tomcat_name }}"
    directory: "{{ tomcat_directory }}"
    version: "{{ tomcat_version }}"
    user: "{{ tomcat_user }}"
    group: "{{ tomcat_group }}"
    xms: "{{ tomcat_xms }}"
    xmx: "{{ tomcat_xmx }}"
    non_ssl_connector_port: "{{ tomcat_non_ssl_connector_port }}"
    ssl_connector_port: "{{ tomcat_ssl_connector_port }}"
    shutdown_port: "{{ tomcat_shutdown_port }}"
    ajp_port: "{{ tomcat_ajp_port }}"

# When downloading wars, should the SSL certificate be valid? (Impossible for
# CentOS 6, so default: no.)
tomcat_validate_certs: no

Dependencies
------------

These loose dependencies are available.

- robertdebock.bootstrap
- robertdebock.epel
- robertdebock.java
- robertdebock.haveged

Download the dependencies by issuing this command:
```
ansible-galaxy install --role-file requirements.yml
```

Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.4|ansible 2.5|ansible 2.6|
|------------|-----------|-----------|-----------|
|alpine-edge|yes|yes|yes|
|alpine-latest|yes|yes|yes|
|archlinux|yes|yes|yes|
|centos-6|yes|yes|yes|
|centos-latest|yes|yes|yes|
|debian-latest|yes|yes|yes|
|debian-stable|yes|yes|yes|
|fedora-latest|yes|yes|yes|
|fedora-rawhide|yes|yes|yes|
|opensuse-leap|yes|yes|yes|
|opensuse-tumbleweed|yes|yes|yes|
|ubuntu-artful|yes|yes|yes|
|ubuntu-latest|yes|yes|yes|

Example Playbook
----------------

The simplest form:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
    - role: robertdebock.java
    - role: robertdebock.haveged
    - role: robertdebock.tomcat
```

And here is a heavily customized installation:
```
- hosts: servers

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.epel
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

Robert de Bock](https://robertdebock.nl/) <robert@meinit.nl>
