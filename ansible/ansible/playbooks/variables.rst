#########
Variables
#########

.. contents::
  :local:

Using Ansible we can also use variables to use the same playbooks, plays, tasks, etc.  We can also create variables by registering the results from previous tasks. Variables can also be used in Conditionals and Loops.

******************
Rules of Variables
******************

Variables must:

- start with a letter
- not include a ``-``, a space, or ``.``
- not be a number

Examples of invalid variables are ``123``, ``variable-name``, ``variable name``, and ``variable.name``.

Valid ones can be ``variable``, ``variable_name``, and ``variable1``

You can also use dictionaries which are supported to map keys to values.

.. code-block:: yaml

  variable:
    key1: value
    key2: value

The previous variable can be referenced by either ``variable['key1']`` or by ``variable.key1`` however you cannot define them in that way, this only works one direction.

Using "dot" notation the following are reserved as they are python methods:

``add``, ``append``, ``as_integer_ratio``, ``bit_length``, ``capitalize``, ``center``, ``clear``, ``conjugate``, ``copy``, ``count``, ``decode``, ``denominator``, ``difference``, ``difference_update``, ``discard``, ``encode``, ``endswith``, ``expandtabs``, ``extend``, ``find``, ``format``, ``fromhex``, ``fromkeys``, ``get``, ``has_key``, ``hex``, ``imag``, ``index``, ``insert``, ``intersection``, ``intersection_update``, ``isalnum``, ``isalpha``, ``isdecimal``, ``isdigit``, ``isdisjoint``, ``is_integer``, ``islower``, ``isnumeric``, ``isspace``, ``issubset``, ``issuperset``, ``istitle``, ``isupper``, ``items``, ``iteritems``, ``iterkeys``, ``itervalues``, ``join``, ``keys``, ``ljust``, ``lower``, ``lstrip``, ``numerator``, ``partition``, ``pop``, ``popitem``, ``real``, ``remove``, ``replace``, ``reverse``, ``rfind``, ``rindex``, ``rjust``, ``rpartition``, ``rsplit``, ``rstrip``, ``setdefault``, ``sort``, ``split``, ``splitlines``, ``startswith``, ``strip``, ``swapcase``, ``symmetric_difference``, ``symmetric_difference_update``, ``title``, ``translate``, ``union``, ``update``, ``upper``, ``values``, ``viewitems``, ``viewkeys``, ``viewvalues``, ``zfill``

*******************
Inventory Variables
*******************

Host vars
=========
To specify variables in your inventory files for a specific host, you would use the following ``key=value`` format.

.. code-block:: ini

  host1 key1=value1 key2=value2

This will provide the host with ``key1`` and ``key2``, which will be used in the playbooks and ad-hoc commands referencing this host.


Group vars
==========
To specify variables in your inventory files you would put the variable on the same line in ``key=value`` format.

To specify group variables in an inventory file:

.. code-block:: ini

  [mygroup]
  host1
  host2

  [mygroup:vars]
  variable1=value1
  variable2=value2
  variable3=value3

This would then provide each of these 3 variables for all hosts in ``mygroup``.

******************
Playbook Variables
******************

In playbooks we can define variables in plays by the following.

.. code-block:: yaml

  - hosts: all
    vars:
      variable1: value1

************************
Included files and roles
************************

We've already covered this previously. To specify a variable to an include:

.. code-block:: yaml

  tasks:
    - include: tasks.yml variable1=value

You can also specify variables this way as well.

.. code-block:: yaml

  tasks:
    - include: tasks.yml
      vars:
        variable1: value

To use the value in the tasks.yml file we will reference the var as ``{{ variable1 }}``.

********************
Registered Variables
********************

An extremely useful feature of Ansible is the ability to register the output of a task into a variable so that it can be referenced later. To view possible output of a task that would be in the registered variable, you can look at the output of ``-v``. What is included in the ``results`` value is what would be contained in the registered variable.

For example:

.. code-block:: yaml

  - hosts: all
    tasks:
      - stat: path=/tmp
        register: tmp_folder_data

      - debug: msg={{ tmp_folder_data }}

This sniplet would look for ``/tmp`` on the remote host, and get the information of that folder as per the ``stat`` module, and then provide us with all the information of that folder by the debug module and printing it to output.

************************
Accessing Variable Data
************************

Sometimes our variables may have more data to them than just a single value. For example the previous example of using ``stat`` module. It returned a bunch of information to us.

