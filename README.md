# TripleO-QuickStart with Distributed-CI

This is an example of an integration of Distributed-CI and TripleO-Quickstart.

## Usage


```bash
cd ansible
```

Prepare and source the dcirc.sh environment file:

```bash
vim files/dcirc.sh
source files/dcirc.sh
```

Load the OpenStack openrc:

```bash
source ~/openrc.sh
```

:warning: If you may need  to use a KeyStone v2 OpenRC file.

Adjust the public IP of the jumpbox and the virthost in the hosts file. The
IP addresses should come from your tenant floating IP pool:

```bash
vim hosts
```

Finally you can start the deployment:
```
ansible-playbook -i hosts -e rhsm_login=XXXX -e rhsm_password=XXXX bootstrap.yml
```

## Workflow

The playbook will do the following steps:

- bootstrap.yml playbook
  - prepare the jumpbox and virthost virtual machines
  - deploy the dci-ansible-agent
  - prepare the configuration for dci-ansible-agent
  - fetch the last version of quickstart.sh
  - push an up to date copy of tripleo-environments
  - start the dci-ansible-agent agent
  - dci-ansible-agent
    - request a new DCI job to the DCI Control Server
    - fetch the RHOSP or RDO repository to validate
    - expose the repository on an internal HTTP server
    - call quickstart.sh with a special requirement file
    - quickstart.sh from TripleO-Quickstart
      - use to requirement to:
        - fetch the last version of rhos-10-baseos-undercloud-dci.yml
        - use the local copy of tripleo-environents
      - deploy an openstack
    - run the DCI tests
      - collect-logs
      - validate-tempest
      - certification

## Gating

The `rpm_to_gate` parameter can be used to inject a local RPM on the jumpbox. In this case, it will be use
instead of the RPM from the upstream repository:

```bash
ansible-playbook -i hosts -e rhsm_login=XXXX -e rhsm_password=XXXX bootstrap.yml -e rpm_to_gate=/tmp/dci-ansible.rpm
```
