##################################
Create Pool and Virtual Service
##################################

In this example we are going to create a pool, and then a vip. We use two of our Ansible modules: ``avi_pool`` and ``avi_virtualservice``.

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
      # Create the virtual service using the avi_virtualservice
      # We have also configured our parameters for performance limits of the
      # virtual service.
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
