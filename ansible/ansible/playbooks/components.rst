###########
Components
###########

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


*****
Roles
*****

Roles are a component of Ansible that allow you to reuse tasks, and other components by putting them in a role. Which can be distributed to other people via Ansible Galaxy, or shared internally to allow reusing sets of tasks, vars, etc, to deploy your applications.

An example role can look like this:

.. code-block:: yaml

  ---
  - hosts: controllers
    roles:
      - role: avinetworks.avicontroller
        con_controller_ip: 10.10.27.101
        con_cores: 4
        con_memory_gb: 12

To explain this playbook we will show we have a role: ``avinetworks.avicontroller``, which has variables ``con_controller_ip`` and ``con_cores``, and ``con_memory_gb`` specified. There are many others possible, but since we are just evaluating the example we will use this. The variables are then passed into the roll to replace any defaults, or simply provide variables that require values. These are referred by roles as "Role Variables", and lists of possible options are usually in the documentation of the role README for example: https://galaxy.ansible.com/avinetworks/avicontroller/ click on the "README" tab of the Galaxy Role.

*******
Include
*******

Play Include
============
There are two types of includes in Ansible. There are Play includes, and Task includes. Play includes will include other plays in your playbook. For example if we have a playbook ``playbook1.yml`` and we want to include that playbook in another playbook, such as ``master_play.yml``, ``master_play.yml`` would look like this.

.. code-block:: yaml

  ---
  - include: playbook1.yml

  - name: Master play playbook
    hosts: all
    tasks:
      - debug: mg="Extra Task"

This playbook will execute everything in ``playbook1.yml`` and then will continue with the debug task in the next play in the ``master_play.yml``.

Task Include
============

The second type of include is Task include. Task includes are used to include other files with tasks in them, and can help break one giant set of tasks into others, as well as control when the other tasks are ran, such as a group of tasks you only want ran when a specific condition is met.

For example, here is Ubuntu.yml, this file has a few tasks specific to Ubuntu distributions. If you notice we don't need ``tasks:`` at the top of the file.


.. code-block:: yaml

  - name: Docker | CE | APT | Add Docker GPG Key
    apt_key:
      id: 0EBFCD88
      url: https://download.docker.com/linux/ubuntu/gpg
      state: present

  - name: Docker | CE | APT | Configure Docker repository
    apt_repository:
      repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} stable"
      state: present

  - name: Docker | CE | APT | Enable Edge repository
    apt_repository:
      repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} edge"
      state: present
    when: docker_channel == "edge"
    notify: Docker | CE | APT | Upgrade to Edge

Because these use APT and the repo is for Ubuntu we only need these to run on Ubuntu. So here's how we would include this in the playbook as a task include.

.. code-block:: yaml

  - name: Master play playbook
    hosts: all
    tasks:
      - name: Docker | CE | APT | Ubuntu
        include: Ubuntu.yml
        when: ansible_distribution == "Ubuntu"

This can make complex task executions much easier and faster as if all the tasks in ``Ubuntu.yml`` don't need to run (system doesn't match as Ubuntu) then it will skip the entire set of tasks in ``Ubuntu.yml``.

You can also reuse includes and change variables in them. For example lets create ``message.yml``:

.. code-block:: yaml

  - name: Give a message
    debug: msg={{ message }}

Let's call this in the master playbook and change the message.

.. code-block:: yaml

  - name: Master play playbook
    hosts: all
    tasks:
      - include: message.yml message="This is my message to you"
      - include: message.yml message="This is my second message to you"


Static and Dynamic Includes
===========================

In the previous examples I covered static includes. But now in Ansible 2.0 and later we have Dynamic includes, which allow us to use variables in our includes. For example:

.. code-block:: yaml

  - name: Master play playbook
    hosts: all
    tasks:
      - include: message.yml message={{ item }}
        with_items:
          - This is my message to you
          - This is my second message to you

We can also use other variables in a dynamic include. Previously we did this:

.. code-block:: yaml

  - name: Master play playbook
    hosts: all
    tasks:
      - name: Docker | CE | APT | Ubuntu
        include: Ubuntu.yml
        when: ansible_distribution == "Ubuntu"

When we wanted the Ubuntu file to run when the OS matched. Now we can take this a step farther. Let's say we have multiple operating systems, Ubuntu, CentOS, RedHat, etc. We can create task files for each of those, then use the following to dynamically select which one we want to run.

.. code-block:: yaml

  - name: Master play playbook
    hosts: all
    tasks:
      - name: Docker | CE | Repo
        include: "{{ ansible_distribution }}.yml"

For more information regarding Dynamic and Static includes please visit: http://docs.ansible.com/ansible/playbooks_roles.html#dynamic-versus-static-includes
