### About
These ansible scripts will install Java and [cassandra](https://cassandra.apache.org/) on Ubuntu or Debian.</br>
If more than one IP address will be provided, it will combine all cassandra's into rings.

### Supported OS:
* Ubuntu 18.04/20.04
* Debian 9/10

### Prerequisites
* [Ansible](https://docs.ansible.com/ansible/latest/index.html)
* [jmespath plugin](https://pypi.org/project/jmespath/)

### Configuration

#### Firewall
On hosts where cassandra will be installed, **UFW** should be enabled and port `22` should be temporary added to the rule.

#### Inventory.yml
In `inventory.yml`, set **IP**, **user**, **password**, **ssh port** or **ssh_key** on where cassandra should be installed.</br>
If **ssh_key** is used, comment **password**.</br>
If **password** is used, comment **ssh_key**.</br>
```yml
linux:
    vars:
      ansible_ssh_user: user
      ansible_ssh_pass: password
      ansible_port: 22
      ansible_ssh_private_key_file: <path_to_key>
    children:
      cassandra:
        hosts:
          127.0.0.1:
```

#### Group_vars/all/common
In `group_vars/all/common`, set:

```txt
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

### How to run:
```bash
ansible-playbook -i inventory.yml install.yml --ask-become-pass -vv
```