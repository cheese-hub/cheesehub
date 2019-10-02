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
install CHEESEhub either as a single-node or multi-node installation.


Provision Kubernetes Cluster
----------------------------

CHEESEhub uses the `kubeadm-terraform <https://github.com/nds-org/kubeadm-terraform>`_ 
to provision Kubernetes clusters on OpenStack.

.. code-block:: bash

  git clone https://github.com/nds-org/kubeadm-terraform


Setup Wildcard DNS
------------------

CHEESEhub requires wildcard DNS support for `*.your.cheesehub.org`. If you do
not have access to manage your own domain, contact us.


Setup Wildcard TLS
------------------

CHEESEHub requires a valid wildcard TLS certificate for `*.your.cheesehub.org`.
Free wildcard certificates are available from Let's Encrypt.

Follow these instructions to generate a valid certificate for your domain: https://opensource.ncsa.illinois.edu/confluence/display/NDS/Wildcard+Certs+via+LetsEncrypt

The certificate and key should be used in your Workbench configuration below.


NGINX Ingress Controller
------------------------

The `kubeadm-terraform` installs an older version of the NGINX controller.  

Delete the old controller:

.. code-block:: bash

  helm delete --purge support


Create `values.yaml`:

.. code-block:: bash

  controller:
    hostNetwork: true
    kind: DaemonSet
    extraArgs:
      default-ssl-certificate: workbench/ndslabs-tls-secret
    config:
      proxy-connect-timeout: "300"
      proxy-read-timeout: "300"
      proxy-send-imeout: "300"
      body-size: "64m"
      worker-shutdown-timeout: "900s"

Install the new controller:

.. code-block:: bash

  sudo helm upgrade \
    --install support stable/nginx-ingress \
    --namespace=support \
    --version=1.17.0 \
    -f values.yaml


Install Workbench Helm Chart
----------------------------

CHEESEHub uses the following values:

.. code-block:: bash

  name: "CHEESEHub"
  domain: "hub.cheesehub.org"
  support_email: <your email address>
  repo: "https://github.com/cheese-hub/catalog.git"
  cert: <See above>
  key: <See above>
  smtp.host: <Your smtp host>
  smtp.port: <Your smtp port>

Install the Helm chart:

.. code-block:: bash

  helm install . --name=workbench --namespace=workbench


Access your instance
--------------------

Use `kubectl` to confirm your workbench instance is running:

.. code-block:: bash

  kubectl get pods -n workbench
  NAME                         READY     STATUS    RESTARTS   AGE
  workbench-7cb876c6b5-tmf8m   4/4       Running   0          5h


Access your instance at https://www.your.cheesehub.org.

