##################################
Create Avi User
##################################

This is an example on how to work with the Avi Ansible modules to create a new user within a Tenant. You can definitely loop with the user as well.

.. note::
  Please be aware, this module is not idempotent because of the inability to match the ``password`` field.

Using the ``avi_api_session`` module we are able to make API calls freely to Avi. Using this we can control the type of HTTP Method, as well as the data and path of the call.

.. code-block:: yaml
  ---
  - hosts: localhost
    connection: local
    environment:
      AVI_CONTROLLER: ec2-34-250-111-254.eu-west-1.compute.amazonaws.com
      AVI_USERNAME: admin
      AVI_PASSWORD: AviNetworks123!
    roles:
      - role: avinetworks.avisdk
    tasks:
      - name: Check if user exists on Avi
        avi_api_session:
          http_method: get
          path: user?name=testuser
        register: user_exists

      - name: Create User on Avi
        avi_api_session:
          http_method: post
          path: user
          data:
            require_password_confirmation: false
            is_active: true
            access:
              - tenant_ref: '/api/tenant?name=admin'
                role_ref: '/api/role?name=System-Admin'
            default_tenant_ref: '/api/tenant?name=admin'
            full_name: testuser
            username: testuser
            password: AviNetworks123
        when: user_exists.obj.count < 1

As you can see because idempotency isn't default we created a check to see if the user already exists, and if the user doesn't exist we run the next task which creates the user.
