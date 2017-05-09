###################
Configuration File
###################

Ansible provides a configuration file that allows you to customize the way Ansible runs. Ansible provides a number of ways to provide the configuration file. They are also processed in the following order.

- ``ANSIBLE_CONFIG`` environment variable
- ``ansible.cfg`` located in the current working directory
- ``.ansible.cfg`` located in the users home directory
- ``/etc/ansible/ansible.cfg`` the main ansible configuration file

The configuration file, like the inventory file are in INI format. For most people the default file that was provided on installation should be enough. However, if you need to customize it please use http://docs.ansible.com/ansible/intro_configuration.html as a reference. There are many values that can be used to control Ansible.
