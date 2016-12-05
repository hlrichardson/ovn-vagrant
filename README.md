# ovn-vagrant
Basic vagrant setup for an OVN cluster

sudo yum install -y vagrant
sudo yum install -y vagrant-libvirt

sudo gpasswd -a ${USER} libvirt
newgrp libvirt

git clone https://github.com/hlrichardson/ovn-vagrant
cd ovn-vagrant

vagrant up --provider libvirt

vagrant ssh central
sudo ovn-sbctl show
exit

vagrant ssh compute1
sudo ovs-vsctl show
exit

vagrant ssh compute2
sudo ovs-vsctl show
exit

When done: vagrant destroy

See also:
    http://www.flaviof.com/blog2/post/main/just-ovn-nodes/
    http://blog.spinhirne.com/2016/09/a-primer-on-ovn.html

