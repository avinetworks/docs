************************************
System Requirements: Hardware
************************************

Avi Vantage runs on standard x86-based servers, with no requirement for special-purpose hardware. In general, adding hardware capacity will greatly expand overall system capacity, for both Avi SEs and Avi Controllers. Please consult an Avi sales engineer or Avi technical support for recommendations tailored to meet the specific needs of your applications and environment.

The defaults are:

- Avi Controller: 8 vCPU cores, 24 GB RAM, and 64 GB of storage. (Click here for important details, including minimum sizing requirements for Avi Controllers.)
- Avi Service Engine: 2 vCPU cores, 2 GB RAM, and 10 GB of storage. (Click here for important details, including minimum sizing requirements for Avi SEs.

A typical deployment will have three Controllers in a redundant Controller cluster. The number of SEs required will depend on the number of applications being served by Avi Vantage and the configured level of redundancy.

.. note::
    - Reservation for CPU and memory is strongly preferred, but not required.
    - Modifying resource settings on VMs, such as CPU cores or RAM, requires powering down the VM, making the changes, and then powering the VM back on.
