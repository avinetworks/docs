#########
Inventory
#########

.. contents::
  :local:

******************
Inventory File
******************

Ansible uses an inventory file to provide a list of hosts to Ansible. These hosts can be seperated into groups, or individual in the inventory file. The default inventory file is located at ``/etc/ansible/hosts`` but you can supply your own and point to it using the ``-i <inventory_file>`` option when running an Ansible playbook or command. The inventory file is usually written in INI format.

***********
Hosts
***********

In the inventory file hosts by themselves are the simplest to define. A simple inventory file can look like this:

.. code-block:: ini

  host.example.com

*******
Groups
*******

When using groups in your inventory file you can classify your hosts and decide what groups of hosts you want to control from Ansible. You can also include a single host in multiple groups.  Such as the being a webserver and a database at the same time.

Groups are defined like this:

.. code-block:: ini

  [webserver]
  server1.example.com
  server2.example.com

  [database]
  db1.example.com
  db2.example.com

There is one catch though. When using groups we cannot place single hosts not included with any groups below. So the following will apply:

.. code-block:: ini

  # my single hosts
  host.example.com
  host1.example.com
  host2.example.com

  # Put Groups below
  ##################
  [webserver]
  server1.example.com
  server2.example.com

  [database]
  db1.example.com
  db2.example.com

***********************
Non-standard SSH Ports
***********************

If you are running non-standard SSH ports on your hosts you can also specify the port on the hostname seperated by a colon. For example:

.. code-block:: ini

  host.example.com:2222

**************
Host Patterns
**************

In an inventory file we can also define multiple hosts with one entry. You can specify numeric ranges, as well as alphabetic ranges.

Numeric range example:

.. code-block:: ini

  host[1:10].example.com

Alphabetic range example:

.. code-block:: ini

  server1[a:e].example.com

*************************
Host Entry Components
*************************

::
  <alias> <special variables> <variables>

host alias:
  can be a hostname or just an alias, if using an alias, you will need to specify the special variable ``ansible_host=10.20.20.10``

special variables:
  there are many of these, they include the `Behavioral Inventory Parameters`

variables:
  these are variables you want to define specifically to a host that would be used in your playbooks

****************************
Using Hosts not in Inventory
****************************

When looking to use hosts without an inventory file, we can specify the ``ansible-playbook`` or ``ansible`` ad-hoc command as

.. code-block:: shell

  ansible-playbook -i hostname, playbook.yml

If you noticed we added the ``,`` after the hostname. This specifies to Ansible that we want to use a comma separated list of hosts not related to the hosts file.

*******************************
Behavioral Inventory Parameters
*******************************

These are also known commonly as `Behavioral Inventory Parameters` and can all be found here: http://docs.ansible.com/ansible/intro_inventory.html#list-of-behavioral-inventory-parameters
