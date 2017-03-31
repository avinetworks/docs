############################
Ansible Best Practices
############################

There are a few key things we need to keep in mind when dealing with the deployment of Avi with Ansible.

- A change in Ansible role values will result in a change in the Avi deployment. Which will likely result in a controller being taken offline for a bit as it restarts with the new service configuration.
- Always, Always verify your values to desired values before execution. Ansible is powerful, it will do what you tell it. Remember: Garbage in, Garbage out.
- Automation makes it less likely to make a mistake, but automation also allows you scale the mistake very rapidly. Always test, or have someone else verify your code prior to execution.
