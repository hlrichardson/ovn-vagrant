# ovn-vagrant
Basic vagrant setup for an OVN cluster

```bash
sudo yum install -y vagrant
sudo yum install -y vagrant-libvirt
```

```bash
sudo gpasswd -a ${USER} libvirt
newgrp libvirt
```

```bash
git clone https://github.com/hlrichardson/ovn-vagrant
cd ovn-vagrant
```

```bash
vagrant up --provider libvirt
```

```bash
vagrant ssh central
sudo ovn-sbctl show
exit
```

```bash
vagrant ssh compute1
sudo ovs-vsctl show
exit
```

```bash
vagrant ssh compute2
sudo ovs-vsctl show
exit
```

When done:

```bash
vagrant destroy
```

See also:
* http://www.flaviof.com/blog2/post/main/just-ovn-nodes/
* http://blog.spinhirne.com/2016/09/a-primer-on-ovn.html

