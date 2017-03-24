############################
Avi Controller Role
############################

To help automate the deployment of Avi Vantage Controller in your environment we've developed an Ansible Role which can deploy in many environemnts. This includes deploying from Docker Hub, Private Docker Repo, Docker compressed images (tgz), as well as some cloud environments including CSP deployment.

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


************************
Deploying Avi Controller
************************

Bare-metal Avi Deployment
========================

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

Deploying Avi Controller from the Docker Hub is the default action. However it does require the server to have internet access to Docker Hub as it will download the image.

**Steps:**

1. Install the Avi Controller role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avicontroller

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [avicontrollers]
      10.120.202.24

  Replace the IP with the IP of your baremetal host you want to deploy to.

3. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: avicontrollers
      become: true
      roles:
        - role: avinetworks.avicontroller
          con_controller_ip: 10.120.202.24
          con_cores: 4
          con_memory_gb: 16

  However, there are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avicontroller). Each one allows customization of the deployment.

  In this playbook we are specifying that this "play" should apply to the "avicontrollers" hosts. Which we have previously defined in the inventory file. We are also specifying that the controllers IP is 10.10.27.101, this is used to properly determine the interface that the controller will use. We are then specifying the core count that the controller will use. This is the amount of cores that docker will be permitted to use for the controller. Then we specify the amount of memory in GB that the controller will be permitted to use. We recommend using 16GB or more.

4. Verify your local user has access to the hosts you are deploying the controller to. You will need `sudo` access as well. Login using your current user.

  .. code-block:: bash

    ssh 10.120.202.24

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: Bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass


Using a Private Docker Repository
------------------------------------

**Steps:**

1. Install the Avi Controller role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avicontroller

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [avicontrollers]
      10.120.202.24

  Replace the IP with the IP of your baremetal host you want to deploy to.

3. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: avicontrollers
      become: true
      roles:
        - role: avinetworks.avicontroller
          con_docker_repo: mydockerrepo.company.com
          con_version: 16.3.9-23923929323923
          con_docker_repo_user: admin
          con_docker_repo_password: adminpassword
          con_controller_ip: 10.120.202.24
          con_cores: 4
          con_memory_gb: 16

  However, there are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avicontroller). Each one allows customization of the deployment.

  In this playbook we are specifying that this "play" should apply to the "avicontrollers" hosts. Which we have previously defined in the inventory file. We are also specifying that the controllers IP is 10.10.27.101, this is used to properly determine the interface that the controller will use. We are then specifying the core count that the controller will use. This is the amount of cores that docker will be permitted to use for the controller. Then we specify the amount of memory in GB that the controller will be permitted to use. We recommend using 16GB or more.

4. Verify your local user has access to the hosts you are deploying the controller to. You will need `sudo` access as well. Login using your current user.

  .. code-block:: bash

    ssh 10.120.202.24

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass



Using a Compressed Docker Image
------------------------------------

**Steps:**

1. Install the Avi Controller role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avicontroller

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [avicontrollers]
      10.120.202.24

  Replace the IP with the IP of your baremetal host you want to deploy to.

3. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: avicontrollers
      become: true
      roles:
        - role: avinetworks.avicontroller
          con_package_deploy: true
          con_package_source: /home/user/Downloads/controller_docker.tgz
          con_controller_ip: 10.120.202.24
          con_cores: 4
          con_memory_gb: 16

  However, there are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avicontroller). Each one allows customization of the deployment.

  In this playbook we are specifying that this "play" should apply to the "avicontrollers" hosts. Which we have previously defined in the inventory file. We are also specifying that the controllers IP is 10.10.27.101, this is used to properly determine the interface that the controller will use. We are then specifying the core count that the controller will use. This is the amount of cores that docker will be permitted to use for the controller. Then we specify the amount of memory in GB that the controller will be permitted to use. We recommend using 16GB or more. We also needed to specify that we want to deploy from package via ``con_package_deploy: true`` which tells the role we're deploying from package. Then we provided the location of the package file by providing the ``con_package_source`` parameter.

4. Verify your local user has access to the hosts you are deploying the controller to. You will need `sudo` access as well. Login using your current user.

  .. code-block:: bash

    ssh 10.120.202.24

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass



Cisco CSP Avi Deployment
==========================

Requirements
------------
You will need to have available memory and storage for both the image, and the service on your CSP 2100.


Using QCOW image
----------------

**Steps:**

1. Install the Avi Controller role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avicontroller

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [csp_devices]
      10.120.222.56

  Replace the IP with the IP of your CSP host you want to deploy to.

3. Make sure you are able to SSH into the CSP device.

  .. code-block:: bash

    ssh user@10.120.222.56

4. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: csp_devices
      gather_facts: false
      roles:
        - role: avinetworks.avicontroller
          con_deploy_type: csp
          con_csp_user: admin
          con_csp_password: password
          con_csp_qcow_image_file: avi-controller.qcow2
          con_csp_mgmt_ip: 10.128.2.20
          con_csp_mgmt_mask: 255.255.255.0
          con_csp_default_gw: 10.128.2.1
          con_csp_service_name: avi-controller
          con_csp_num_cpu: 4
          con_csp_memory_gb: 16

  con_deploy_type:
      Sets the type of deployment that should be triggered.
  con_csp_user:
      Username that will be used to connect to the CSP server
  con_csp_password:
      Password required to authenticate the user
  con_csp_qcow_image_file:
      is the relative, or absolute location of the qcow file that will be uploaded to the CSP device.
  con_csp_mgmt_ip:
      IP of the controller on the management network.
  con_csp_mgmt_mask:
      Subnet mask that the controller will require.
  con_csp_default_gw:
      Default gateway for the controller
  con_csp_service_name:
      Name of the service to be created on the CSP
  con_csp_num_cpu:
      Number of CPUs to be allocated to the Controller
  con_csp_memory_gb:
      Amount of memory in GB allocated to the Controller

  .. note:: There are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avicontroller). Each one allows customization of the deployment.

  .. _CSP Deployment Variables: https://github.com/avinetworks/ansible-role-avicontroller#csp-deployment-variables

  In this playbook we are specifying that this "play" should apply to the "csp_devices" hosts. Anything not required mentioned here `CSP Deployment Variables`_ can be omitted from your role parameters. Defaults are also noted on that document.

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass
