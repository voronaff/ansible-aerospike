Aerospike community server
==========================


This is an Ansible role for Aerospike community server.


Requirements
------------

This role has been tested with Ansible 2.1 only. It's also supposed that
you are using the merge behaviour for variables (please, see about
[hash_behaviour=merge](http://docs.ansible.com/ansible/intro_configuration.html#hash-behaviour)
for details)


Role Variables
--------------

This role declares and uses the configurations variables in a hash under the
_aerospike_ key (besides a variable aerospike_version). This is a description 
for main variables which you may want to change.


  * _aerospike_version_ is a desired version of collectd agent or plugins;

  * _aerospike.config_ is a section to declare plugin settings which you want
    to use in your environment.

    See examples below how to declare different parts of aerospike configuration in YAML
    format.

Dependencies
------------

This role doesn't depend from other ansible roles


Example Playbook
----------------

An example of how to use collectd agent:

        - hosts: all
          roles:
            - role: aerospike
              aerospike_version: 3.9.0
              aerospike:
                config:
                  # It's a default configuration from aerospike
                  service:
                    paxos-single-replica-limit: 1 # Number of nodes where the replica count is automatically reduced to 1
                    service-threads: 4
                    transaction-queues: 4
                    transaction-threads-per-queue: 4
                    proto-fd-max: 15000
              
                  logging:
                    file /var/log/aerospike/aerospike.log:
                      context: "any info"
                    console:
                      context: "any info"
              
                  network:
                    service:
                      address: any
                      port: 3000
              
                    heartbeat:
                      mode: multicast
                      address: 239.1.99.222
                      port: 9918
                      interval: 150
                      timeout: 10
                      mesh-seed-address-port:
                        - "some host 9000"
                        - "some host 8888"
              
                    fabric:
                      port: 3001
              
                    info:
                      port: 3003
              
                  namespace test:
                    replication-factor: 2
                    memory-size: 4G
                    default-ttl: 30d # 30 days, use 0 to never expire/evict.
                    storage-engine: memory
              
                  namespace bar:
                    replication-factor: 2
                    memory-size: 4G
                    default-ttl: 30d # 30 days, use 0 to never expire/evict.
                    storage-engine: memory

License
-------

MIT


Author Information
------------------

Alexey Voronov <vorona84@gmail.com>
