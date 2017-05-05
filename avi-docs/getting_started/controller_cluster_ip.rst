############################
Controller Cluster IP
############################

The Avi Controller cluster IP address is a single IP address shared by multiple Avi Controllers within a cluster. This is the address to which the web interface, CLI commands and REST API calls are directed. As a best practice, to access the Avi Controller, one logs onto the cluster IP address instead of the IP addresses of individual Avi Controller nodes.

For cluster IPs, the management IPs of the Controllers must all be in the same subnet.

For AWS deployments where Controllers are on different subnets, customers have the option to use Route 53 with health checks to resolve the domain name of the Cluster to a Controller IP address directly.

.. note:: If the IP address of an Avi Controller node has changed, a script must be run to update the configuration.

************************
Cluster IP Advertisement
************************

The Avi Controller cluster IP is ARPed by whichever Avi Controller is the primary (or leader, depending on the infrastructure type) within the cluster. When another Avi Controller becomes the primary, it will send out a gratuitous ARP to claim ownership of the cluster IP.

Administrators may manage any of the Avi Controllers within the cluster by directly accessing the cluster IP address. The Avi Controllers will handle all replication, so there is no requirement to make changes specifically on the primary Avi Controller.

Note: In Avi, the cluster IP is not referred to as a "floating IP". In Avi Vantage, the term "floating IP" applies only to OpenStack.

***************************
Configuring the Cluster IP
***************************

Web Interface
=============

To add the cluster IP within the web interface, navigate to Administration > Controller > Edit. Add the new address to the Controller Cluster IP field. This change takes effect immediately upon saving.

.. note:: As of Avi Vantage 16.3, DNS host names may be specified in lieu of IP addresses.

.. image:: images/cluster-config-example.png
    :width: 600 px
