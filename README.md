# Ansible playbooks for trojan

## Introduction

Those are Ansible playbook files for installing and maintaining [Trojan](https://github.com/trojan-gfw/trojan) servers.

`setup.yml` will:

- setup Nginx and Trojan
- provision an SSL certificate
- setup auto renewal

After installation, Nginx will listen on port 80, and Trojan will listen on port 443. All non-Trojan traffic will be forwarded to port 80, so your server will function as an inconspicuous HTTP/HTTP site.

You can use `update-config.yml` for updating configurations.

## Prerequisite

The playbooks are only tested under the following conditions:

- debian based Linux system
- services are managed by `systemd`
- some domain is registered and pointed to your server
- you've setup public-key authentication with ssh
- you can login as root or can become root without typing your password

## Usage

### Initial setup

Install a recent version of Python 3, and then install Ansible:

```
pip install ansible
```

Install the `certbot` role:

```
ansible-galaxy install geerlingguy.certbot
```

Rename the example configurations and change them according to your situation:

```
mv ansible.cfg-example ansible.cfg
mv hosts-example hosts
```

You probably don't need to worry about `ansible.cfg` but definitely need to change `hosts` to fill in your server ip and domainname. You can add multiple servers. Change your `~/.ssh/config` to setup additional ssh parameters if necessary.

Apply `setup.yml`

```
ansible-playbook -b setup.yml
```

### Updating configuration

If you've updated any of the configuration files (for example, passwords in `hosts`), apply `update-config.yml` to update the servers.

```
ansible-playbook -b update-config.yml
```
