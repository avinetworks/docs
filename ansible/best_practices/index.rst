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
      hostname1                     # variables assigned to the specific host with name of 'hostname1'
      hostname2                     # variables assigned to the specific host with name of 'hostname2'

    library/                        # if you create custom modules to perform a task put them here

    avicontroller.yml               # playbook that can deploy your controller or
                                    # build your controller

    avise.yml                       # playbook that can prepare your SE servers for
                                    # the Linux Server Cluster Avi deploy

    create_vip.yml                  # playbook that may create a Virtual Service, Pool, etc.
    ...                             # add more as you like, if you have too many of these,
                                    # think about combining tasks into roles,
                                    # and providing variables to roles

    roles/
      avinetworks.avisdk            # avisdk role you downloaded from Ansible Galaxy
      avinetworks.avicontroller     # avicontroller role downloaded from Ansible Galaxy
      avinetworks.docker            # docker role downloaded from Ansible Galaxy
      automation_role/              # this can be the custom role you create
        tasks/
          main.yml                  # this can be a bunch of tasks that are common to your playbooks
        vars/
          main.yml                  # vars you set for your tasks within the role
        defaults/
          main.yml                  # these are default values you can use to provide
                                    # defaults to your role tasks (can be overriden by playbook
        library/                    # can also contain custom modules
