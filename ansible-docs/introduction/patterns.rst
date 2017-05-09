########
Patterns
########

.. contents::
  :local:

When running Ad-Hoc commands Ansible can allso accept patterns, for example

.. code-block:: shell

  ansible *.example.com -m ping

This command would ping all hosts in your inventory file that contains .example.com at the end of the host or alias.

If you wanted to use all hosts you can use:

.. code-block:: shell

  ansible all -m ping

You can allso use:

.. code-block:: shell

  ansible * -m ping

For more information please visit http://docs.ansible.com/ansible/intro_patterns.html
