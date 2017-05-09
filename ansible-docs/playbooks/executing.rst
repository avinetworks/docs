###################
Executing Playbooks
###################

To execute a playbook, you can simply follow this format:

.. code-block:: bash

  ansible-playbook playbook.yml

There are many options that can be used alongside the ansible-playbook command. To view these please use ``ansible-playbook --help``.

Passing Variables
=================

When executing the playbook command, we can also pass variables via the command line. To do so we will use the following command:

.. code-block:: shell

  ansible-playbook playbook.yml --extra-vars "variable=value variable2=value2"


***************
Checking Syntax
***************

The ``ansible-playbook`` command can also be used to validate the syntax of your playbook without executing it against the remote hosts. This will help prevent errors from causing mid play crashes and other issues. To do this use the following command.

.. code-block:: bash

  ansible-playbook playbook.yml --syntax-check

This is extremely useful as a way to lint test your playbooks.
