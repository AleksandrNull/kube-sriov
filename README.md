# SR-IOV CNI plugin for k8s
This repo contains a .yaml file with definitions of network for k8s using SR-IOV functionality of a modern NICs.
Tested with Mellanox and Intel nics with suport of SR-IOV functionality 

# Sources
Container build based on:

https://github.com/containernetworking/plugins

https://github.com/containernetworking/cni

https://github.com/hustcat/sriov-cni

# Before you start

You should have your worker nodes configured with support on VT-d and SR-IOV. NIC kernel modules should also be booted up with number of VFs configured.

Here is some examples for different NICs:

Ubuntu Linux kernel parameters:
```
vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="splash quiet intel_iommu=on iommu=pt"
update-grub
```

Intel:
```
# vi /etc/modprobe.conf
options ixgbe max_vfs=8,8
```

Mellanox:
```
vi /etc/modprobe.conf
options mlx4_core num_vfs=16 port_type_array=2,2 probe_vf=16
```

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

After .yaml file prep you just apply it on kubernetes cluster using the following command:

* kubeclt create -f sriov.yaml

# Known limitations

This CNI plugin assumes that you have your networking infrastructure ready (with configured VLANs, routers, etc) and all containers would work with it.

kube-dns wouldn't work a usual way so you have to configure routing from your tenants networks to the kubernetes control plane network to allow kube-dns retrieve information.

It also possible to use this plugin with Intel Multus CNI to go with multiple interfaces per container but it also involves additional effort and infrastructure preps.




