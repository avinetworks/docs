Cisco CSP Avi Deployment
==========================

Requirements
------------

- You will need to have available memory and storage for both the image, and the service on your Cisco CSP device.

Using QCOW image
----------------

Steps
^^^^^

1. Install the Avi SE role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avise

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [csp_devices]
      10.120.222.24

  Replace the IP with the IP of your CSP host you want to deploy to.

3. Make sure you are able to SSH into the CSP device.

  .. code-block:: bash

    ssh user@10.120.222.24

4. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: csp_devices
      gather_facts: false
      roles:
        - role: avinetworks.avise
          se_deploy_type: csp
          se_csp_user: admin
          se_csp_password: password
          se_master_ctl_ip: 10.128.2.20
          se_master_ctl_username: admin
          se_master_ctl_password: password
          se_csp_qcow_image_file: avi-se.qcow2
          se_csp_mgmt_ip: 10.128.2.20
          se_csp_mgmt_mask: 255.255.255.0
          se_csp_default_gw: 10.128.2.1
          se_csp_service_name: avi-se
          se_csp_disk_size: 10
          se_csp_num_cpu: 2
          se_csp_memory_gb: 4
          se_csp_vnics:
            - nic: "0"
              type: access
              tagged: "false"
              network_name: enp1s0f0
            - nic: 1
              type: passthrough
              passthrough_mode: sriov
              vlan: 200
              network_name: enp7s0f0
            - nic: 2
              type: passthrough
              passthrough_mode: sriov
              vlan: 201
              network_name: enp7s0f1

  .. note:: There are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avise). Each one allows customization of the deployment.

  .. _CSP Deployment Variables: https://github.com/avinetworks/ansible-role-avise#csp-deployment-variables

  In this playbook we are specifying that this "play" should apply to the "csp_devices" hosts. Anything not required mentioned here `CSP Deployment Variables`_ can be omitted from your role parameters. Defaults are also noted on that document.

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass
