#########
Inventory
#########

.. contents::
  :local:

******************
Inventory File
******************

Ansible uses an inventory file to provide a list of hosts to Ansible. These hosts can be seperated into groups, or individual in the inventory file. The default inventory file is located at ``/etc/ansible/hosts`` but you can supply your own and point to it using the ``-i <inventory_file>`` option when running an Ansible playbook or command.
