Ansible Role: Prometheus
======================================

[![Build Status](https://travis-ci.org/entercloudsuite/ansible-prometheus.svg?branch=master)](https://travis-ci.org/entercloudsuite/ansible-prometheus)
[![Galaxy](https://img.shields.io/badge/galaxy-entercloudsuite.prometheus-blue.svg?style=flat-square)](https://galaxy.ansible.com/entercloudsuite/prometheus)  

Installs Prometheus on Ubuntu 16.04 (Xenial)

## Requirements

This role requires Ansible 2.4 or higher.

## Role Variables

The role defines most of its variables in `defaults/main.yml`:

### `prometheus_components`
- List of components to be installed.
- Default value: **["prometheus", "alertmanager", "telegram_bot"]**

Telegram bot will install docker

### `prometheus_alertmanager_url`
- Url of the Alertmanager service.

- Default value: **http://{{alertmanager_url}}:{{alertmanager_port}}**
- Result Value **"http://localhost:9093/"**
### alertmanager_url
  - Default Value: localhost
### alertmanager_port:
  - Default : **9093**

### `prometheus_version`
- Version of the Prometheus service.
- Default value: **2.3.2**

### `alertmanager_version`
- Version of the Alertmanager service.
- Default value: **0.15.1**

### `prometheus_prometheus_web_external_url:`  
- The  URL  under  which  Prometheus  is  externally reachable  (for  example,  if  Prometheus  is  served  via a reverse proxy). Used for generating relative and absolute links back to Prometheus itself. If the URL has a path portion, it will be used to prefix all HTTP endpoints served by Prometheus. If omitted, relevant URL components will be derived automatically.
- Default value: **Not defined** (By default get hostname)

### `prometheus_prometheus_web_route_prefix:`  
- Prefix forthe internal routes of web endpoints.
- Default value: **Void**

### `prometheus_alertmanager_web_external_url:`  
- The  URL  under  which  Alertmanager  is  externally reachable  (for  example,  if  Alertmanagr  is  served  via a reverse proxy). Used for generating relative and absolute links back to Alertmanager itself. If the URL has a path portion, it will be used to prefix all HTTP endpoints served by Alertmanager. If omitted, relevant URL components will be derived automatically.
- Default value: **Not defined** (By default get hostname)

### `prometheus_alertmanager_web_route_prefix:`  
- Prefix forthe internal routes of web endpoints.
- Default value: **Void**


## Playbook Example
### Basic Playbook
``` yaml
Run with default vars:

    - hosts: all
      roles:
        - role: ansible-prometheus
          prometheus_server: prometheus_hostname
```

### Production Playbook

``` yaml
Run with default vars:
- hosts: all
      roles:
      - role: entercloudsuite.prometheus
        prometheus_conf_main: "prometheus/prometheus.yml"
        prometheus_alertmanager_conf: "prometheus/alertmanager.yml"
        prometheus_prometheus_bot_template_conf: prometheus/telegram_bot/template.tmpl
        prometheus_prometheus_bot_conf: prometheus/telegram_bot/config.yaml    
        prometheus_rule_files:
          basic_rules:
            src:  "prometheus/rules/basic.rules"
            dest: basic.rules
          black_box_rules:
            src:  "prometheus/rules/black_box.rules"
            dest: black_box.rules

```
### Production Tree Example

```
├── group_vars
│   └── all
├── host
├── playbook.yml
├── prometheus
│   ├── alertmanager.yml
│   ├── prometheus.yml
│   ├── rules
│   │   ├── basic.rules
│   │   └── black_box.rules
│   └── telegram_bot
│       ├── config.yaml
│       └── template.tmpl
├── requirements.yaml
└── roles

```

## Testing

Tests are performed using [Molecule](http://molecule.readthedocs.org/en/latest/).

Install Molecule or use `docker-compose run --rm molecule` to run a local Docker container, based on the [enterclousuite/molecule](https://hub.docker.com/r/fminzoni/molecule/) project, from where you can use `molecule`.

1. Run `molecule create` to start the target Docker container on your local engine.  
2. Use `molecule login` to log in to the running container.  
3. Edit the role files.  
4. Add other required roles (external) in the molecule/default/requirements.yml file.  
5. Edit the molecule/default/playbook.yml.  
6. Define infra tests under the molecule/default/tests folder using the goos verifier.  
7. When ready, use `molecule converge` to run the Ansible Playbook and `molecule verify` to execute the test suite.  
Note that the converge process starts performing a syntax check of the role.  
Destroy the Docker container with the command `molecule destroy`.   

To run all the steps with just one command, run `molecule test`. 

In order to run the role targeting a VM, use the playbook_deploy.yml file for example with the following command: `ansible-playbook ansible-prometheus/molecule/default/playbook_deploy.yml -i VM_IP_OR_FQDN, -u ubuntu --private-key private.pem`.  

# Fast Test
docker-compose run --rm molecule
molecule create
molecule converge

## License

MIT
