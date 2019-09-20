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

Now adjust the `ansible/hosts.ini` file to your local settings. Then copy the example ini:

```bash
sudo cp inventory/group_vars/example/all.yml inventory/group_vars/all/all.yml
```

and adjust the settings in `inventory/group_vars/all/all.yml` to your local configuration.

Now run the playbook:

```bash
ansible-playbook -i inventory/hosts.ini playbook.yml
```

The playbook creates a `/opt/zgw` folder that includes the `docker-compose.yml` file that spins up all the containers and the nginx loadbalancer. Also a new systemd service is created that spins up the `docker-compose.yml` on system boot.

## Configuration
The components now need to be configured with initial setup (only needs to be run once):

```bash
cd /opt/zgw
./configure.sh
```

## Let's encrypt
The generation of SSL certificates is handled by Let's Encrypt. Bootstrap the configuration with:

```bash
sudo certbot certonly --webroot -w /var/www/html \
    -d gemma-zrc.my-domain.dev \
    -d gemma-ztc.my-domain.dev \
    -d gemma-drc.my-domain.dev \
    -d gemma-brc.my-domain.dev \
    -d gemma-nrc.my-domain.dev \
    -d gemma-ac.my-domain.dev \
    -d gemma-zaken-demo.my-domain.dev
```

And change the `zgw_ssl_enabled`, `zgw_ssl_certificate`, `zgw_ssl_certificate_key` and `zgw_ssl_dhparam` parameters in the inventory accordingly.
