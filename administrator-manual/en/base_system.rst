.. _base_system-section:

===========
Base system
===========

This chapter describes all available modules at the end of installation.
All modules outside this section must be installed from :ref:`package_manager-section`, including
backup and users support.

.. _dashboard-section:

Dashboard
=========

The index:`Dashboard` page is the landing page after a successful login.
The page will display the :index:`status` and configurations of the system.

.. _network-section:

Network
=======

:index:`Network` configuration tells the system how the server is connected to local network (LAN) or public ones (Internet).

If the server has firewall and gateway functionality, it will handle extra networks with special function like 
DMZ (DeMilitarized Zone) and guests network.

|product| supports an unlimited number of network interfaces.
Any network managed by the system must follow these rules:

* networks must be physically separated (multiple networks can't be connected to the same switch/hub)
* networks must be logically separated: each network must have different addresses
* private networks, like LANs, must follow address's convention from RFC1918 document.
  See :ref:`RFC1918-section`

Every network interface as a specific role which determinates its behavior. Roles are identified by colors.
Each role correspond to a well-known zone with special network traffic rules:

* *green*: local network. Hosts on this network can access any other configured network
* *blue*: guests network. Hosts on this network can access orange and red network, but can't access to green zone
* *orange*: DMZ network.  Hosts on this network can access red networks, but can't access to blue, orange and green zones
* *red*: public network. Hosts on this network can access only the server itself

See :ref:`policy-section` for more information on roles and firewall rules.

.. note:: The server must have at least one network interface. When the server has only one interface, this interface must have green role.

If the server i installed on a public VPS (Virtual Private Server) public, it should must be configured with a green interface.
All critical services should be closed using :ref:`network_services-section` panel.

.. _logical_interfaces-section:

Logical interfaces
------------------

Supported logical interfaces are:

* :index:`alias`: associate more than one IP address to an existing network interface. 
  The alias has the same role of its associated physical interface
* :index:`bond`: arrange two or more network interfaces, provides load balancing and fault tolerance
* :index:`bridge`: connect two different networks, it's often used for bridged VPN and virtual machine
* :index:`vlan` (Virtual Local Area Network): create two or more physically separated networks using a single interface

Aliases are used to configure multiple IPs on a single NIC. For example, if you want to have more public IP on a
red interface.

Bonds allow you to aggregate bandwidth between two or more network interfaces. The system will use all network interfaces
at the same time, balancing traffic among all active interfaces.
If an error occurs, the faulty card is automatically excluded from the bond.

Bridge has the function to connect different network segments, for example by allowing virtual machines, or client connected using a VPN,
to access to the local network (green).

When it is not possible to physically separate two different networks, you can use tagged vlan. The traffic of the two networks can
be transmitted on the same cable, but it will be handled as if it were sent and received on separate network cards.
The use of VLAN, requires properly configured switches.

.. _RFC1918-section:

Address for private networks (RFC1918)
--------------------------------------

TCP/IP private networks not directly connected to Internet should use special addresses selected by
Internet Assigned Numbers Authority (IANA).

===============   ===========   =============================
Private network   Subnet mask   IP addresses interval
===============   ===========   =============================
10.0.0.0          255.0.0.0     10.0.0.1 - 10.255.255.254
172.16.0.0        255.240.0.0   172.16.0.1 - 172.31.255.254
192.168.0.0       255.255.0.0   192.168.0.1 - 192.168.255.254
===============   ===========   =============================

.. _network_services-section:

Network services
================

A :index:`network service` is a service running on the firewall itself.

These services are always available to hosts on green network (local network).
Access policies can be modified from :guilabel:`Network services` page.

Available policies are:

* Access only from local networks (private): all hosts from green network and from VPNs
* Access from any network (public): any hosts
* Access only from the server itself (none): no host can connect to selected service

If policies private or public are selected, it’s possible to add a specific host (or a host list) always allowed (or blocked).

.. _remote_access-section:

Remote access
=============

Server Manager
--------------

It's possible to grant Server Manager's access to selected networks.
For example, if the server is inside a customer network, you should allow connections from remote management networks.

SSH
---

The :index:`SSH` (Secure Shell) should be always available.
SSH is a protocol to open remote shells over secure connections.

Default configuration allows authentication using password and public/private keys.


.. _trusted_networks-section:


Trusted networks
================

:dfn:`Trusted networks` are special networks (local or remote) allowed to access special server's services.

For example, hosts inside :index:`trusted networks` can access to:

* Server Manager
* Shared folders (SAMBA)

If users connected from VPNs must access system's services, add VPN networks to this page.

If the remote network is reachable using a router, remember to add a static route inside :ref:`static_routes-section` page.



.. _static_routes-section:

Static routes
==============

This page allow to create special :index:`static routes` which will use the specified gateway.
These routes are usually used to connect private network.

Remember to add the network to :ref:`trusted_networks-section`, if you wish to allow remote hosts to access local services.


.. _organization_contacts-section:

Organization contacts
=====================

Fields in this section are used to generate self-signed SSL certificates and for user creation.

.. note:: Any modification to these data will regenerate all SSL certificates. Most clients will must
   be reconfigured.

.. _user_profile-section:

User's profile
==============

All users can login to Server Manager using their own credentials.

After login, a user can :index:`change the password` and information about the account, like:

* Name and surname
* External mail address

The user can also overwrite fields set by the administrator:

* Company
* Office
* Address
* City

Shutdown
========

The machine where |product| is installed can be rebooted or halted from the :menuselection:`Shutdown` page.
Choose an option (reboot or halt) then click on submit button.

Always use this module to avoid bad shutdown which can cause data damages.

Log viewer
==========

All services will save operations inside files called :dfn:`logs`.
The :index:`log` analysis is the main tool to find and resolve problems.
To analyze log files click in :menuselection:`Log viewer`.

This module allows to:

* start search on all server's logs
* display a single log
* follow the content of a log in real time

Date and time
=============

After installation, make sure the server is configured with the correct timezone.
The machine clock can be configured manually or automatically using public NTP servers (preferred).

The machine clock is very important in many protocols. To avoid problems, all hosts in LAN can be configured to use the server as NTP server.

