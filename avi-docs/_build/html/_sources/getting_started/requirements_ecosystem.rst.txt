************************************
System Requirements: Ecosystem
************************************


Hypervisor Support
======================

- Amazon Web Services (AWS)
- Google Cloud Platform
- Nutanix Acropolis 4.6
- OpenStack environments: KVM - RHEL/CentOS 6.4, 7.1, Ubuntu 12.04, 14.04, 16.04
- VMware vSphere 5.1, 5.5, 6.0

Bare Metal (Linux Server Cloud)
=================================

- Bare metal hosts running:

    - Oracle Enterprise Linux 7.0, 7.1, 7.2, 7.3
    - Red Hat Enterprise Linux 7.0, 7.1, 7.2, 7.3
    - CentOS Linux 7.0, 7.1, 7.2, 7.3
    - Ubuntu LTS 14.04, 16.04


Orchestrator Support
======================

- Docker UCP version 1.1.1
- Fleet 0.10.5
- Kubernetes 1.3+
- Marathon 0.13.x, 0.14.x, 0.15.0, 0.15.1, 0.15.2, and 0.15.3
- Mesos 0.23.0, 0.23.1, 0.24.0, 0.24.1, 0.25.0, 0.26.0, 0.27.0, and 0.27.1
- Mesosphere DC/OS 1.6 (16.2 and later), 1.8 (16.2.3 and later)
- OpenShift v3
- OpenStack Version Support: Havana, Icehouse, Juno, Kilo, Liberty, Mitaka. LBaaS v1 and v2. Keystone v2 and v3
- Rancher (Server/Agent): v1.0.0; Cattle: v0.159.2
- VMware vCenter 5.1, 5.5, 6.0, 6.5, vCO and vCAC

SDN Solutions
======================

- Cisco APIC Version 1.03(f) and later
- Juniper Contrail v3.0.2 and later (only for OpenStack)
- Nuage v3.1 and later (only for OpenStack)

Avi Vantage may be deployed in various environments with write (recommended), read, or no access integration with the virtualization orchestrator. The primary difference among these modes is the level of automation performed by Avi Vantage and the cloud orchestrator compared to the level of manual configuration required of administrators. There are no differences in hardware or system requirements among these modes. Servers being load balanced by Avi Vantage may exist within the same virtualization environment or be bare-metal, non-virtualized servers.

Avi supports the ability to manage multiple cloud environments from a single Controller cluster.
