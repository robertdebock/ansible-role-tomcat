tomcat
=========

<img src="https://docs.ansible.com/ansible-tower/3.2.4/html_ja/installandreference/_static/images/logo_invert.png" width="10%" height="10%" alt="Ansible logo" align="right"/>
<a href="https://travis-ci.org/robertdebock/ansible-role-tomcat"><img src="https://travis-ci.org/robertdebock/ansible-role-tomcat.svg?branch=master" alt="Build status" align="left"/></a>

Install and configure tomcat on your system.

<img src="https://img.shields.io/ansible/role/d/22945"/>
<img src="https://img.shields.io/ansible/quality/22945"/>

Example Playbook
----------------

This example is taken from `molecule/resources/playbook.yml`:
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: yes

  vars:
    tomcat_instances:
      - name: "tomcat"
      - name: "tomcat-version-7"
        version: 7
        shutdown_port: 8007
        non_ssl_connector_port: 8082
        ssl_connector_port: 8445
        ajp_port: 8011
      - name: "tomcat-version-8"
        version: 8
        shutdown_port: 8008
        non_ssl_connector_port: 8083
        ssl_connector_port: 8446
        ajp_port: 8012
      - name: "tomcat-version-9"
        version: 9
        shutdown_port: 8019
        non_ssl_connector_port: 8084
        ssl_connector_port: 8447
        ajp_port: 8013
      - name: "tomcat-specific"
        user: "specificuser"
        group: "specificgroup"
        shutdown_port: 8020
        non_ssl_connector_port: 8085
        ssl_connector_port: 8448
        ajp_port: 8014
        xms: 256M
        xmx: 512M
      - name: "tomcat-with-wars"
        shutdown_port: 8021
        non_ssl_connector_port: 8086
        ssl_connector_port: 8449
        ajp_port: 8015
        wars:
          - url: https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
      - name: "tomcat-java_opts"
        shutdown_port: 8022
        non_ssl_connector_port: 8087
        ssl_connector_port: 8449
        ajp_port: 8016
        java_opts:
          - name: UMASK
            value: "0007"
      - name: "tomcat-with_lib"
        shutdown_port: 8023
        non_ssl_connector_port: 8088
        ssl_connector_port: 8450
        ajp_port: 8017
        libs:
          - url: "https://search.maven.org/remotecontent?filepath=io/prometheus/simpleclient/0.6.0/simpleclient-0.6.0.jar"

  roles:
    - robertdebock.tomcat
```

The machine you are running this on, may need to be prepared.
```yaml
---
- name: Converge
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - robertdebock.bootstrap
    - robertdebock.core_dependencies
    - robertdebock.java
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

Role Variables
--------------

These variables are set in `defaults/main.yml`:
```yaml
---
# defaults file for tomcat

# The explicit version to use when referring to the short name.
tomcat_version7: 7.0.96
tomcat_version8: 8.5.46
tomcat_version9: 9.0.26

# The location where to download Apache Tomcat from.
tomcat_mirror: "https://archive.apache.org"

# Some "sane" defaults.
tomcat_name: tomcat
tomcat_directory: /opt
tomcat_version: 8
tomcat_user: tomcat
tomcat_group: tomcat
tomcat_xms: 512M
tomcat_xmx: 1024M
tomcat_non_ssl_connector_port: 8080
tomcat_ssl_connector_port: 8443
tomcat_shutdown_port: 8005
tomcat_ajp_port: 8009
tomcat_jre_home: /usr

# This role allows multiple installations of Apache Tomcat, each in their own
# location, potentially of different version.
# This is done by defining a "tomcat_instances" where "name:" is a unique
# identifier of an instance.
# The default tomcat_instances is one instance using the defaults described
# in defaults/main.yml.
tomcat_instances:
  - name: "{{ tomcat_name }}"
    version: "{{ tomcat_version }}"
    user: "{{ tomcat_user }}"
    group: "{{ tomcat_group }}"
    xms: "{{ tomcat_xms }}"
    xmx: "{{ tomcat_xmx }}"
    non_ssl_connector_port: "{{ tomcat_non_ssl_connector_port }}"
    ssl_connector_port: "{{ tomcat_ssl_connector_port }}"
    shutdown_port: "{{ tomcat_shutdown_port }}"
    ajp_port: "{{ tomcat_ajp_port }}"
    java_opts:
      - name: JRE_HOME
        value: "{{ tomcat_jre_home }}"

# When downloading wars, should the SSL certificate be valid? (Impossible for
# CentOS 6, so default: no.)
tomcat_validate_certs: no
```

Requirements
------------

- Access to a repository containing packages, likely on the internet.
- A recent version of Ansible. (Tests run on the current, previous and next release of Ansible.)

The following roles can be installed to ensure all requirements are met, using `ansible-galaxy install -r requirements.yml`:

```yaml
---
- robertdebock.bootstrap
- robertdebock.core_dependencies
- robertdebock.java
- robertdebock.service

```

This role uses the following modules:
```yaml
---
- file
- get_url
- group
- import_role
- include
- service
- set_fact
- systemd
- template
- unarchive
- user
```

Context
-------

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/tomcat.png "Dependency")


Compatibility
-------------

This role has been tested against the following distributions and Ansible version:

|distribution|ansible 2.7|ansible 2.8|ansible devel|
|------------|-----------|-----------|-------------|
|alpine-edge*|yes|yes|yes*|
|alpine-latest|yes|yes|yes*|
|archlinux|yes|yes|yes*|
|centos-7|yes|yes|yes*|
|centos-latest|yes|yes|yes*|
|debian-stable|yes|yes|yes*|
|debian-unstable*|yes|yes|yes*|
|fedora-latest|yes|yes|yes*|
|fedora-rawhide*|yes|yes|yes*|
|opensuse-leap|yes|yes|yes*|
|ubuntu-devel*|yes|yes|yes*|
|ubuntu-latest|yes|yes|yes*|
|ubuntu-rolling|yes|yes|yes*|

A single star means the build may fail, it's marked as an experimental build.


Included version(s)
-------------------

This role [refers to a version](https://github.com/robertdebock/ansible-role-tomcat/blob/master/defaults/main.yml) released by Apache Tomcat. Check the released version(s) here:
- [Tomcat version 7](https://tomcat.apache.org/download-70.cgi).
- [Tomcat version 8](https://tomcat.apache.org/download-80.cgi).
- [Tomcat version 9](https://tomcat.apache.org/download-90.cgi).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.

Testing
-------

[Unit tests](https://travis-ci.org/robertdebock/ansible-role-tomcat) are done on every commit and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-tomcat/issues)

To test this role locally please use [Molecule](https://github.com/ansible/molecule):
```
pip install molecule
molecule test
```

To test on Amazon EC2, configure [~/.aws/credentials](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/credentials.html) and set a region using `export AWS_REGION=eu-central-1` before running `molecule test --scenario-name ec2`.

There are many specific scenarios available, please have a look in the `molecule/` directory.

License
-------

Apache-2.0


Author Information
------------------

[Robert de Bock](https://robertdebock.nl/)
