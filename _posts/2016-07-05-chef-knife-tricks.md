---
layout: post
title: "Chef Knife Tricks"
excerpt: The knife tool in The Chef Development Kit is a great time saver. Here are some useful tricks which I have gleaned from various sources.
category: [Chef]
comments: true
---


The knife tool in [The Chef Development Kit](https://downloads.chef.io/chefdk "Chef Development Kit Download") is a great time saver. Here are some useful tricks which I have gleaned from various sources and put in one place for reference.

## Find out when chef-client last updated a node
```bash
knife status

4 minutes ago, test2-inf-mongo-01, centos 6.6.
4 minutes ago, test-hyb-man-01, centos 6.6.
4 minutes ago, stg-hyb-sol-02, centos 6.6.
4 minutes ago, test2-node-api-02, centos 6.6.
4 minutes ago, test-inf-redis-02, centos 6.6.
4 minutes ago, test2-inf-mongo-03, centos 6.6.
3 minutes ago, stg-inf-redis-02, centos 6.6.
3 minutes ago, test2-hyb-solr-01, centos 6.6.
3 minutes ago, con-inf-db-01, centos 6.6.
2 minutes ago, uat-inf-mongo-01, centos 6.6.
1 minute  ago, stg-inf-mongo-02, centos 6.6.
1 minute  ago, stg-inf-gfs-01, centos 6.6.
0 minutes ago, uat-inf-db-06, centos 6.6.
0 minutes ago, solopt-node-mgmt-01, centos 6.7.
0 minutes ago, stg-inf-db-03, centos 6.6.

```

## Find all nodes in an environment
```bash
knife search node chef_environment:<environment name>
```

## Find all nodes which contain a role
```bash
knife search node role(s):<role name>

knife search node "role:web_frontend" -a hostname
```

You should use 'roles' plural when looking in the expanded run list.

## Find all nodes which contain a recipe
```bash
# Looks for elkstack::default
knife search node "recipes:elk_stack"

#not the use of \:\ to escape the double colon
knife search node "recipe:windows\:\reboot_handler"
```

## Find all non 64-bit nodes
```bash
knife search node "(NOT kernel_machine:x86_64)"
```

## Return selected attributes from knife search
```bash
knife search node "name:*" -a chef_packages.chef.version
```
This returns the chef_packages.chef.version attribute from all nodes in the Chef Organisation.

## Add a role to all nodes in an environment
```bash
# First, run it like this to run without saving the results back to the Chef server.
knife exec -E 'nodes.transform("chef_environment:uat") {|n| puts n.run_list << "role[hosts_file]" }'

knife exec -E 'nodes.transform("Chef_environment:uat") {|n| puts n.run_list << "role[hosts_file]"; n.save}'
```

To be on the safe side, run the command above without the n.save so results aren't saved back to the Chef server. When you are sure
about the command, run it with n.save to save the results back to the Chef server.

## Add a role to all nodes in an Environment which does not contain the given role
```bash
knife exec -E 'nodes.find("chef_environment:uat") {|n| puts.n.run_list << "role[base]" unless n.run_list.include?("role[base]"); n.save }'
```
## Remove a recipe from all nodes in an Environment
```bash
knife exec -E 'nodes.transform("chef_environment:uat") {|n| puts n.run_list.remove("recipe[chef-client::upgrade]""); n.save }'
```

## Remove all nodes from a given role
```bash
knife exec -E 'nodes.find("role:web-frontend") {|n| n.run_list.remove("role(web-frontend)"); n.save }'
```

## Set a node's run_list back to a single item
```bash
knife exec -E 'nodes.transform("name:webserver-01.domain.com") {|n| n.run_list(["role[base]"])}'
```
