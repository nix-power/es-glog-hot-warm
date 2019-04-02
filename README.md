AWS Elasticsearch cluster.

At my current position i was responsible to plan, to provision and to maintain graylog cluster which is capable of gathering of ~ 1Tb
logs daily from over two thousands physical servers, several hundreds AWS ec2 machines and from 3 large kubernetes clusters on AWS
in various regions.
Graylog is log aggregation software, which acts similar to logstash, but allowing all configuration to be made through the WebUI/RestAPI
It uses elasticsearch cluster as a backennd to store logs and MongoDB replica set to store its own configuration.

This part of code intended to cover mostly elasticsearch topics:
Provision databse node (my custom bash scripts or any other AWS compatible provisioning tool can be used, such as salt-cloud, terraform etc)
Salt-ssh configuration provisioning
Elasticsearch tweks for pproper shard allocation based on rack location and node type (hot/warm)
Installing and configuring curator for indices/shards housekeeping: shards that older than a week will be moved to "warm" nodes, 
(d2 aws instances) with lot of disk space, but using "slow" HDD disks, while newer ones will always allocate to "hot" nodes (i3 AWS instances)
with very fast and expensive NVME SSD disks.


Salt based provisioning of elasticsearch databse nodes for HOT-WARM scenario.
Including rack awarness allocation

First steps should be done:
1. Prepare elasrticsearch data nodes to segregate them by box_type and rack_id
2. Configure elasticsearch to be rack aware while allocating shards (primary and replica will never be asssigned to nodes in the same rack)
3. Configure elasticsearch to route all new inedices created by graylog to "hot" box_type nodes only
4. Install and configure curator
5. Automate curator running on daily basis

1. provision datanode
2. configuring datanode according to its name to be whether hot or warm and joining it to the cluseter
