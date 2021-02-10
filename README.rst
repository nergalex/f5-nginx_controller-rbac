Role Base Access Control
=======================================================================
.. contents:: Table of Contents

Principle
==================================================
A User resource represents an NGINX Controller User account.
Assign Roles to Users :

1. Built-in role
###############
Assign one built-in role to define the **upper privilege** (i.e actions) an User can perform in NGINX Controller globally:

=======     ============    =======================================================================================================================================
Role        Permissions	    Details
=======     ============    =======================================================================================================================================
admin		Full	        Full permissions for all Environments and can publish API Definitions.
user		Write	        Write access to Environments and can publish API Definitions.
guest		Read-Only	    Read-Only access to Environments and cannot publish API Definitions.
=======     ============    =======================================================================================================================================

Permission options are:

- ``NONE``: Does not have any access to the path or object
- ``READ``: Has read only access (HTTP GET requests)
- ``WRITE``: Has read and write access (POST, PUT, PATCH requests) but cannot delete
- ``FULL``: Has read, write and delete access

More details `here <https://docs.nginx.com/nginx-controller/platform/access-management/manage-roles/>`_

2. Custom role(s)
###############
Create and assign one or multiple roles to defines a **set of permissions** that allow or prevent Users from performing operations in NGINX Controller path or object.
For each path or object, a permission is set and replace default permission of Built-in role.
Example:

- instance ``location(s)`` on which Users can deploy (``placement`` property in ``gateway`` object)
- which ``environment(s)`` Users can access (``gateways``, ``components``...)

Use Case
==================================================
Organization
###############
A company hosts Applications in Public Cloud.
Applications are grouped by Project (GCP) or Subscription (Azure).
A Project "ACME" nominate:

- "John Doe" as a responsible for App deployment. He needs a FULL access.
- "Jane Doe" as a responsible for Support. She needs only WRITE access on Apps Components.

Configuration
###############
NetOps decided to allocate dedicated WAF resources to ACME: 2 VMs per region France Central, West Europe
NetOps prepared configurations on NGINX Controller:

- Create 2 platform/``location``: ``acme-france_central``, ``acme-west_europe``
- Create 4 platform/``instance``, 2 per location: ``acme-fr-vm1``, ``acme-fr-vm2``, ``acme-we-vm1``, ``acme-we-vm2``
- Create an environment "acme"
- Create a role ``acme_admin_full`` that limits access to ACME's ``locations`` and ACME's ``environment``
- Create a role ``acme_admin_apps`` that limits access to ACME's ``Apps`` only

During a maintenance window, NetOps freeze platform changes to ACME's ``instances``:

- Update role ``acme_admin_full`` to READ access
- Update a role ``acme_admin_apps`` to READ access

Demo
###############

.. raw:: html

    <a href="http://www.youtube.com/watch?v=iGFQ6YrJ9Uk"><img src="http://img.youtube.com/vi/iGFQ6YrJ9Uk/0.jpg" width="600" height="400" title="NGINX Controller RBAC" alt="NGINX Controller RBAC"></a>

Objects tree
==================================================
Gateway:

.. figure:: _figures/gw.png

Component:

.. figure:: _figures/component.png

Location:

.. figure:: _figures/location.png
