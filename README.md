Logging role
============

This role facilitates the configuration of global logging attributes, and it supports the configuration of logging servers. This role is abstracted for dellos6, dellos9, and dellos10.

The Logging role requires an SSH connection for connectivity to a Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the *provider* dictionary.

Installation
------------

    ansible-galaxy install Dell-Networking.dellos-logging

Role variables
--------------

- Role is abstracted using the *ansible_net_os_name* variable that can take dellos9, dellos6, and dellos10 values
- If the *dellos_cfg_generate* variable is set to true, it generates the role configuration commands in a file
- Any role variable with a corresponding state variable set to absent negates the configuration of that variable
- Setting an empty value for any variable negates the corresponding configuration
-  Variables and values are case-sensitive

**dellos_logging keys**

| Key        | Type                      | Description                                             | Support               |
|------------|---------------------------|---------------------------------------------------------|-----------------------|
| ``logging`` | list | Configures the logging server (see ``logging.*``) | dellos6, dellos9, dellos10 |
| ``logging.ip`` | string (required)         | Configures the IPv4 address for the logging server (A.B.C.D format) | dellos6, dellos9, dellos10 |
| ``logging.secure_port`` | integer | Specifies log messages over the TLS port | dellos9 |
| ``logging.tcp_port`` | integer | Specifies log messages over the TCP port if *secure_port* is not defined | dellos9 |
| ``logging.udp_port`` | integer | Specifies log messages over the UDP port if both TCP and the secure port key are not defined | dellos9 |
| ``logging.vrf`` | dict | Specifies a VRF instance to be used to reach the host | dellos9 |
| ``logging.vrf.name`` | string | Specifies the VRF name | dellos9 |
| ``logging.vrf.secure_port`` | integer | Specifies log messages over the TLS port | dellos9 |
| ``logging.vrf.tcp_port`` | integer | Specifies log messages over the TCP port if *secure_port key* is not defined | dellos9 |
| ``logging.vrf.udp_port`` | integer | Specifies log messages over the UDP port if both TCP and *secure_port_key* is not defined | dellos9 |
| ``logging.vrf.state`` | string: absent,present\* | Deletes VRF instance of the logging server if set to absent | dellos9 |
| ``logging.state`` | string: absent,present\*     | Deletes the logging server if set to absent   | dellos6, dellos9, dellos10 |
| ``console`` | dictionary | Configures logging to the console (see ``console.*``) | dellos10  |
| ``console.enable`` | boolean | Enables/disables logging to the console | dellos10 |
| ``console.severity`` | string | Configures the minimum severity level for logging to the console | dellos10 |
| ``log_file`` | dictionary | Configures logging to a log file (see ``log_file.*``) | dellos10 |
| ``log_file.enable`` | boolean | Enables/disables logging to a log file | dellos10 |
| ``log_file.severity`` | string | Configures the minimum severity level for logging to a log file | dellos10 |
| ``buffer`` | integer | Specifies the buffered logging severity level (0 to 7) | dellos9 |
| ``console_level`` | integer | Configures the console logging level (0 to 7) | dellos9  |
| ``trap_level`` | integer | Configures the syslog server severity level (0 to 7) | dellos9|
| ``syslog_version`` | integer | Configures the syslog version (0/1) | dellos9 |
| ``monitor`` | integer | Configures the terminal line logging level (0 to 7) | dellos9|
| ``history`` | integer | Configures the syslog history table (0 to 7) | dellos9 |
| ``history_size`` | integer | Specifies the history table size | dellos9  |
| ``on`` | boolean |  Enables logging to all supported destinations if set to true | dellos9 |
| ``extended`` | boolean | Enables extended logging if set to true | dellos9  |
| ``coredump`` | dict | Configures coredump logging | dellos9  |
| ``coredump.server`` | dict | Specifies all server details | dellos9 |
| ``coredump.server.server_ip`` | string (required) | Specifies the IPv4/IPv6 address of the logging server | dellos9 |
| ``coredump.server.username`` | string | Specifies the username to be configured | dellos9 |
| ``coredump.server.password`` | string | Specifies the password to be configured | dellos9 |
| ``coredump.server.state`` | string: present,absent\* | Deletes the coredump server if set to absent | dellos9 |
| ``coredump.stackunit`` |dict | Specifies details for enabling a coredump on the stack-unit | dellos9 |
| ``coredump.stackunit.all`` | boolean | Enables a coredump on all stack-units  | dellos9 |
| ``coredump.stackunit.unit_num`` | integer | Specifies the stack-unit number (0 to 5) | dellos9 |
| ``coredump.stackunit.state`` | string: present,absent\*| Deletes the stack-unit coredump if set to absent | dellos9 |

