############################
Ansible Best Practices
############################

.. contents::

There are a few key things we need to keep in mind when dealing with the deployment of Avi with Ansible.

- A change in Ansible role values will result in a change in the Avi deployment. Which will likely result in a controller being taken offline for a bit as it restarts with the new service configuration.
- Always, Always verify your values to desired values before execution. Ansible is powerful, it will do what you tell it. Remember: Garbage in, Garbage out.
- Automation makes it less likely to make a mistake, but automation also allows you scale the mistake very rapidly. Always test, or have someone else verify your code prior to execution.

*********************
Repository Layout
*********************

We recommend following the structure described by Ansible here http://docs.ansible.com/ansible/playbooks_best_practices.html#directory-layout

  .. code-block:: shell

    production                      # production inventory file
    stage                           # stage inventory file

    group-vars/
      group1                        # variables assigned to hosts in group1
      group2                        # variables assigned to hosts in group2
    host_vars/
      hostname1                     # variables assigned to 'hostname1'
      hostname2                     # variables assigned to 'hostname2'

    library/                        # custom modules go here

    avicontroller.yml               # playbook that can deploy your controller or
                                    # build your controller

    avise.yml                       # playbook that can prepare your SE servers
                                    # for the Linux Server Cluster Avi deploy

    create_vip.yml                  # playbook that uses modules to create vip
    ...                             # if you have too many of these,
                                    # think about combining tasks into roles,
                                    # and providing variables to roles

    roles/
      avinetworks.avisdk            # avisdk role downloaded from Galaxy
      avinetworks.avicontroller     # avicontroller role downloaded from Galaxy
      avinetworks.docker            # docker role downloaded from Galaxy
      automation_role/              # this can be the custom role you create
        tasks/
          main.yml                  # tasks that are common to your playbooks
        vars/
          main.yml                  # vars you set for your tasks within the role
        defaults/
          main.yml                  # default values you that can provide
                                    # defaults to your role tasks
                                    # and can be overriden by playbook
        library/                    # can also contain custom modules
