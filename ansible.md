# Ansible Documentation for Elasticsearch Cluster(master and data node), Kibna and Fluentbit 

Ansible roles for 6.x/ Elasticsearch, Kibana and Fluentbit.  Currently this works on Debian and RedHat based linux systems. Tested platforms are:

* Ubuntu 16.04
* Ubuntu 18.04
* Debian 8
* CentOS 7
* Redhat 7

# Pre-Requisites:
 1. Kibana server: Kibana is an open source data visualization plugin for Elasticsearch. It provides visualization capabilities on top of the content indexed on an Elasticsearch cluster. Users can create bar, line and scatter plots, or pie charts and maps on top of large volumes of data.
    Recommended Settings- 
        Ram: 4GB
        Disk: 30 GB
        CPU: 2 Cores
 
 2. Elasticsearch Cluster: 
        Ram: 4 GB
        Disk: 32 GB
        CPU: 2 Cores
        server: one master and one data node (for best practices three master node and two data node)    
 3. Fluentbit: Fluentbit agent does not consume much system resources and it is very light weight.
    
# Steps For Execution:
### Dependencies

To run the playbook the target servers must have python installed. Generally, flavours of debian and fedora comes with a version of python installed. 
To run the ansible playbook, user needs to clone this below repository url as below:

    git clone -b ansible --single-branch https://ramesh.chand@gitlab.intelligrape.net/devops/hawk.git

change to the directory where playbook, inventory and input variable yaml file are kept
    
    cd hawk/EFK/
    
### Understanding the inventory file

IP addresses along with the secret pem key file's location of the hosts is needed to be mentioned in the inventory file on which the ansible will run the playbook to configure these agents.

This needs be in below format where IP address will be mentioned in place of <IP ADDRESS> and the location of pem file will be provided.
```
[host]

<IP ADDRESS> ansible_user=ubuntu ansible_ssh_private_key_file=/home/user/Downloads/ttn.pem ansible_connection=ssh
```

### Running the playbook

To run the playbook user needs to use the below command once he is inside the TIG dir
 ```
 ansible-playbook main.yaml -i inventory.yaml
 ```
 <p>Where inventory.yaml keeps the target inventory list. </p> 
 


# Roles definition for Elasticsearch Cluster

An Ansible role to setup Elasticsearch cluster
(master/data node)

Elasticsearch cluster va

* ```es_config['cluster.name']``` - define the elastic cluster name
* ```es_config['transport.tcp.port']``` - the transport port for the node
* ```es_config['discovery.zen.ping.unicast.hosts']``` - the unicast discovery list, in the comma separated format ```"<host>:<port>,<host>:<port>"``` (typically the clusters dedicated masters)
* ```es_config['network.host']``` - sets both network.bind_host and network.publish_host to the same host value. The network.bind_host setting allows to control the host different network components will bind on.  
* ```es_node_data``` and ```es_node_data``` - if want setup a master node in elasticsearch cluster the we define es_node_master as true and es_node_data as false and if want setup a data node in elasticsearch cluster the we define es_node_master as false and es_node_data as true.
* ```es_max_map_count``` maximum number of VMA (Virtual Memory Areas) a process can own. Defaults to 262144.
* ```es_max_open_files``` the maximum file descriptor number that can be opened by this process. Defaults to 65536.
* ```es_instance_name``` define the node name(master/data)
* ```es_heap_size``` define JVM heap size value.
* ```es_plugins``` an array of plugin definitions e.g.:
  ```yaml
    es_plugins:
      - plugin: ingest-geoip 
* ```es_pid_dir``` - defaults to "/var/run/elasticsearch".
* ```es_data_dirs``` - defaults to "/var/lib/elasticsearch"
* ```es_log_dir``` - defaults to "/var/log/elasticsearch"



# Roles definition for Kibana

An Ansible role to setup kibana.

Kibana variables:

* ```kibana_elasticsearch_url``` define the elastic search master endpoint address.


# Roles definition for Fluentbit

An Ansible role to setup kibana.

Kibana variables:

* ```fluentbit_inputs and fluentbit_outputs``` define input and output variable .Find below URL for Input and output pulgin vaibales
* ``` https://fluentbit.io/documentation/current/output/ and https://fluentbit.io/documentation/current/input ``` .

-------------------------------------------------------------------------------

All the variables for driving different configuration wrt telelgraf, influxdb and Grafana are stored in input.yaml which can be easily over-ridden by simply changing the variables to custom value.

```
Variables file: hawk/EFK/input.yaml

Inventory File: hawk/EFK/inventory.yaml

Main yaml File: hawk/EFK/main.yaml

```
