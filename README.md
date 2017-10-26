# Ansible Role: airflow

[![Build Status](https://travis-ci.org/CreditCardsCom/ansible-airflow.svg?branch=master)](https://travis-ci.org/CreditCardsCom/ansible-airflow)

Install and configure an [airflow](http://airflow.incubator.apache.org/) host. Can be used to configure master and worker nodes.

## Requirements

You will require sudo access to the host being provisioned, use the following to run as root.

```yml
- name: Provision airflow master hosts
  hosts: airflow_hosts
  become: yes
  roles:
    - creditcardscom.airflow
```

## Role Variables

Base airflow configuration variables:

```yml
# User airflow should be started under
airflow_user: airflow

# Airflow version to be installed
airflow_version: 1.8.1

# Directory airflow will be installed to
airflow_install_directory: /usr/share/airflow

# Extra packages that will be installed for airflow via pip install
airflow_extra_packages:
  - celery
  - alldbs

# Services that will be started on the host
airflow_services:
  - webserver
  - scheduler
  - worker
```

Extra variables to support specific deployment patterns:

```yml
# determine whether and which process manager to setup
use_systemd: yes
use_supervisor: no

# support installing airflow into a virtual environment
# NOTE: required for use with ansible-container
virtualenv_path: undefined
```

#### `airflow.cfg`
The main airflow configuration happens with yaml objects, which is simply the default configuration `default_airflow` gets merged with any overrides found in the `airflow` object. All object keys will be templated into the `airflow.cfg` under the appropriate section. *All values are the default airflow values.*

```yml
airflow:
  core:
    dags_folder: "/usr/share/airflow_dags"
    base_log_folder: "/var/log/airflow"

  webserver:
    base_url: "{{ airflow_base_url }}"

  scheduler:
    statsd_on: True
```

## Dependencies

None

## Example Playbook

#### play.yml

```yml
- hosts: airflow-masters
  become: yes
  roles:
    - creditcardscom.airflow
```

#### `vars/main.yml`

```yml
airflow_user: airflow_user
airflow_version: 1.8.0
airflow_services:
  - webserver
  - scheduler

airflow:
  core:
    dags_folder: "/home/{{ airflow_user }}/dags"
```

## License (MIT)

Copyright (c) 2017 CreditCards.com

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
