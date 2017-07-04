Logging Role for Dell EMC Networking OS
======================================

This role facilitates the configuration of global logging attributes. It supports the configuration of logging servers. This role is abstracted for dellos6, dellos9, and dellos10.


Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-logging
```

Requirements
------------
This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Role Variables
--------------

Any role variable with a corresponding state variable set to absent negates the configuration of that variable. For variables with no state variable, setting an empty value for the variable negates the corresponding configuration.

The variables and its values are case-sensitive.

``dellos_logging`` contains the following keys:

|        Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| logging | list | Configures the logging server. See the following logging.* keys for each list item. |
|         logging.ip | string (required)         | Configures the IPv4 address for the logging server. The value must be in the form of A.B.C.D                                                                                                 |
| logging.secure_port | integer | Specify log messages over TLS port if this key is defined. This key is not supported in dellos10 and dellos6 devices. |
| logging.tcp_port | integer | Specify log messages over TCP port if secure_port key is not defined.This key is not supported in dellos10 and dellos6 devices. |
| logging.udp_port | integer | Specify log messages over UDP port if both tcp and secure port key is not defined. This key is not supported in dellos10 and dellos6 devices. |
| logging.vrf | dict | Specify vrf instance to be used to reacch the host .This key is not supported in dellos10 and dellos6 devices. |
| logging.vrf.name | string | Specify vrf name |
| logging.vrf.secure_port | integer | Specify log messages over TLS port if this key is defined. |
| logging.vrf.tcp_port | integer | Specify log messages over TCP port if secure_port key is not defined. |
| logging.vrf.udp_port | integer | Specify log messages over UDP port if both tcp and secure port key is not defined. |
| logging.vrf.state | string, choices: absent, present* | If set to absent, deletes vrf instance of the logging server.|
|     logging.state | string, choices: absent, present*     | If set to absent, deletes the logging server.                                                                                                             |
|  buffer | integer | Specified buffered logging severeity level between 0-7. This key is not supported in dellos10 and dellos6 devices.|
| console_level | integer | Configure console logging level between 0-7. This key is not supported in dellos10 and dellos6 devices. |
| trap_level | integer | Configure syslog server severity level between 0-7. This key is not supported in dellos10 and dellos6 devices.|
| syslog_version | integer | Configure syslog version 0/1. This key is not supported in dellos10 and dellos6 devices.|
| monitor | integer | Configure terminal line logging level between 0-7 . This key is not supported in dellos10 and dellos6 devices.|
| history | integer | Configure syslog history table between 0-7. This key is not supported in dellos10 and dellos6 devices.|
| history_size | integer | Specifies history table size. This key is not supported in dellos10 and dellos6 devices.|
| on | boolean |  Enable logging to all supported destinations if set to true. This key is not supported in dellos10 and dellos6 devices. |
| extended | boolean | Enable extended logging if set to true. This key is not supported in dellos10 and dellos6 devices.|
| coredump | dict | Configure coredump logging. This key is not supported in dellos10 and dellos6 devices.|
| coredump.server | dict | Specifies all aerver details. |
| coredump.server.server_ip | string(required) | Specify ipv4/ipv6 address of the logging server. |
| coredump.server.username | string | Specify username to be configured. |
| coredump.server.password | string | Specify password that has to be configured.|
| coredump.server.state | string, choices: present,absent* |If set to absent, deletes the coredump server.|
| coredump.stackunit |dict | Specifies details for enabling coredump on stack-unit. |
| coredump.stackunit.all | boolean | Enable coredump on all stackunits . |
| coredump.stackunit.unit_num | integer |Specify stack-unit number between 0 to 5. |
| coredump.stackunit.state | string, choices: present, absent*|If set to absent, deletes the stackunit coredump. |


```
Note: Asterisk (*) denotes the default value if none is specified. 
```

Connection Variables
--------------------

Ansible Dell EMC Networking roles require the following connection information to establish 
communication with the nodes in your inventory. This information can exist in
the Ansible group_vars or host_vars directories, or in the playbook itself.



|         Key | Required | Choices    | Description                              |
| ----------: | -------- | ---------- | ---------------------------------------- |
|        host | yes      |            | Hostname or address for connecting to the remote device over the specified ``transport``. The value of this key is the destination address for the transport. |
|        port | no       |            | Port used to build the connection to the remote device. If the value of this key does not specify the value, the value defaults to 22. |
|    username | no       |            | Configures the username that authenticates the connection to the remote device. The value of this key authenticates the CLI login. If this key does not specify a value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
|    password | no       |            | Specifies the password that authenticates the connection to the remote device. If this key does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
|   authorize | no       | yes, no*   | Instructs the module to enter privileged mode on the remote device before sending any commands. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. If not specified, the device attempts to execute all commands in non-privileged mode.|
|   auth_pass | no       |            | Specifies the password to use if required to enter privileged mode on the remote device. If ``authorize`` is set to no, then this key is not applicable. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
|   transport | yes      | cli*       | Configures the transport connection to use when connecting to the remote device. This key supports connectivity to the device over CLI (SSH).  |
|    provider | no       |            | Convenient method that passes all of the above connection arguments as a dictonary object. All constraints (such as required, choices) must be met either by individual arguments or values in this dictonary. |


```
Note: Asterisk (*) denotes the default value if none is specified.
```

Dependencies
------------

The dellos-logging role is built on modules included in the core Ansible code.
These modules were added in Ansible version 2.2.0.

Example Playbook
----------------

The following example uses the dellos.dellos-logging role to completely set up
logging servers. 
The example creates a ``hosts`` file with the switch details and corresponding 
variables.
It writes a simple playbook that only references the dellos-logging role. 
By including the role, you automatically get access to all of the tasks to configure logging
features. 


Sample ``hosts`` file:
 
    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>

Sample ``host_vars/leaf1``:

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx 
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
      transport: cli
	  
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

Simple playbook to setup logging, ``leaf.yaml``:

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-logging

Then run with:

    ansible-playbook -i hosts leaf.yaml

License
--------

Copyright (c) 2017, Dell Inc. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
