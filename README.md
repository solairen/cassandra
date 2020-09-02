### Precondition

(Virtual-)Machine with CentOS/Debian/Ubuntu system installed.

### About cassandra_ansible

This ansible scirpts will install java and cassandra on machines with operating system:
  * Centos 7/8
  * Ubuntu 18.04/20.04
  * Debian 9/10<br/>

If more than one IP addresses will be provider, it will combine all cassandra's into ring.<br/>
On the last step, this script will open all necessary firewall ports.

### How to use cassandra_ansible

Open ***config.vars*** and enter desired values

```vars
_CLUSTER_NAME: Test Cluster
_SEEDS: "{{ groups['cassandra'] | join(',') }}"
_LISTEN_ADDRESS: "{{ inventory_hostname }}"
_BROADCAST_ADDRESS: "{{ inventory_hostname }}"
_START_RPC: true
_RPC_ADDRESS: 0.0.0.0
_DATA_DIR: /var/lib/cassandra/data
_HINTS_DIR: /var/lib/cassandra/hints
_COMMITLOG_DIR: /var/lib/cassandra/commitlog
_SAVE_CACHE_DIR: /var/lib/cassandra/saved_caches
_PARTITIONER: org.apache.cassandra.dht.Murmur3Partitioner
_ENABLE_MATERIALIZED_VIEWS: false
_ENDPOINT_SNITCH: GossipingPropertyFileSnitch
_JMX_PORT: 8090
_DMX_ADDRESS: 0.0.0.0
_DMX_PORT: 8091
```
In ***inventory.yml*** add IP address(es) to (virtual-)machines

> to install on one (virtual-)machine
```yaml
all:
  children:
      linux:
        vars:
          ansible_user: username
          ansible_ssh_pass: password
        children:
          cassandra:
            hosts:
              127.0.0.1:
```

> to install on two and more (virtual-)machines
```yaml
all:
  children:
      linux:
        vars:
          ansible_user: username
          ansible_ssh_pass: password
        children:
          cassandra:
            hosts:
              127.0.0.1:
              127.0.0.2:
              (...)
```

* vars:<br/>
    * ansible_user: enter username<br/>
    * ansible_password: enter password

#### To start cassandra_ansible

From CMD type: ansible-playbook -i inventory.yml install.yml --ask-become-pass