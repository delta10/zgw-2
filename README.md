# ZGW v2
This installer can be used to install the [VNG reference implementation of the ZGW API's](https://github.com/vng-realisatie/gemma-zaken) v2. It uses Ansible and docker-compose to run the containers.

## Installation
This playbook only works on Ubuntu 18.04 LTS.

First make sure Ansible is installed on the machine where you would like to run the Ansible playbook:

```bash
sudo apt update
sudo apt install -y software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt install -y ansible
```

Now adjust the `ansible/hosts.ini` file and run the playbook:

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

The playbook creates a `/opt/zgw` folder that includes the `docker-compose.yml` file that spins up all the containers and the nginx loadbalancer. Also a new systemd service is created that spins up the `docker-compose.yml` on system boot.

## Configuration
The components now need to be configured with initial setup:

```bash
cd /opt/zgw
./configure.sh
```
