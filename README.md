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

Adjust the public IP of the jumpbox and the virthost in the hosts file. The
IP addresses should come from your tenant floating IP pool:

```bash
vim hosts
```

Finally you can start the deployment:
```
ansible-playbook -i hosts -e rhsm_login=XXXX -e rhsm_password=XXXX bootstrap.yml
```
