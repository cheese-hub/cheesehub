Local development
=================

Using kubeadm-bootstrap
-----------------------

If you have access to an Ubuntu VM, the easiest approach to getting a
development environment up is to use the ``kubeadm-bootstrap`` process. At this
time, we've forked the original `data8 repository <https://github.com/data-8/kubeadm-bootstrap>`_ 
to add support for Weave, which is required by CHEESE.

On your Ubuntu VM:

.. code-block:: bash

   git clone https://github.com/nds-org/kubeadm-bootstrap
   cd kubeadm-bootstrap
   sudo ./install-kubeadm.bash
   sudo -E ./init-master.bash weave

Next, clone and configure the Workbench Helm chart:

.. code-block:: bash

   git clone https://github.com/nds-org/workbench-helm-chart
   cd workbench-helm-chart

Generate a self-signed certificate:

.. code-block:: bash

   ./generate-self-signed-cert.sh dev.cheesehub.org

Customize the helm chart ``values.yml``.  See the Helm chart
`README
<https://github.com/nds-org/workbench-helm-chart/blob/master/README.md>`_
for details. Change the ``specs.repo`` to:

.. code-block:: bash

   specs:
     repo: "https://github.com/cheese-hub/catalog.git"
     branch: "master"

Workbench relies on labeled nodes. Label the node:

.. code-block:: bash

   kubectl label nodes <nodename>  ndslabs-role-compute=true


Finally, install the chart:

.. code-block:: bash

    helm install . --name=workbench --namespace=workbench

Once the chart is installed and services are running you should be able to
register and login to your Workbench/CHEESEhub instance.


Using Minikube
--------------


Install and start  `Minikube <https://kubernetes.io/docs/tasks/tools/install-minikube/>`_
based on the official documentation for your operating system.

.. code-block:: bash
 
   minikube start


Install `Helm <https://docs.helm.sh/using_helm/#installing-helm>`_ based on the
official documentation for your operating system.

Install the tiller service:

.. code-block:: bash

   kubectl --namespace kube-system create sa tiller
   kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
   helm init --service-account tiller


Install the NGINX load balancer based on the `data-8 kubeadm-bootstrap repo
<https://github.com/data-8/kubeadm-bootstrap.git>`_:


.. code-block:: bash

   git clone https://github.com/data-8/kubeadm-bootstrap.git
   cd kubeadm-bootstrap/support && helm dep up && cd ..
   helm install --name=support --namespace=support support/


Workbench requires wildcard DNS. You can either create entries in ``/etc/hosts`` as needed or install
``dnsmasq``. The following instructions are for MacOS based on https://gist.github.com/petemcw/9265821:

.. code-block:: bash

   brew up
   brew install dnsmasq


Get the IP of your minikube instance:

.. code-block:: bash
  
   minikube ip


Edit ``/usr/local/etc/dnsmasq.conf`` and add the following entry around line 80 replacing the value with your actual minikube IP:

.. code-block:: bash

   address=/cheesehub.local/<minikube-ip>


Setup resolution for domain ``cheesehub.local``:

.. code-block:: bash

   $ sudo mkdir -p /etc/resolver
   $ sudo tee /etc/resolver/cheesehub.local > /dev/null <<EOF
   nameserver 127.0.0.1
   domain cheesehub.local
   search_order 1
   EOF


Restart ``dnsmasq``:

.. code-block:: bash

   sudo launchctl stop homebrew.mxcl.dnsmasq
   sudo launchctl start homebrew.mxcl.dnsmasq


Confirm DNS is working:

.. code-block:: bash

   ping xyz.cheesehub.local
   PING xyz.cheesehub.local (192.168.99.100): 56 data bytes
   64 bytes from 192.168.99.100: icmp_seq=0 ttl=64 time=0.844 ms


Next, clone and configure the Workbench Helm chart:

.. code-block:: bash

   git clone https://github.com/nds-org/workbench-helm-chart
   cd workbench-helm-chart


Generate a self-signed certificate:

.. code-block:: bash

   ./generate-self-signed-cert.sh cheesehub.local

Customize the helm chart ``values.yml``.  At a minimum, change the ``domain`` to ``cheesehub.local``, 
set ``require_account_approval`` to ``false`` and configure your SSL certs. See the Helm chart
`README
<https://github.com/nds-org/workbench-helm-chart/blob/master/README.md>`_
for details. Change the ``specs.repo`` to:

.. code-block:: bash

   specs:
     repo: "https://github.com/cheese-hub/catalog.git"
     branch: "master"

Workbench relies on labeled nodes. Label the node:

.. code-block:: bash

   kubectl label nodes minikube  ndslabs-role-compute=true


Finally, install the chart:

.. code-block:: bash

    helm install . --name=workbench --namespace=workbench

Once the chart is installed and services are running you should be able to register and login to your Workbench/CHEESEhub instance at https://www.cheesehub.local
