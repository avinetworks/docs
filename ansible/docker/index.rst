###############
Avi Docker Role
###############

Avi's Docker role was created to help deploy and configure the Docker service on your server. The goal was to allow users to not only install Docker, but also configure the service itself. We allow choosing specific storage drivers, and also have automated some of the configuration required when doing so.

****************
Example Usage
****************

**Sample Deployment on Ubuntu**

.. code-block:: yaml

  # myplaybook.yml
  ---
  - hosts: avicontrollers
    become: true
    roles:
      - role: avinetworks.docker

**Sample Deployment on CentOS/RedHat/Oracle**

.. code-block:: yaml

  # myplaybook.yml
  ---
  - hosts: avicontrollers
    become: true
    roles:
      - role: avinetworks.docker
        docker_storage_driver: devicemapper
        docker_block_device: /dev/sda3
