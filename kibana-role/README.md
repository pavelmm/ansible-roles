Ansible-Role Kibana for CentOS 7
=========

Role installs Kibana on CentOS 7. 

Requirements
------------

None.

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

* The version of kibana to install.
  ```yml
  kibana_version: "7.14.0"
  ```
* The URL (including port) over which Kibana will connect to Elasticsearch.
  ```yml
  kibana_elasticsearch_url: "http://localhost:9200"
  ```

Dependencies
------------

None.

Example Playbook
----------------

The simpliest example:
```yaml
- name: Install Kibana
  roles:
    - kibana-role
```

License
-------

MIT

Author Information
------------------

The role was created by Pavel Nadezhdin in 2022/05 as a homework.

