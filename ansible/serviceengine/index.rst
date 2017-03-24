############################
Avi SE Role
############################

To help automate the deployment of Avi Vantage Service Engine in your environment we've developed an Ansible Role which can deploy in many environemnts. This includes deploying from Docker Hub, Private Docker Repo, Docker compressed images (tgz), as well as some cloud environments including CSP deployment.

************************
Prerequisites
************************

To get started you will need the following:

- Ansible 2.2 or higher
- Server or Cloud to install Avi Controller on (Baremetal, CSP, etc.)
- Pre-existing Avi Controller

************************
Parameters
************************

Ansible Role can take parameters, these parameters can determine where and how Ansible will execute the installation and configuration of the Avi Vantage Service Engine.

Due to the fast pace of features and parameters that get added we will not include them here directly, but they can be accessed in two places. In our public GitHub repository (https://github.com/avinetworks/ansible-role-avise) or in the Ansible Galaxy (https://galaxy.ansible.com/avinetworks/avise/). The readme includes all possible parameters.


************************
Deploying Avi SE
************************

.. toctree::
    :maxdepth: 2

    baremetal/index
    csp/index
    openshift/index
