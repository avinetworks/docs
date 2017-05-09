######
Blocks
######

As long as you are using Ansible 2.0 and later, you can use blocks. These help in logically grouping a set of tasks in your playbooks, as well as error handling. Most options you can pass for a task will work for a block. However at this moment ``with_items``, and other loops do not work.

**************
Grouping tasks
**************

.. code-block:: yaml

  tasks:
    - block:
        - yum: name=httpd state=present

        - template: src=httpd.j2 dest=/etc/httpd/conf/httpd.conf

        - service: name=httpd state=started enabled=true

      when: ansible_distribution == "CentOS"
      become: true
      become_user: root

This is an example of creating a block of tasks which are only to run when the OS Distribution is CentOS. It helps clean up cases where you may have multiple tasks that rely on the same ``when`` statements, or other commonalities that can be combined.

*************
Error Handing
*************

Another nice feature of blocks includes the ability to add error handling to your playbooks. You can recover or create a process to roll back your tasks if a failure occurs. This can have great benefits for some use cases.

There are a few options in block we can use such as of course ``block``, but also ``rescue``, and ``always``.

.. code-block:: yaml

  tasks:
    - block:
        - debug: msg="Executing task 1"
        - shell: /bin/false
        - debug: msg="Executing task 2"
      rescue:
        - debug: msg="The previous command failed"
        - debug: msg="Execute rescue task 1"
        - shell: /bin/true
      always:
        - debug: msg="I will always work regardless of any failures prior"

To explain what happens here:

First we start executing tasks in the block. However the second task ``shell`` returns ``false`` which will fail the task. This results in the skip of "task 2". Then the block will load the ``rescue`` portion of the block. We can use this section to both inform the user that a block failed, and issue commands to fix it. Then the block will progress. It will always run the section ``always`` as the ``always`` section will run regardless of error condition on the ``block`` or ``rescue`` sections. Which could be handy to proceed or change data for each run successful or not.
