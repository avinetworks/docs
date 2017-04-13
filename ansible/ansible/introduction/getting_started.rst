###############
Getting Started
###############

.. contents::
  :local:
  
**********************
How does Ansible Work?
**********************

Ansible uses OpenSSH by default to connect and execute commands on remote hosts. This also allows the user to take advantage of ControlPersist, Kerberos, and the options avaialble in the users SSH config (~/.ssh/config). If OpenSSH is too old on the host, and it requires features in a newer version, Ansible will attempt to use paramiko to execute. If you want to use Kerberos in this situation, please upgrade your OpenSSH version.

By default, Ansible will attempt to use your SSH key to authenticate the current user to the remote server. To prevent this and customize these options please review the following:

``--ask-pass``
  option to allow us to supply a password for login to the remote host via prompt.

``--ask-become-pass``
  option to allow us to supply a password for sudo/become login to the remote host via prompt

``--user <myuser>``
  option to allow us to supply a user for login to the remote host

********************
Testing Connectivity
********************

When using Ansible, we rely on connectivity to the remote host via ssh. To test if we can connect we can use

.. code-block:: shell

  ansible 10.20.10.200 -m ping -u root --ask-pass

This command will connect over SSH to the remote host and verify connectivity. We will explain later more about this type of Ansible command.


*****************
Host Key Checking
*****************

Host checking is used to verify the known host key against the host key on the remote server. Host keys are often times located in your ``known_hosts`` file. This file is usually located at ``~/.ssh/known_hosts``. If this file doesn't have a host key for the host it will ask if you trust it. If not then it proceeds. However, if the host is in there, but the key doesn't match, it will return an error. When using automation sometimes this is common, and not an issue. In Ansible we have a way to disable errors on host key checking. You can edit ``/etc/ansible/ansible.cfg`` or ``~/.ansible.cfg`` and set ``host_key_checking = False``. For example:

.. code-block:: shell

  [defaults]
  host_key_checking = False

You can also use an environment variable, however I do discourage this practice unless required.

.. code-block:: shell

  export ANSIBLE_HOST_KEY_CHECKING=False
