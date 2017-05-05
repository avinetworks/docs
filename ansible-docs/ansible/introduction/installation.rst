############
Installation
############

.. contents::
  :local:

*****************
Chosing a Version
*****************

We recommend using the latest major release available. Our playbooks, roles, and modules have been developed on 2.2, so we would recommend that version or later. However, 2.1 can be used but may have occasional issues on some of our playbooks, roles, and modules.

************
Requirements
************

Ansible can be ran from any machine running Python 2.6 or 2.7. In Ansible 2.2, a preview version of Python 3 support was added.

.. warning:: Some modules and plugins will have additional requirements. Some may be libraries on the local machine, and some on remote machines. Please check the module documentation prior to executing the module.

For further installation requirements please visit Ansible's Installation documentation at http://docs.ansible.com/ansible/intro_installation.html

******************
Installing Ansible
******************

Ansible is available through many package repositories including EPEL, Apt, YUM, Portage, and many more. Here I will cover the most common ways to deploy Ansible to a host.

.. contents::
  :local:

Installing via YUM
==================

RPMs for Ansible are located in the EPEL repository. You will need to enable this repository on your host. To install EPEL on most recent operating systems use the following command.

.. code-block:: bash

  sudo yum install epel-release

This will install the EPEL repository form your base OS repository. If this fails, you can also try and use the following command. Please modify <version> with the correct version of your distribution.

.. code-block:: bash

  sudo rpm -ivh https://dl.fedoraproject.org/pub/epel/epel-release-latest-<version>.noarch.rpm

Once EPEL is installed you will be able to simply run the following command, which will install Ansible from EPEL onto your host.

.. code-block:: bash

  sudo yum install ansible

Installing via Apt
==================

Ansible is not included in any of Ubuntu's repositories, and needs to be installed via PPA. To use PPA you will need to run the following commands. These commands will install the proper package tools to add the PPA repository, and update your cache, and then install the Ansible package.

.. code-block:: bash

  sudo apt-get install software-properties-common
  sudo apt-add-repository ppa:ansible/ansible
  sudo apt-get update
  sudo apt-get install ansible

Installing via Pip
==================

Ansible can also be installed via Pip. Pip is a Python package manager. If it is not already available on your system you may install it with the following command.

.. code-block:: bash

  sudo easy_install pip

Then to install Ansible the following command.

.. code-block:: bash

  sudo pip install ansible

It has been documented that on OS X Mavericks there could be issues with the compiler. To work around the issue use the following command.

.. code-block:: bash

  sudo CFLAGS=-Qunused-arguments CPPFLAGS=-Qunused-arguments pip install ansible

Installing via Brew
====================

Mac OSX has an awesome utility called brew, which is available if Homebrew is installed. If installed, to install ansible becomes one simple command.

.. code-block:: bash

  brew install ansible
