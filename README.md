# role_mysql_orchestrator
Ansible Role to install and configure Orchestrator (MySQL HA Tool)

```
- name: install orchestrator
  hosts: all
  pre_tasks:
    - name: install curl
      package: name=curl
    - name: install percona mysql repo
      shell: curl -O https://repo.percona.com/apt/percona-release_0.1-4.$(lsb_release -sc)_all.deb && dpkg -i percona-release_0.1-4.$(lsb_release -sc)_all.deb
      args:
        creates: /etc/apt/sources.list.d/percona-release.list
        executable: /bin/bash
        chdir: /tmp
  roles:
    - role: entercloudsuite.mysql
      mysql_packages:
        - percona-server-server-5.7
      mysql_users:
        - name: orchestrator
          host: '%'
          password: mysuperpassword
          priv: '*.*:ALL'
    - role: jsecchiero.orchestrator
      orchestrator_version: 3.0.10
      orchestrator_mysql_user: orchestrator
      orchestrator_mysql_password: mysuperpassword
      orchestrator_mysql_topology_user: orchestrator
      orchestrator_mysql_topology_password: mysuperpassword
      orchestrator_read_only: true
      orchestrator_super_read_only: true
      orchestrator_mysql_compatible_version:
      orchestrator_listen_address: :3000
      orchestrator_cluster_name_to_alias: { .*: default }
      orchestrator_recover_master_cluster_filters: ["*"]
      orchestrator_recover_intermediate_master_cluster_filters: ["*"]
      orchestrator_master_failover_lost_instances_downtime_minutes: 1
      orchestrator_failure_detection_period_block_minutes: 1
      orchestrator_recovery_period_block_seconds: 30
      orchestrator_detect_cluster_alias_query:
      orchestrator_detect_cluster_domain_query:
      orchestrator_detect_datacenter_query:
      orchestrator_replication_lag_query:
      orchestrator_pseudo_gtid_pattern:
      orchestrator_detect_pseudo_gtid_query:
      orchestrator_authentication_method:
      orchestrator_http_auth_user:
      orchestrator_http_auth_password:
      orchestrator_url_prefix:
```
