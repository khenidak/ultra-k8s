
# What is this
Over the next couple of months, i will be putting together ultra scale kubernetes cluster (target 5000 nodes). This is relevant to you if:

1. Plan to use large clusters on Azure (50+ nodes)
2. Plan to create *one* large clutser in *one* azure subscription.
3. Plan to create *many* small clusters in *one* azure subscription.

# How-To-Use

* Directories named `x-y-nodes` contain validated configuration used to build the target count of nodes (count of nodes is masters+nodes). 
* Directories named `z-{config}` contain validated configuration for tool that will be used in one or more of the `x-y-nodes` configuration.


# Validation Process

1. Cluster creation process.
2. Cluster is healthy in idle state.
3. Cluster is healthy in active state (new apps/services/nodes coming in/out of the cluster).
4. Cluster is perfermant (apps, services, load balancer, nodes) are activated in resonable timeframe.
5. No throttling problems on Azure side.
