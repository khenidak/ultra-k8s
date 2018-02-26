# 1 - 100 Nodes

This example is based on acs-engine's [large clusters](https://github.com/Azure/acs-engine/tree/master/examples/largeclusters)


## Versions Supported

1.7.x++

## Special Configuration

1. etcd disk is `2047` to get the [highest disk i/o](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/premium-storage)
2. Network policy is set to `none` because Azure CNI reserves IPs per each node NIC and you will quickly go over `4096` ip per VNET limit 
3. `--route-reconciliation-period` is set to `3m` to limit the # of `GET` calls the controller execites against ARM endpoints.

> every 3 mins a 300 GET call will be executed against ARM. [Max is 15000 GET per hour](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-request-limits)

## Impact

New nodes will be able to serve traffic after max(3m) of when they register with node controller. To be more specific, routing traffic to any pod hosted on new nodes will fail for 3m until route is created in VNET routing table. 

