##################################
Create Pool and Virtual Service
##################################

In this example we are going to create a pool, and then a vip. We use two of our Ansible modules: ``avi_pool`` and ``avi_virtualservice``.

The first task we create the pool using the ``avi_pool`` module. Using the ``name`` value we are able to name the pool, then we set the ``enabled`` value to ``false``, because at this moment we do not want to enable the pool. ``health_monitor_refs`` field allows us to link the pools health monitor value to an already present health monitor. Due to the structure of the API, we will need to reference the monitor by it's API name (``/api/healthmonitor?name=System-HTTP``). If the health monitor doesn't exist and you make a reference to it, you will see an API error on execution. So if you are creating a new health monitor, please create it in the task prior to a reference. We also will now list our servers. These are done with the ``servers`` list, which includes a dictionary for each server. In this example we use an IPv4 address.

.. code-block:: yaml

  # Create pool, 1 VIP, and assign SSL Certificate to Pool
  ---
  - hosts: localhost
    connection: local
    environment:
        AVI_CONTROLLER: 10.10.27.90
        AVI_USERNAME: admin
        AVI_PASSWORD: AviNetworks123!
    roles:
      - role: avinetworks.avisdk
    tasks:
      # Create the pool using the avi_pool api
      - name: Create the testpool1 pool
        avi_pool:
          tenant: admin
          name: testpool1
          state: present
          enabled: false
          health_monitor_refs:
            - '/api/healthmonitor?name=System-HTTP'
          servers:
            - ip:
                addr: 10.90.130.13
                type: V4
            - ip:
                addr: 10.90.130.15
                type: V4
      ...................

Next we will create the virtual service. This module will look similar to this. We of course define the name, tenant, state, and other parameters required for the Virtual Service. We also specify the list of ``services`` to the virtual service. We also call in the ``System-Standard`` SSL profile, the Application profile, and define the SSL key, and certificate.

Additional values can be found in the Swagger documentation at https://<controller-ip>/swagger/#!/default/get_virtualservice

.. code-block:: yaml

      ...................
      - name: Create the Virtual Service testvs1 and assign testpool1 to it
        avi_virtualservice:
          tenant: admin
          name: testvs1
          state: present
          performance_limits:
            max_concurrent_connections: 1000
          services:
            - port: 443
              enable_ssl: true
            - port: 80
          ssl_profile_ref: '/api/sslprofile?name=System-Standard'
          application_profile_ref: '/api/applicationprofile?name=System-Secure-HTTP'
          ssl_key_and_certificate_refs:
            - '/api/sslkeyandcertificate?name=System-Default-Cert'
          ip_address:
            addr: 10.90.131.105
            type: V4
          pool_ref: '/api/pool?name=testpool1'
