=====================================
Ceilometer role for OpenStack-Ansible
=====================================

This Ansible role installs and configures OpenStack ceilometer.

Meter and notification storage is configured to use a MongoDB backend
by default. This role does not install and configure the MongoDB backend.
Deployers wishing to use MongoDB must install and configure it prior to
using this role and override ``ceilometer_connection_string`` variable with
MongoDB connection details.

Table of Contents
=================

.. toctree::
   :maxdepth: 2

   configure-ceilometer.rst

To clone or view the source code for this repository, visit the role repository
for `os_ceilometer <https://github.com/openstack/openstack-ansible-os_ceilometer>`_.

Adding The Service to Your OpenStack-Ansible Deployment
-------------------------------------------------------

To add a new service to your OpenStack-Ansible (OSA) deployment:

* Define ``metering-compute_hosts`` and ``metering-compute_hosts`` (on which
  runs ceilometer compute agent) in your ``conf.d`` or
  ``openstack_user_config.yml``. For example:

  .. code-block:: yaml

      metering-infra_hosts:
        infra1:
          ip: 172.20.236.111
        infra2:
          ip: 172.20.236.112
        infra3:
          ip: 172.20.236.113

      metering-compute_hosts:
        compute1:
          ip: 172.20.236.110

* Create respective LXC containers (skip this step for metal deployments):

  .. code-block:: console

     openstack-ansible openstack.osa.containers_lxc_create --limit ceilometer_all,metering-compute_hosts

* Run service deployment playbook:

  .. code-block:: console

     openstack-ansible openstack.osa.ceilometer

For more information, please refer to the `OpenStack-Ansible project documentation <https://docs.openstack.org/project-deploy-guide/openstack-ansible/latest/>`_.

Always verify that the integration is successful and that the service behaves
correctly before using it in a production environment.

Default variables
~~~~~~~~~~~~~~~~~

.. literalinclude:: ../../defaults/main.yml
   :language: yaml
   :start-after: under the License.

Example playbook
~~~~~~~~~~~~~~~~

.. literalinclude:: ../../examples/playbook.yml
   :language: yaml

Dependencies
~~~~~~~~~~~~

This role needs pip >= 7.1 installed on the target host.

Tags
~~~~

This role supports two tags: ``ceilometer-install`` and
``ceilometer-config``. The ``ceilometer-install`` tag can be used to install
and upgrade. The ``ceilometer-config`` tag can be used to maintain the
configuration of the service.
