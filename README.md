Software image upgrade/install role
===================================

This role facilitates upgrades or installation of an OS10 software image. The *dellos-image-upgrade* role requires an SSH connection for connectivity to a Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Installation
------------

    ansible-galaxy install Dell-Networking.dellos-image-upgrade

Role variables
--------------

- Setting an empty value for any variable negates the corresponding configuration
- Variables and values are case-sensitive

**dellos_image_upgrade keys**

| Key        | Type                      | Description                                             | Support               |
|------------|---------------------------|---------------------------------------------------------|-----------------------|
| ``operation_type``   | string: upgrade,install | Displays the type of image operation | dellos10 |
| ``software_image_url`` | string          | Configures the URL path to the image file | dellos10 |
| ``software_version`` | string         | Displays the software version of the image file | dellos10 |
                                                                                                      
Connection variables
--------------------

Ansible Dell EMC Networking roles require connection information to establish communication with the nodes in your inventory. This information can exist in the Ansible *group_vars* or *host_vars* directories or in the playbook itself.

| Key         | Required | Choices    | Description                                         |
|-------------|----------|------------|-----------------------------------------------------|
| ``host`` | yes      |            | Specifies the hostname or address for connecting to the remote device over the specified transport. |
| ``port`` | no       |            | Specifies the port used to build the connection to the remote device; if value is unspecified, the value defaults to 22 |
| ``username`` | no       |            | Specifies the username that authenticates the CLI login for the connection to the remote device; if value is unspecified, the ANSIBLE_NET_USERNAME environment variable value is used |
| ``password`` | no       |            | Specifies the password that authenticates the connection to the remote device; if value is unspecified, the ANSIBLE_NET_PASSWORD environment variable value is used |
| ``authorize`` | no       | yes, no\*   | Instructs the module to enter privileged mode on the remote device before sending any commands; if value is unspecified, the ANSIBLE_NET_AUTHORIZE environment variable value is used, and the device attempts to execute all commands in non-privileged mode |
| ``auth_pass`` | no       |            | Specifies the password to use if required to enter privileged mode on the remote device; if *authorize* is set to no, this key is not applicable; if value is unspecified, the ANSIBLE_NET_AUTH_PASS environment variable value is used |
| ``provider`` | no       |            | Passes all connection arguments as a dictonary object; all constraints (such as required or choices) must be met either by individual arguments or values in this dictionary |

> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Dependencies
------------

The *dellos-image-upgrade* role is built on modules included in the core Ansible code. These modules were added in Ansible version 2.4.0.

Example playbook
----------------

This example uses the *dellos-image-upgrade* role to upgrade/install software image. It creates a *hosts* file with the switch details, corresponding *host_vars* file, and a simple playbook that references the *dellos-image-upgrade* role.

**Sample hosts file**

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos10)>

**Sample host_vars/leaf1**

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
    dellos_image_upgrade:
      operation_type: install
      software_image_url: tftp://10.16.148.8/PKGS_OS10-Enterprise-10.2.9999E.5790-installer-x86_64.bin
      software_version: 10.2.9999E

**Simple playbook to setup system - leaf.yaml**

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-image-upgrade
                
**Run**

    ansible-playbook -i hosts leaf.yaml

(c) 2017 Dell EMC
