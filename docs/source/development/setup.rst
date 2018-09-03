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

   git clone https://github.com/cheese-hub/kubeadm-bootstrap
   cd kubeadm-bootstrap
   sudo ./install-kubeadm.bash
   sudo -E ./init-master.bash

Next, clone and configure the Workbench Helm chart:

.. code-block:: bash

   git clone https://github.com/nds-org/workbench-helm-chart
   cd workbench-helm-chart

Optionally, generate a self-signed certificate:

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

Finally, install the chart:

.. code-block:: bash

   helm install .

Once the chart is installed and services are running you should be able to
register and login to your Workbench/CHEESEhub instance.




