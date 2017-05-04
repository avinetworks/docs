#####
Loops
#####

Loops are extremely common in playbooks and tasks. It's a great way to reuse a single task to iterate through a list of values or objects. We can use it to install multiple packages, maybe run a task on multiple objects.

**************
Standard Loops
**************

The most common loop you will likely use is the standard loop. Which we use ``with_items`` provided to the task.

.. code-block:: yaml

  - name: Install packages
    package: name={{ item }} state=present
    with_items:
      - httpd
      - memcached

This will run the task twice once with ``httpd`` as the package name, and once with ``memcached`` as the package name.

We can also iterate through hashes and reference subkeys.

.. code-block:: yaml

  - name: Add user and assign groups
    user:
      name: "{{ item.name }}"
      group: "{{ item.groups }}"
      state: present
    with_items:
      - name: user1
        groups: wheel
      - name: user2
        groups: root

With items can also have a non-yaml hash

.. code-block:: yaml

  - name: Add user and assign groups
    user:
      name: "{{ item.name }}"
      group: "{{ item.groups }}"
      state: present
    with_items:
      - { name: user1, groups: wheel }
      - { name: user2, groups: root }


************
Nested Loops
************

Loops can also be nested:

.. code-block:: yaml

  vars:
    ip_addresses:
      - 10.10.20.21
      - 10.10.20.22
      - 10.10.20.23
    ports: ['80','443']
  tasks:
    - name: give multiple users access to multiple databases
      mysql_user:
        name: "{{ item[0] }}"
        priv: "{{ item[1] }}.*:ALL"
        append_privs: yes
        password: "password"
      with_nested:
        - [ '', 'user2' ]
        - [ 'database1', 'database2', 'database3' ]

You can also use a variable with a list.

.. code-block:: yaml

  vars:
    users:
      - user1
      - user2
    databases:
      - database1
      - database2
      - database3
  tasks:
    - name: give multiple users access to multiple databases
      mysql_user:
        name: "{{ item[0] }}"
        priv: "{{ item[1] }}.*:ALL"
        append_privs: yes
        password: "password"
      with_nested:
        - {{ users }}
        - {{ databases }}

This could help you reuse the same user list and database list for other tasks too.

*******************
Looping with Hashes
*******************

Lets use a hash of a few datapoints.

.. code-block:: yaml

  vars:
    pools:
      pool1:
        servers:
          - { "ip": { "addr": "10.90.130.13", "type": "V4" }}
          - { "ip": { "addr": "10.90.130.15", "type": "V4" }}
      pool2:
        servers:
          - { "ip": { "addr": "10.90.132.13", "type": "V4" }}
          - { "ip": { "addr": "10.90.132.15", "type": "V4" }}
  tasks:
    - name: Create the pools
      avi_pool:
        controller: 10.10.27.90
        username: admin
        password: AviNetworks123!
        tenant: admin
        name: "{{ item.key }}"
        state: present
        enabled: false
        health_monitor_refs:
          - '/api/healthmonitor?name=System-HTTP'
        servers: "{{ item.value.servers }}"
      with_dict:
        "{{ pools }}"

This would iterate through our list of pools, and with their different servers create 2 new pools via the ``avi_pool`` module.

******************
Looping with Files
******************

We can also loop over files for example printing content of a file we have locally. Files are either absolute location or relative.

.. code-block:: yaml

  - tasks:
      - debug: msg={{ item }}
        with_file:
          - file01
          - file02

This will print out both file01 and file02 to our screen.

**********************
Looping with Fileglobs
**********************

Using a fileglob we can output all files in a directory that match a pattern, non-recursively.

.. code-block:: yaml

  - tasks:
      - name: ensure remote directory exists
        file: dest="/opt/mydirectory" state=directory

      - name: deploy my files
        copy: src="{{ item }}" dest="/opt/mydirectory/"
        with_fileglob:
          - "mydirectory/*"

.. note::
  When using a relative path with ``with_fileglob`` in a role, Ansible resolves the path relative to the roles/<rolename>/files directory.

************************
Looping with Subelements
************************

Lets take an example where we need to setup a set of servers before deploying Avi Controllers to them. We need to create users, and allow their SSH keys to be used. We will define a set a vars and then use those to create our users with associated keys.

.. code-block:: yaml

  users:
    - name: user1
      ssh_pub_key:
        - /tmp/user1/ssh/user1.pub
        - /tmp/user1/ssh/otherkey.pub
    - name: user2
      ssh_pub_key:
        - /tmp/user2/ssh/user2.pub

.. code-block:: yaml

  - name: Create User
    user:
      name: "{{ item.name }}"
      state: present
      generate_ssh_key: yes
    with_items:
      - "{{ users }}"

  - name: Set authorized ssh key
    authorized_key:
      user: "{{ item.0.name }}"
      key: "{{ lookup('file', item.1) }}"
    with_subelements:
       - "{{ users }}"
       - ssh_pub_key

*******************************
Registered Variables with Loops
*******************************

When using ``register`` with a loop the results will contain a list of the responses. This can be very useful when registering host creation and getting the servers from the array from the task.

.. code-block:: yaml

  tasks:
    - name: Provision a set of instances
      ec2:
        aws_access_key: "{{ec2_access_key}}"
        aws_secret_key: "{{ec2_secret_key}}"
        key_name: aws_key
        group: server
        instance_type: t2.micro
        image: ami-123456
        wait: true
        exact_count: 1
        count_tag:
          Name: "{{ item }}"
        instance_tags:
          Name: "{{ item }}"
      register: ec2
      with_items:
        - server1
        - server2
        - server3

This would then return an array of results, which each would contain IP addresses and other data from the AWS api call. We can then use the returned data to add servers to a group, create an array to submit to a controller as new pool servers as well.

.. code-block:: yaml

  # Value returned from Ansible

***********************
Looping over Inventory
***********************

If you want to loop over a group of inventory hosts or a subset you can use ``with_items`` and the variable ``play_hosts`` or using the ``group`` variable. Play hosts would use the hosts assigned to the current play in the playbook. Group variable usage is ``groups['groupname']`` and would use all of the hosts in the declared group.

This example would give us a list of all the hosts in the current play.

.. code-block:: yaml

  - debug: msg={{ item }}
    with_items:
      - "{{ play_hosts }}"

This example would give us a list of all the hosts in the group we specified in this case ``servers``.

.. code-block:: yaml

  - debug: msg={{ item }}
    with_items:
      - "{{ groups['servers']}}"
