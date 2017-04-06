#######
Ansible
#######

Ansible is an IT automation tool based on Python. It can managae network devices, configure systems, orchestrate cloud environments and deploy software.

It's goal is to create a secure way to manage infrastructure (using OpenSSH) and allow easy auditing by even those not familiar with the product. There's no complicated syntax to learn, or any complicated languages. It's designed for developers, system adminstrators, network engineers, security engineers, and release engineers.

Ansible is an agentless automation tool. Which means there is no agent that is required to be installed on the remote host. It just requires a "control" machine which executes Ansible playbooks against remote servers. Of course the control host will require access to the remote servers.

Avi Networks recognizes how automation using Ansible is drastically changing the way network and operations teams work and have decided to support the project by providing modules, and roles that allow automation in Ansible to be used against Avi Networks software.

.. toctree::
    :maxdepth: 2

    installation
    understanding_yaml
    best_practices
