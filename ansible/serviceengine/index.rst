############################
Avi SE Role
############################

To help automate the deployment of Avi Vantage Service Engine in your environment we've developed an Ansible Role which can deploy in many environemnts. This includes deploying from Docker Hub, Private Docker Repo, Docker compressed images (tgz), as well as some cloud environments including CSP deployment.

.. contents:: Contents
    :local:
    :depth: 3


************************
Prerequisites
************************

To get started you will need the following:

- Ansible 2.2 or higher
- Server or Cloud to install Avi Controller on (Baremetal, CSP, etc.)


************************
Parameters
************************

Ansible Role can take parameters, these parameters can determine where and how Ansible will execute the installation and configuration of the Avi Vantage Controller.

Due to the fast pace of features and parameters that get added we will not include them here directly, but they can be accessed in two places. In our public GitHub repository (https://github.com/avinetworks/ansible-role-avicontroller) or in the Ansible Galaxy (https://galaxy.ansible.com/avinetworks/avicontroller/). The readme includes all possible parameters.

*****************************
Deploying Avi Service Engine
*****************************

Bare-metal Avi Deployment
=========================

.. contents:: Contents
    :local:
    :depth: 1


Requirements
------------

- CentOS/RHEL/OracleLinux 7.x
- Ubuntu 14.04 or higher
- Docker 1.12 or higher


Using Docker Hub
------------------------

**Steps:**


Using a Private Docker Repository
------------------------------------

**Steps:**


Using a Compressed Docker Image
------------------------------------

**Steps:**


Cisco CSP Avi Deployment
===========================

Requirements
---------------
You will need to have available memory and storage for both the image, and the service on your CSP 2100.


Using QCOW image
-----------------
