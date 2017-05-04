Using a Compressed Docker Image
------------------------------------

Steps
^^^^^

1. Install the Avi SE role on the host you are executing from.

  .. code-block:: bash

    sudo ansible-galaxy -f avinetworks.avise

2. Identify how we want to define our hosts. Ansible can accept hosts three ways: command-line, by inventory file, or by dynamic inventory file. We will use an inventory in this case. Ansible by default uses /etc/ansible/hosts as it's default inventory. Lets add this segment to the bottom of the page.

  .. code-block:: text

      [service_engines]
      10.120.202.56

  Replace the IP with the IP of your baremetal host you want to deploy to.

3. Create the playbook. It should look similar to this

  .. code-block:: yaml

    # myplaybook.yml
    ---
    - hosts: service_engines
      become: true
      roles:
        - role: avinetworks.avise
          se_package_deploy: true
          se_package_source: /home/user/Downloads/se_docker.tgz
          se_master_ctl_ip: 10.120.202.24
          se_master_ctl_username: admin
          se_master_ctl_password: avi123
          se_disk_gb: 60
          se_cores: 2
          se_memory_gb: 4

  However, there are are many possible values which can be found on the roles README file (https://github.com/avinetworks/ansible-role-avise). Each one allows customization of the deployment.

  We will automatically use the ``se_master_ctl_ip``, ``se_master_ctl_username``, and ``se_master_ctl_password`` to authenticate, and autoregister the service engine to the controllers default cloud. To choose which cloud you want please use the ``se_cloud_name`` value. If you use multiple tenants please use ``se_tenant`` to choose the specific tenant the cloud is under.

4. Verify your local user has access to the hosts you are deploying the service engine to. You will need `sudo` access as well. Login using your current user.

  .. code-block:: bash

    ssh 10.120.202.56

5. Execute the playbook.

  .. note::
    - If you are not using an ssh-key you will also need to specify `--ask-pass` to ansible.
    - If your current user is different you will need to specify `-u <username>` to ansible.

  .. code:: bash

    ansible-playbook myplaybook.yml -u <username> --ask-pass
