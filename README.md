Ansible Role: Logstash
=========

[![CI](https://github.com/NETWAYS/ansible-role-logstash/workflows/Molecule%20Test/badge.svg?event=push)](https://github.com/NETWAYS/ansible-role-logstash/workflows/Molecule%20Test/badge.svg)

This role installs and configures [Logstash](https://www.elastic.co/products/logstash) on Linux systems.

It can optionally configure two types of Logstash pipelines:
* Pipeline configuration managed in an external git repository
* A default pipeline which will read from different Redis keys and write into Elasticsearch

It will work with the standard Elastic Stack packages and Elastics OSS variant.

Requirements
------------

You need to have the Elastic Repos configured on your system. You can use our [role](https://github.com/widhalmt/ansible-role-elastic-repos) for that but you don't have to.

If you want to use the default pipeline configuration you need to have `git` available.

You need to have `curl` installed. We are using `curl` instead of the `uri` module because we got better results. Feel free to file a pull request if you find a working solution. (And please remove this line with it)

If you want to use the default pipeline (or other pipelines communicating via Redis) you might want to install Redis first (e.g. by using an [Ansible Role for Redis](https://galaxy.ansible.com/geerlingguy/redis)

Role Variables
--------------

* *logstash_enable*: Start and enable Logstash service (default: `true`)
* *logstash_config_backup*: Keep backups of all changed configuration (defualt: `no`)
* *logstash_manage_yaml*: Manage and overwrite `logstash.yml` (default: `true`)

If `logstash.yml` is managed, the following settings apply.

* *logstash_config_autoreload*: Enable autoreload of Logstash configuration (default: `true`)
* *logstash_config_path_data*: Logstash data directory (default: `/var/lib/logstash`)
* *logstash_config_path_logs*: Logstash log directory (default: `/var/log/logstash`)

Aside from `logstash.yml` we can manage Logstashs pipelines.

* *logstash_manage_pipelines*: Manage pipelines at all (default: `true`)
* *logstash_pipelines*: List of pipelines with URL to repo
* *logstash_elasticsearch_output*: Enable default pipeline to Elasticsearch (default: `true`)
* *logstash_beats_input*: Enable default pipeline with `beats` input (default: `true`)
* *logstash_connector*: Enable default to connect input and output (default: `true`)
* *logstash_elasticsearch*: Address of Elasticsearch instance for default output (default: list of Elasticsearch nodes from `elasticsearch` role or `localhost` when used standalone)
* *logstash_security*: Enable X-Security (default: `false`)

These variables are identical over all our elastic related roles, hence the different naming scheme.

*elastic_release*: Major release version of Elastic stack to configure. (default: `7`)
*elastic_variant*: Variant of the stack to install. Valid values: `elastic` or `oss`. (default: `elastic`)

The following variables only apply if you use this role together with our Elasticsearch and Kibana roles.

* *elastic_stack_full_stack*: Use `ansible-role-elasticsearch` as well (default: `false`)
* *elastic_ca_dir*: Directory where the CA and certificates lie on the main Elasticsearch host (default: `/opt/es-ca`)
* *elastic_initial_passwords*: File where initial passwords are stored on the main Elasticsearch host (default: `/usr/share/elasticsearch/initial_passwords`)

Dependencies
------------

This role has no dependencies. As mentioned above you might want to use another role to install Redis

Example Playbook
----------------

This is a simple sample playbook which first uses an Ansible role to install Redis and afterwards install and configure Logstash.

    - hosts: logstash
      roles:
        - geerlingguy.redis
        - logstash


License
-------

GPLv3+

Author Information
------------------

This role was created in 2019 by [Netways](https://www.netways.de/).
