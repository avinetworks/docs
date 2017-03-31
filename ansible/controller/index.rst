############################
Avi Controller Role
############################

To help automate the deployment of Avi Vantage Controller in your environment we've developed an Ansible Role which can deploy in many environemnts. This includes deploying from Docker Hub, Private Docker Repo, Docker compressed images (tgz), as well as some cloud environments including CSP deployment.

************************
Best Practices
************************

There are a few key things we need to keep in mind when dealing with the deployment of Avi with Ansible.

- A change in Ansible role values will result in a change in the Avi deployment. Which will likely result in a controller being taken offline for a bit as it restarts with the new service configuration.
- Always, Always verify your values to desired values before execution. Ansible is powerful, it will do what you tell it. Remember Garbage in, Garbage out or GIGO (bad data in, results in bad data out).
- Automation makes it less likely to make a mistake, but automation also allows you scale the mistake very rapidly. Always test, or have someone else verify your code prior to execution.


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


************************
Deploying Avi Controller
************************

.. toctree::
    :maxdepth: 2

    baremetal/index
    csp/index
    openshift/index
