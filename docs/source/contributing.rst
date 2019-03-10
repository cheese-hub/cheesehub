.. _contributing:

Contributing
============

How to Contribute
^^^^^^^^^^^^^^^^^

You can contribute to the CHEESE initiative through any of the following:

* Suggest changes or fix problems by `submitting an issue <https://github.com/cheese-hub/cheesehub/issues>`_ or `creating a pull request <https://github.com/cheese-hub/cheesehub/pulls>`_.
* Contribute to existing hands-on scenarios or :ref:`lessons`.
* Contribute a new hands-on scenario by developing Github repository defining Docker images and related content.
* Contribute a new lesson by developing a Github repository based on the `Carpentry lesson template <https://github.com/carpentries/lesson-example>`_. 
* Become a reviewer to review and approve lesson content and/or scenarios.


Hands-on Scenarios
^^^^^^^^^^^^^^^^^^

Hands-on scenarios in CHEESEHub are implemented as one or more Docker images
configured to run in Labs Workbench and demonstrate a particular technical
concept.

The `ARP spoofing example <https://github.com/cheese-hub/arpspoof>`_ provides an
illustration of hands-on scenario development.

**Example: ArpSpoof**

The ARP Spoofing scenario is a 3-component scenario with victim (client), 
server, and hacker containers.  The victim container is a VNC-enabled Ubuntu 
environment with terminal, web browser, and network tools.  The server is an 
Apache webserver with web-based terminal and example application.  The hacker 
is a Jupyter notebook server including notebook with step-by-step instructions 
for performing an ARP poisoning attack. 
 

Developing this scenario required the following: 

* `Github <https://github.com/>`_ account
* `Dockerhub <https://hub.docker.com>`_ account
* Creation of a Github repository containing the scenario materials. This includes at minimum a Dockerfile and README.md
* Automated build configuration on Dockerhub 
* Creation of a set of JSON specifications in the CHEESE `catalog <https://github.com/cheese-hub/catalog>`_ defining each application in the scenario. 

Once the scenario was developed, the author created a pull request on the
`CHEESE appliation catalog <https://github.com/cheese-hub/catalog>`_ containing
the JSON specifications, initiating the review process. The scenario was
reviewed both technically and for security concepts.

**Checklist**

The following checklist can be used to guide scenario development. Each new
scenario repository should have the following

* Dockerfile
* Top level README.md
* License (MIT or BSD-3 recommended)
* Scenario instructions either via Lessons or integrated Notebook

The README.md should have the following headings:

* Description of the scenario: What is the scenario?
* Target audience: Who is the scenario for? This may be boilerplate.
* Design and architecture: Describe why you chose the tools/methods you did and any limitations including diagrams.
* Installation and usage: At a minimum, include a link to CHEESEHub. Optionally, describe how to run directly in Docker and/or Kubernetes
* How to contribute: A link to the contributing guidelines.


Lessons
^^^^^^^

All hands-on scenarios will be accompanied by Carpentry-style lessons. Examples
forthcoming.


Reviewers
^^^^^^^^^

The CHEESE community needs reviewers with expertise in each of the following:

* Technical aspects
* Security aspects
* Instructional/education aspects
