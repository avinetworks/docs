###############
Ad-Hoc Commands
###############

Ad-hoc commands are a great way to quickly issue a command against a host without running an entire playbook. They also don't require knowing YAML.

*************************
Ad-Hoc Command Structure
*************************

.. code-block:: shell

  ansible <group or hostname> -m <module name> -u username [--ask-pass] [--become] [--ask-become-pass]

For more options just type ``ansible --help`` and a full list of options will be available.
