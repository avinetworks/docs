#########
Playbooks
#########

Ansible's configuration language is put into a Playbook. Playbooks have many components including hosts, variables, tasks, etc. We will cover all of these components and more throughout this section. Another way to look at playbooks is as a instruction for Ansible to orchestrate, provision, deploy, and configure your environment.

Playbooks can contain a list one or multiple plays. Each play can target the same or different systems, but plays are usually targetted to a specific group of systems.

Here is an example playbook containing a single play.

.. code-block:: yaml

  ---
  - hosts: all
    vars:
      controller_version: latest
    roles:
      - role: avinetworks.docker
      - role: avinetworks.avicontroller
        con_version: "{{ controller_version }}"

As you can see it's also in YAML format which we've covered earlier.

*******************
Executing Playbooks
*******************

To execute a playbook, you can simply follow this format:

.. code-block:: bash

  ansible-playbook playbook.yml

There are many options that can be used alongside the ansible-playbook command. To view these please use ``ansible-playbook --help``.

Checking Syntax
===============

The ``ansible-playbook`` command can also be used to validate the syntax of your playbook without executing it against the remote hosts. This will help prevent errors from causing mid play crashes and other issues. To do this use the following command.

.. code-block:: bash

  ansible-playbook playbook.yml --syntax-check

This is extremely useful as a way to lint test your playbooks.
