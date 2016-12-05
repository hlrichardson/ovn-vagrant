# ovn-vagrant
Basic vagrant setup for an OVN cluster

Install vagrant and libvirt provider:

```bash
sudo yum install -y vagrant
sudo yum install -y vagrant-libvirt
```

Avoid having to enter credentials every time vagrant does
something with libvirt:

```bash
sudo gpasswd -a ${USER} libvirt
newgrp libvirt
```

Checkout this repository.

```bash
git clone https://github.com/hlrichardson/ovn-vagrant
cd ovn-vagrant
```

Create CentOS VMs (OVN central and two OVN compute nodes)

```bash
vagrant up --provider libvirt
```

Look around in central node:
```bash
vagrant ssh central
sudo ovn-sbctl show
exit
```

Look around in compute1 node:
```bash
vagrant ssh compute1
sudo ovs-vsctl show
exit
```

Look around in compute2 node:
```bash
vagrant ssh compute2
sudo ovs-vsctl show
exit
```

When done, clean up:

```bash
vagrant destroy
```

See also:
* http://www.flaviof.com/blog2/post/main/just-ovn-nodes/
* http://blog.spinhirne.com/2016/09/a-primer-on-ovn.html