.. code-block:: json

  {
      "tmp_data": {
          "changed": false,
          "stat": {
              "atime": 1481748353.0,
              "ctime": 1492640380.9926686,
              "dev": 1,
              "executable": true,
              "exists": true,
              "gid": 0,
              "gr_name": "root",
              "inode": 281474977014021,
              "isblk": false,
              "ischr": false,
              "isdir": true,
              "isfifo": false,
              "isgid": false,
              "islnk": false,
              "isreg": false,
              "issock": false,
              "isuid": false,
              "mode": "1777",
              "mtime": 1492640380.9926686,
              "nlink": 2,
              "path": "/tmp",
              "pw_name": "root",
              "readable": true,
              "rgrp": true,
              "roth": true,
              "rusr": true,
              "size": 0,
              "uid": 0,
              "wgrp": true,
              "woth": true,
              "writeable": true,
              "wusr": true,
              "xgrp": true,
              "xoth": true,
              "xusr": true
          }
      }
  }

To access a specific item for example ``exists``, in this object we can use two types of notation.

.. code-block:: jinja

  {{ tmp_data["stat"]["exists"] }}

.. code-block:: jinja

  {{ tmp_data.stat.exists }}

Both will return ``true`` as the result.

To access the first element of an array we would use ``data[0]``.

*******************
Variable Precedence
*******************

Because of how many possible places we can put a variable, we will need to understand variable precedence. Top of the list is the weakest, bottom is the strongest.

