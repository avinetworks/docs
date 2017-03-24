###################
Ansible Integration
###################

.. toctree::
    :maxdepth: 1

    controller/index
    serviceengine/index
    docker/index
    network_interfaces/index
    modules/index

Ansible is an extremely popular tool among operations and network engineers, and allows them to automate many tasks, which are often repeatable. It can configure systems, network devices, handle orchestration, and launch continuous deployments. It's goal is ease of use, and minimum amount of moving parts. It uses OpenSSH for transport instead of proprietary protocols, and communication. It's agentless, and can be ran from just about any machine (of course with network access). It's designed to be used by all types of users. For Ansible there is no need to manage remote daemons. It's also decentralized and relies on your existing credentials to gain access to the remote hosts.

We've built Ansible Roles that can be used to deploy Ansible in your environment, as well as Ansible Modules that can be used to manage and control your Avi Vantage deployment. Due to our vast API, our Ansible integration can do anything our API permits.
