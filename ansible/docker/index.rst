###############
Avi Docker Role
###############

Avi's Docker role was created to help deploy and configure the Docker service on your server. The goal was to allow users to not only install Docker, but also configure the service itself. We allow choosing specific storage drivers, and also have automated some of the configuration required when doing so.

Docker recently has split their project into two new projects. Community Edition (CE) and Enterprise Edition (EE), these two versions have a couple differences.

**Docker Enterprise Edition (EE)**

Docker EE is the supported and certified container platform. Docker and its partners provide cooperative support for Certified Containers, and Plugins so customers can confidently use products in production.

Docker EE is available in three tiers:

- **Basic:** The Docker platform for certified infrastructure, with support from Docker Inc. and certified Containers and Plugins from Docker Store
- **Standard:** Adds advanced image and container management, LDAP/AD user integration, and role-based access control (Docker Datacenter)
- **Advanced:** Adds Docker Security Scanning and continuous vulnerability monitoring

It is available as a free trial and for purchase at Docker Store, or from the Docker Sales team. It is also supported by many Docker partners.

**Docker Community Edition (CE)**

Docker CE is the free version of Docker. It containers the full Docker platform, and is great for developers and operations teams starting to build container applications. CE uses time-based releases with a YY.MM versioning scheme. It can be enhanced by free and paid addons from the Docker Cloud.

Docker CE comes in two different variants:

- **Edge:** for users wanting to get the latest and greatest features
- **Stable:** released quarterly for users that want an easier-to-maintain release cycle


*****************************
Docker Community Edition (CE)
*****************************


Example Usage
=============

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

******************************
Docker Enterprise Edition (EE)
******************************

Example Usage
=============
