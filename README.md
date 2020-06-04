# [tomcat](#tomcat)

Install and configure tomcat on your system.

|Travis|GitHub|Quality|Downloads|Version|
|------|------|-------|---------|-------|
|[![travis](https://travis-ci.com/robertdebock/ansible-role-tomcat.svg?branch=master)](https://travis-ci.com/robertdebock/ansible-role-tomcat)|[![github](https://github.com/robertdebock/ansible-role-tomcat/workflows/Ansible%20Molecule/badge.svg)](https://github.com/robertdebock/ansible-role-tomcat/actions)|[![quality](https://img.shields.io/ansible/quality/22945)](https://galaxy.ansible.com/robertdebock/tomcat)|[![downloads](https://img.shields.io/ansible/role/d/22945)](https://galaxy.ansible.com/robertdebock/tomcat)|[![Version](https://img.shields.io/github/release/robertdebock/ansible-role-tomcat.svg)](https://github.com/robertdebock/ansible-role-tomcat/releases/)|

## [Example Playbook](#example-playbook)

This example is taken from `molecule/resources/converge.yml` and is tested on each push, pull request and release.
```yaml
---
- name: converge
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
          - url: "https://github.com/aeimer/java-example-helloworld-war/raw/master/dist/helloworld.war"
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
    - role: robertdebock.tomcat
```

The machine may need to be prepared using `molecule/resources/prepare.yml`:
```yaml
---
- name: prepare
  hosts: all
  become: yes
  gather_facts: no

  roles:
    - role: robertdebock.bootstrap
    - role: robertdebock.core_dependencies
    - role: robertdebock.java
```

For verification `molecule/resources/verify.yml` run after the role has been applied.
```yaml
---
- name: Verify
  hosts: all
  become: yes
  gather_facts: yes

  tasks:
    - name: check if connection still works
      ping:
```

Also see a [full explanation and example](https://robertdebock.nl/how-to-use-these-roles.html) on how to use these roles.

## [Role Variables](#role-variables)

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

## [Requirements](#requirements)

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

## [Context](#context)

This role is a part of many compatible roles. Have a look at [the documentation of these roles](https://robertdebock.nl/) for further information.

Here is an overview of related roles:
![dependencies](https://raw.githubusercontent.com/robertdebock/drawings/artifacts/tomcat.png "Dependency")

## [Compatibility](#compatibility)

This role has been tested on these [container images](https://hub.docker.com/u/robertdebock):

|container|tags|
|---------|----|
|amazon|2018.03|
|fedora|31, 32|
|ubuntu|bionic|

The minimum version of Ansible required is 2.8 but tests have been done to:

- The previous version, on version lower.
- The current version.
- The development version.

## [Exceptions](#exceptions)

Some variarations of the build matrix do not work. These are the variations and reasons why the build won't work:

| variation                 | reason                 |
|---------------------------|------------------------|
| amazonlinux:1 | Not idempotent. |
| alpine | Restart fails. |

## [Included version(s)](#included-versions)

This role [refers to a version](https://github.com/robertdebock/ansible-role-tomcat/blob/master/defaults/main.yml) released by Apache Tomcat. Check the released version(s) here:
- [Tomcat version 7](https://tomcat.apache.org/download-70.cgi).
- [Tomcat version 8](https://tomcat.apache.org/download-80.cgi).
- [Tomcat version 9](https://tomcat.apache.org/download-90.cgi).

This version reference means a role may get outdated. Monthly tests occur to see if [bit-rot](https://en.wikipedia.org/wiki/Software_rot) occured. If you however find a problem, please create an issue, I'll get on it as soon as possible.
## [Testing](#testing)

[Unit tests](https://travis-ci.com/robertdebock/ansible-role-tomcat) are done on every commit, pull request, release and periodically.

If you find issues, please register them in [GitHub](https://github.com/robertdebock/ansible-role-tomcat/issues)

Testing is done using [Tox](https://tox.readthedocs.io/en/latest/) and [Molecule](https://github.com/ansible/molecule):

[Tox](https://tox.readthedocs.io/en/latest/) tests multiple ansible versions.
[Molecule](https://github.com/ansible/molecule) tests multiple distributions.

To test using the defaults (any installed ansible version, namespace: `robertdebock`, image: `fedora`, tag: `latest`):

```
molecule test

# Or select a specific image:
image=ubuntu molecule test
# Or select a specific image and a specific tag:
image="debian" tag="stable" tox
```

Or you can test multiple versions of Ansible, and select images:
Tox allows multiple versions of Ansible to be tested. To run the default (namespace: `robertdebock`, image: `fedora`, tag: `latest`) tests:

```
tox

# To run CentOS (namespace: `robertdebock`, tag: `latest`)
image="centos" tox
# Or customize more:
image="debian" tag="stable" tox
```

## [License](#license)

Apache-2.0


## [Author Information](#author-information)

[Robert de Bock](https://robertdebock.nl/)

Please consider [sponsoring me](https://github.com/sponsors/robertdebock).
