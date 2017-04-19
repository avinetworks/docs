#########
Playbooks
#########

.. toctree::
  :maxdepth: 2

  components
  executing
  variables
  loops
  blocks
  best_practices

Ansible's configuration language is put into a Playbook. Playbooks have many components including hosts, variables, tasks, etc. We will cover all of these components and more throughout this section. Another way to look at playbooks is as a instruction for Ansible to orchestrate, provision, deploy, and configure your environment.

Playbooks can contain a list one or multiple plays. Each play can target the same or different systems, but plays are usually targetted to a specific group of systems.

Here is an example playbook containing a single play.

.. code-block:: yaml

  ---
  - hosts: all
    vars:
      controller_version: latest
    roles:
      - role: avinetworks.docker
      - role: avinetworks.avicontroller
        con_version: "{{ controller_version }}"

As you can see it's also in YAML format which we've covered earlier.
