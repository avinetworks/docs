#########
Playbooks
#########

.. contents::
  :local:
  
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

*****
Hosts
*****

Each play will require a ``hosts`` line. This can be a host pattern, host group, host, and can be separated by colons. The section looks like this. ``all`` can be replaced with something like ``*.example.com`` as well.

.. code-block:: yaml

  - hosts: all

*****
Users
*****

In playbooks we also can specify which user we want to use for the play. To do this we will use the ``remote_user`` value. For example to use the user ``centos`` we would use this example.

.. code-block:: yaml

  - hosts: all
    remote_user: centos

This allows us to modify the remote user instead of using our current logged in user on the control machine (the machine we run Ansible from).

*********************
Priviledge Escalation
*********************

Often times we may try to make a change that would require sudo access. By default this priviledge isn't granted to Ansible. We will need to provide it. To do this we will of course need to provide your user sudo on the remote machine. This will have to be done manually, or you will need to run Ansible with a ``root`` or different account that has the access to modify your sudoers priviledge.

Once the remote machine is configured properly we have a series of directives that we can use.

become
  when set to ``true`` or ``yes`` will escalate the priviledge
become_user
  choose which user will be used to escalate priviledge
become_method
  overrides the default in your ansible configuration file, can use ``sudo``, ``su``, ``pbrun``, ``pfexec``, ``doas``, ``dzdo``, ``ksu``

Below is an example of using priviledge escalation:

.. code-block:: yaml

  - hosts: all
    remote_user: centos
    become: yes

We can also apply this to specific tasks throughout the play too!

.. code-block:: yaml

  - hosts: all
    remote_user: centos
    tasks:
      - service: name=firewalld status=stopped enabled=no
        become: yes

We can also use other methods like ``su`` to escalate priviledge.

.. code-block:: yaml

  - hosts: all
    remote_user: centos
    become: yes
    become_method: su

When using become method and not using a SSH key, you will need to provide the ``ansible-playbook`` command the password for priviledge escalation.  To do this you can use the option ``--ask-become-pass``, followed by the password. If you run the playbook and it hangs it's possibly stuck at the priviledge escalation prompt, you can use `Control-C` to quit and then try again with a valid password.

*****
Tasks
*****

Playbooks also will include a list of tasks. Each are run in the order they are listed. Tasks can also include vars specific to the task itself, as well as specific arguments documented in the module.

Tasks will look like this:

.. code-block:: yaml

  tasks:
    - name: Stop the Firewalld service and disable it from boot
      service: name=firewalld status=stopped enabled=no

We can also specify the task like this:

.. code-block:: yaml

  tasks:
    - name: Stop the Firewalld service and disable it from boot
      service:
        name: firewalld
        status: stopped
        enabled: no

It's really up to you, however the first is usually cleaner on some modules, while the second can be useful for modules with many values. The second will also use YAML for everything, the first will likely need specific json formating for complex values.

If a task fails please keep in mind the playbook will stop. You will need to fix the task, then you will need to rerun the playbook. Because of this idempotency is extremely important. If you do not ensure idempotency of your tasks you will possibly run the same command twice.

When using ``shell`` or ``command`` modules they will run the command again. To prevent this you should use a ``creates`` flag or use ``when`` and have a previous task register if the task needs to run again.

.. note::

  ``command`` and ``shell`` modules are the only modules that do not follow key=value format. They are in the free form format of ``shell: cat myfile`` or ``command: cat myfile``.

You can also ignore errors if your command task results in a ``1`` or if a module fails. To ignore errors simply add ``ignore_errors: True`` to your task.

.. code-block:: yaml

  tasks:
    - name: get contents of myfile
      shell: cat myfile
      ignore_errors: True

You can also use previously defined variables in your tasks.

.. code-block:: yaml

  vars:
    filename: myfile
  tasks:
    - name: get contents of {{ filename }}
      shell: cat {{ filename }}

********
Handlers
********

Ansible also has an event system which allows tasks to trigger actions. To take advantage of this we have "Handlers". Handlers can be called using the ``notify`` option on tasks. A nice benefit to this is when you have multiple files that when edited need to restart a service, will notify the hander task which will signal it to run at the end of a play. If multiple files need to restart the same service, it will only restart the service once at the end of the play (instead of multiple times). An example of this is below:

.. code-block:: yaml

  handlers:
    - name: restart service
      service: name=service state=restarted
  tasks:
    - name: modify config file
      template: src=config.j2 dest=/etc/config.conf
      notify: restart service

This will tell Ansible that at the end of the play it will restart the service.

For more information on handlers please visit: http://docs.ansible.com/ansible/playbooks_intro.html#handlers-running-operations-on-change

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