- role defaults
- inventory INI or script group vars
- inventory group_vars/all
- playbook group_vars/all
- inventory group_vars/*
- playbook group_vars/*
- inventory INI or script host vars
- inventory host_vars/*
- playbook host_vars/*
- host facts
- play vars
- play vars_prompt
- play vars_files
- role vars (defined in role/vars/main.yml)
- block vars (only for tasks in block)
- task vars (only for the task)
- role (and include_role) params
- include params
- include_vars
- set_facts / registered vars
- extra vars (always win precedence)

Extra vars are what we specify on the command line as we talked about earlier with ``-e`` or ``--extra-vars``.

There are also 3 types of variable scopes, `Global`, `Play`, and `Host`.

- Global is set via command line, Environment Variable, or using the config file.
- Play is set in the play, using vars entries, include_vars, role defaults, and vars.
- Host is set in the inventory, facts, or registered output from tasks.

*************
Ansible Facts
*************

Ansible by default will gather facts about the remote host. You can see all the facts gathered from a remote host by using the command:

.. code-block:: shell

  ansible hostname -m setup

Of course replace hostname with the name of the host, the host will need to be in your inventory file. Once it runs it will return a JSON object with all the information Ansible knows of the host. It can return interface information, disk information, kernel information, OS information, and much more.

To turn off Ansible Facts on a host you would use the following:

.. code-block:: yaml

  - hosts: all
    gather_facts: no

Setting ``gather_facts`` to ``no`` will disable the gathering of facts from the remote host.

Ansible also has Local Facts, which can be provided by custom facts modules. For more information please visit: http://docs.ansible.com/ansible/playbooks_variables.html#local-facts-facts-d

**********************************
Using Variables in Jinja Templates
**********************************

Ansible uses the Jinja template system to create files and handle variables within playbooks. An example of a a template task and the jinja template would be:

.. code-block:: yaml

  vars:
    port: 9060
    server_ip: 192.168.1.20
  tasks:
    - template: src=server.j2 dest=/etc/app/server.conf mode=0644

.. code-block:: jinja

  port={{ port }}
  serverIP={{ server_ip }}

So the end result of the file located at /etc/app/server.conf would be:

.. code-block:: ini

  port=9060
  serverIP=192.168.1.20

************************
Using Variables in Tasks
************************

Ansible allows us to use Jinja within playbooks as well. Making reusing tasks much easier as well as customizing tasks for a different operating system, or any configuration that may differ from server to server.

For example we can change the variables based on the os distribution. Then use those to define a package name. This allows you to support cases in which Apache on CentOS is ``httpd`` but on Ubuntu is ``apache``. We can load the variables specific to that OS and use those.

Ubuntu.yml

.. code-block:: yaml

  package_name: apache

CentOS.yml

.. code-block:: yaml

  package_name: httpd

Playbook Excerpt

.. code-block:: yaml

  - name: Include OS Specific Variables
    include_vars: "{{ ansible_distribution }}.yml"
  - name: Install Package
    package: name={{ package_name }} state=present

If you didn't notice, when we did the ``include_vars`` the value had ``""`` (double quotes) around it. Any value that starts with a variable will need quotes around it. This is a YAML syntax usage correction. Failure to do this will cause Ansible to hit an error on execution.

****************
Jinja Filters
****************

There are many filters that can be extremely useful in modifying playbooks, values, and even dynamically handling data for variables. We can force things to be uppercase, lowercase, combine items, and much more. Jinja has a list of built-in filters documented here: http://jinja.pocoo.org/docs/2.9/templates/#builtin-filters

We will go over a few of these filters that have been common throughout our experience, and provide you some examples.

Math Operators
==============

Jinja will also let us perform mathmatical actions on values. For example

.. code-block:: jinja

  - hosts: all
    vars:
      some_number: 2
    tasks:
      - debug: msg={{ some_number + 1 }}

The result of this would give us a message with the number ``3``.

\+
  Adds objects together, it's not recommended to use this for strings, for strings use ``~`` which will concatenate strings.

\-
  Will subtract the second number from the first

\/
  Divides two numbers and will return a float

\//
  Divides two numbers and will return a truncated integer, this does not round, it just drops everything after the `.`

\%
  Provides the remainder of an integer division

\*
  Multiplies the left operand with the right. ``{{ 2 * 4 }}`` will return ``8``. ``{{ '#' * 40 }}`` would return 40 ``#`` symbols

\**
  Raises the left operated to the power of the right.

Comparisons
===========

==
  Compares two objects for equality.

!=
  Compares two objects for inequality.

>
  true if the left hand side is greater than the right hand side.

>=
  true if the left hand side is greater or equal to the right hand side.

<
  true if the left hand side is lower than the right hand side.

<=
  true if the left hand side is lower or equal to the right hand side.

These are extremely common in ``when`` portions of tasks.

Logic
=====

and
  Return true if the left and the right operand are true.

or
  Return true if the left or the right operand are true.

not
  negate a statement (see below).

(expr)
  group an expression.

These can be useful when handing ``when`` statements or ``if`` statements in your jinja templates.

Other Operators
===============

The following operators are very useful but don’t fit into any of the other two categories:

in
  Perform a sequence / mapping containment test. Returns true if the left operand is contained in the right. {{ 1 in [1, 2, 3] }} would, for example, return true.

is
  Performs a test.

|
  Applies a filter.

~
  Converts all operands into strings and concatenates them.
  ``{{ "Hello " ~ name ~ "!" }}`` would return (assuming name is set to 'John') ``Hello John!``.

()
  Call a callable: {{ post.render() }}. Inside of the parentheses you can use positional arguments and keyword arguments like in Python:
  ``{{ post.render(user, full=true) }}``.

. / []
  Get an attribute of an object.

ipaddr
======

For instance lets validate the ip address we pass as a variable.

To use ``ipaddr`` we will need to install ``netaddr``.

.. code-block:: shell

  pip install netaddr

.. code-block:: shell

  ansible-playbook playbook.yml --extra-vars "controller_ip=10.23.222.10"

.. code-block:: yaml

  - hosts: any
    roles:
      - role: avinetworks.avicontroller
        con_controller_ip: {{ controller_ip | ipaddr }}

If the supplied ``controller_ip`` isn't a valid IP, the value of ``con_controller_ip`` will be "False" which would result in a failure of the execution.

default
=======

The ``default()`` filter allows us to provide a default to a variable if it's not defined. Preventing an error if a value isn't provided. For example:

.. code-block:: jinja

  {{ my_string|default('You didn't provide a string')}}

Would provide the result ``You didn't provide a string`` if ``my_string`` wasn't defined.

In Ansible Use

.. code-block:: yaml

  tasks:
    - debug: msg={{ my_string | default('You didn't provide a string')}}

When executing this if you don't provide a variable named ``my_string`` then the ``debug`` module will return the message ``You didn't provide a string``. This can be useful when not requiring variables.

join
====

When using the ``join()`` filter we can join items in an array into a string.

.. code-block:: yaml

  vars:
    my_list:
      - item1
      - item2
      - item3
  tasks:
    - debug: msg={{ my_list | join(',')}}

Would create me a comma seperated list of ``my_list`` which would look like ``item1,item2,item3``

**************
Python Methods
**************

In practice we found that Python methods can also be useful when parsing text that is put into a variable, replace text, and many more python methods.

split
=====

For instance, we want to split up a comma seperated list that was provided as a string variable to Ansible.

To do this we can do the following using the ``split`` python method.

.. code-block:: yaml

  tasks:
    - name: build server list
      set_fact:
        servers: "{{servers|default([]) + [{'ip': {'addr': item, 'type': 'V4'}}] }}"
      with_items: "{{pool_servers.split(',')}}"

Using that task we are able to take in a comma seperated list of server ip addresses as ``pool_servers`` and iterate through those and append those to the servers variable.
