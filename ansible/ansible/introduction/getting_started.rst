##############
Getting Started
##############

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

*****************
Host Key Checking
*****************



********************
Testing Connectivity
********************

When using Ansible, we rely on connectivity to the remote host via ssh. To test if we can connect we can use

.. code-block:: shell

  ansible 10.20.10.200 -m ping -u root --ask-pass

This command will connect over SSH to the remote host.
