# Packer Image Builder for RHEL Family 7

## Build CentOS 7 Cloud Image

```bash
# start the installation
packer build -only=centos-7-cloud-kvm rhel7.json

# shrink the image size
qemu-img convert -c -f qcow2 -O qcow2 -o cluster_size=2M output-centos-7-cloud-kvm/packer-centos-7-cloud-kvm.qcow2 output-centos-7-cloud-kvm/packer-centos-7-cloud-kvm.compressed.qcow2

# upload the image to open stack
glance image-create --name "CentOS 7" --container-format ovf --disk-format qcow2 --file output-centos-7-cloud-kvm/packer-centos-7-cloud-kvm.compressed.qcow2 --is-public True --progress
```


## Meta Data Server

To ensure cloud-init works properly you need to ensure that cloud-init is able to reach the metadata server:

route add 169.254.169.254 mask 255.255.255.255 <router-ip>