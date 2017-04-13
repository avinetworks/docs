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
