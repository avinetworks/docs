######################
Dynamic Inventory
######################

.. contents::
  :local:

Dynamic inventory allows us to with a script dynamically pull in an inventory from an environment. It could query anything you have with an API and provide a list of servers, and groups of servers, it can even group servers by tags as well. For example, if you have a bunch of servers in AWS, using a Dynamic inventory, you can run an Ansible Playbook against servers based on their names, and tags. Helps when you have a dynamic environment itself.

With a dynamic inventory, you can also pull data from cloud providers, LDAP, cobbler, and other software that may be keeping track of your servers. Ansible can support all of these using it's external inventory system.

If you are interested in these you can read more, and view examples on the Ansible documentation website: http://docs.ansible.com/ansible/intro_dynamic_inventory.html
