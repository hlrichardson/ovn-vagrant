box              = "centos/7"
provider_netmask = "255.255.0.0"
ovn_repo         = "https://github.com/openvswitch/ovs.git"
ovn_branch       = "master"
ovn_pkg_dir      = "/vagrant/pkgs"

ovn_cfg = [
    {
     :name => "central",
     :autostart => true,
     :host_name  => "central.ovn.dev",
     :ip => "192.168.33.11",
     :prov_ip => "10.10.0.11",
     :memory => 2048,
     :cpus => 2,
     :mtu => 1500,
   },{
     :name => "compute1",
     :autostart => true,
     :host_name  => "compute1.ovn.dev",
     :ip => "192.168.33.31",
     :prov_ip => "10.10.0.31",
     :memory => 512,
     :cpus => 2,
     :mtu => 1500,
   },{
     :name => "compute2",
     :autostart => true,
     :host_name  => "compute2.ovn.dev",
     :ip => "192.168.33.32",
     :prov_ip => "10.10.0.32",
     :memory => 512,
     :cpus => 1,
     :mtu => 1500,
   },{
     :name => "compute3",
     :autostart => false,
     :host_name  => "compute3.ovn.dev",
     :ip => "192.168.33.33",
     :prov_ip => "10.10.0.33",
     :memory => 512,
     :cpus => 1,
     :mtu => 1500,
   },
]

Vagrant.configure("2") do |config|
  config.vm.box = box
  config.vm.box_check_update = false

  config.ssh.insert_key = false
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'" # avoids 'stdin: is not a tty' error.

  ovn_cfg.each do |server|
    if server[:name] == "central"
        is_primary = true
    else
        is_primary = false
    end
    config.vm.define server[:name], primary: is_primary, autostart: server[:autostart] do |node|

      ovn_cfg.each do |entry|
          node.vm.provision "shell", inline: %Q(sudo sh -c "echo \\\"#{entry[:ip]} #{entry[:name]}\\\" >> /etc/hosts")
      end

      node.vm.hostname = server[:host_name]
      node.vm.network :private_network, ip: server[:ip]
      node.vm.network :private_network,
                      :ip => server[:prov_ip],
                      :libvirt__netmask => provider_netmask,
                      :libvirt__forward_mode => "none"

      node.vm.provision "shell", inline: "sudo ip link set dev eth1 mtu #{server[:mtu]}"
      node.vm.provision "shell", inline: "sudo ip link set dev eth2 mtu #{server[:mtu]}"

      config.vm.provider "libvirt" do |domain|
        domain.memory = server[:memory]
        domain.cpus = server[:cpus]
      end

      node.vm.provision "shell", path: "build_ovn", :args=> "#{ovn_repo} #{ovn_branch}"

      if server[:name] == "central"
          node.vm.provision "shell", inline: "sudo systemctl enable ovn-northd"
          node.vm.provision "shell", inline: "sudo systemctl start ovn-northd"
      else # compute node
          node.vm.provision "shell", inline: "sudo systemctl enable ovn-controller"
          node.vm.provision "shell", inline: "sudo systemctl start ovn-controller"
          node.vm.provision "shell", path: "setup_controller"
      end
    end
  end
end