> **NOTE**: Asterisk (_*_) denotes the default value if none is specified. 

Connection variables
--------------------

Ansible Dell EMC Networking roles require connection information to establish communication with the nodes in your inventory. This information can exist in the Ansible *group_vars* or *host_vars* directories, or in the playbook itself.

| Key         | Required | Choices    | Description                                         |
|-------------|----------|------------|-----------------------------------------------------|
| ``host`` | yes      |            | Specifies the hostname or address for connecting to the remote device over the specified transport |
| ``port`` | no       |            | Specifies the port used to build the connection to the remote device; if unspecified, the value defaults to 22 |
| ``username`` | no       |            | Specifies the username that authenticates the CLI login to connect to the remote device; if value is unspecified, the ANSIBLE_NET_USERNAME environment variable value is used |
| ``password`` | no       |            | Specifies the password that authenticates the connection to the remote device; if value is unspecified, the ANSIBLE_NET_PASSWORD environment variable value is used |
| ``authorize`` | no       | yes, no\*   | Instructs the module to enter privileged mode on the remote device before sending any commands; if value is unspecified, the ANSIBLE_NET_AUTHORIZE environment variable value is used, and the device attempts to execute all commands in non-privileged mode . This key is supported only in dellos9 and dellos6. |
| ``auth_pass`` | no       |            | Specifies the password to use if required to enter privileged mode on the remote device; if *authorize* is set to no this key is not applicable; if value is unspecified, the ANSIBLE_NET_AUTH_PASS environment variable value is used . This key is supported only in dellos9 and dellos6. |
| ``provider`` | no       |            | Passes all connection arguments as a dictonary object; all constraints (such as required or choices) must be met either by individual arguments or values in this dictionary |

> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Dependencies
------------

The *dellos-logging* role is built on modules included in the core Ansible code. These modules were added in Ansible version 2.2.0.

Example playbook
----------------

This example uses the *dellos-logging* role to completely set up logging servers. It creates a *hosts* file with the switch details and corresponding variables. The hosts file should define the *ansible_net_os_name* variable with corresponding Dell EMC networking OS name. When *dellos_cfg_generate* is set to true, generates the configuration commands as a .part file in *build_dir* path. By default, the variable is set to false.

**Sample hosts file**
 
    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>

#### Sample host_vars/leaf1

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx 
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
    build_dir: ../temp/dellos9
	  
    dellos_logging:
      logging:
       - ip : 1.1.1.1
         state: present
       - ip: 2.2.2.2
         secure_port: 1025
         tcp_port: 1024
         udp_port: 2000
         state: present
       - ip: 3.3.3.3
         vrf:
           name: test
           secure_port: 1024
           tcp_port: 1025
           udp_port: 2000
           state: present
         secure_port: 1025
         tcp_port: 2000
         udp_port: 1025
         state: present
     buffer: 5
     console_level: 7
     trap_level: 5
     syslog_version: 5
     history: 4
     history_size: 3
     monitor: 5
     on: true
     extended: true
     coredump:
       server:
         server_ip: 2.2.2.2
         username: u1
         password: pwd
         state: present
       stackunit:
          all: true
          unit_num: 5
          state: present

**Simple playbook to setup logging - leaf.yaml**

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-logging

**Run**

    ansible-playbook -i hosts leaf.yaml

(c) 2017 Dell EMC
