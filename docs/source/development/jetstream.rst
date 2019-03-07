Deploying on Jetstream
======================

The CHEESE project uses cloud resources provided by 
`NSF XSEDE Jetstream <https://portal.xsede.org/jetstream>`_.  This document 
describes how to configure a Jetstream project for use with CHEESEhub.

Apply for an allocation
-----------------------

Jestream allocations are available for research and education applications.  
See `XSEDE Resource Allocation System <https://portal.xsede.org/submit-request>`_
for more information.


Jetstream Project Setup
-----------------------
CHEESE uses the Jetstream OpenStack user interface (UI) and API for 
platform deployment. New Jetstream allocations require a few preliminary 
steps to setup project network, subnet, and router. See the Jetstream's 
`Setup+for+Horizon+API+User+Instances <https://iujetstream.atlassian.net/wiki/spaces/JWT/pages/44826638/Setup+for+Horizon+API+User+Instances>`_.

The basic steps include:

* Create network (named [project]-net)
* Create subnet
* Create router, attached to public network
* Add interface from router to project network
* Add security groups including remote SSH/HTTPS

Consider restricting SSH ingress to known CIDR ranges.

Upload Base OS Image
--------------------
The CHEESE platform is currently based on Ubuntu LTS images. It is 
necessary to upload your own image to Jetstream. This can be done
via the Horizon UI or via the OpenStack CLI.

Download the image from https://cloud-images.ubuntu.com/bionic/current/ and
upload using the OpenStack CLI:

.. code-block:: bash

   openstack image create --disk-format qcow2 --container-format bare \
              --file bionic-server-cloudimg-amd64.img "Ubuntu 18.04 LTS"


Create VM Instance
------------------
At this point you can create a VM instance based on the uploaded image and 
install CHEESEhub. 
