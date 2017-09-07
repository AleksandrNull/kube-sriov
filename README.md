# SR-IOV CNI plugin for k8s
This repo contains a .yaml file with definitions of network for k8s using SR-IOV functionality of a modern NICs.
Tested with Mellanox and Intel nics which suports SR-IOV functionality 

# Sources
Container build based on:

https://github.com/containernetworking/plugins

https://github.com/containernetworking/cni

https://github.com/hustcat/sriov-cni

# Usage
Modify sriov.yaml files for your own network usecases. Allowed parameters:

* `name` (string, required): the name of the network
* `type` (string, required): "sriov"
* `master` (string, required): name of the PF
* `vf` (int, optional): VF index, default value is 0
* `vlan` (int, optional): VLAN ID for VF device
* `mac` (string, optional): mac address for VF device
* `ipam` (dictionary, required): IPAM configuration to be used for this network.

By default it uses daemonset which will spawn container with binaries of sriov, cni, fixipam and other cni tools 
across all the worker nodes with tag net=sriov.

