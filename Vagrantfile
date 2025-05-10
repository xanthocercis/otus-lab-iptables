# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :inetRouter => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "inetRouter",
        :net => [
                   {ip: "192.168.255.1", adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: "192.168.50.10", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :inetRouter2 => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "inetRouter2",
        :net => [
                   {ip: "192.168.255.3", adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router2-net"},
                   {ip: "192.168.56.10", adapter: 3, netmask: "255.255.255.0", type: "hostonly"},
                   {ip: "192.168.50.13", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :centralRouter => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "centralRouter",
        :net => [
                   {ip: "192.168.255.2", adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: "192.168.255.4", adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "router2-net"},
                   {ip: "192.168.0.1", adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: "192.168.0.33", adapter: 5, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
                   {ip: "192.168.0.65", adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "mgt-net"},
                   {ip: "192.168.255.9", adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {ip: "192.168.255.5", adapter: 8, netmask: "255.255.255.252", virtualbox__intnet: "office2-central"},
                   {ip: "192.168.50.11", adapter: 9, netmask: "255.255.255.0"},
                ]
  },
  :centralServer => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "centralServer",
        :net => [
                   {ip: "192.168.0.2", adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: "192.168.50.12", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :office1Router => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "office1Router",
        :net => [
                   {ip: "192.168.255.10", adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
                   {ip: "192.168.2.1", adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "dev1-net"},
                   {ip: "192.168.2.65", adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "test1-net"},
                   {ip: "192.168.2.129", adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
                   {ip: "192.168.2.193", adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "office1-net"},
                   {ip: "192.168.50.20", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :office1Server => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "office1Server",
        :net => [
                   {ip: "192.168.2.130", adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
                   {ip: "192.168.50.21", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :office2Router => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "office2Router",
        :net => [
                   {ip: "192.168.255.6", adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office2-central"},
                   {ip: "192.168.1.1", adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "dev2-net"},
                   {ip: "192.168.1.129", adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "test2-net"},
                   {ip: "192.168.1.193", adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "office2-net"},
                   {ip: "192.168.50.30", adapter: 8, netmask: "255.255.255.0"},
                ]
  },
  :office2Server => {
        :box_name => "generic/ubuntu2204",
        :vm_name => "office2Server",
        :net => [
                   {ip: "192.168.1.2", adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev2-net"},
                   {ip: "192.168.50.31", adapter: 8, netmask: "255.255.255.0"},
                ]
  }
}

Vagrant.configure("2") do |config|
  config.vm.define "inetRouter" do |box|
    box.vm.box = MACHINES[:inetRouter][:box_name]
    box.vm.host_name = MACHINES[:inetRouter][:vm_name]
    box.vm.provider "virtualbox" do |v|
      v.memory = 768
      v.cpus = 1
    end
    MACHINES[:inetRouter][:net].each do |ipconf|
      box.vm.network "private_network", ip: ipconf[:ip], adapter: ipconf[:adapter], netmask: ipconf[:netmask], virtualbox__intnet: ipconf[:virtualbox__intnet]
    end
    box.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh
      cp ~vagrant/.ssh/auth* ~root/.ssh
      apt-get update
      apt-get install -y traceroute
    SHELL
  end

  config.vm.define "inetRouter2" do |box|
    box.vm.box = MACHINES[:inetRouter2][:box_name]
    box.vm.host_name = MACHINES[:inetRouter2][:vm_name]
    box.vm.provider "virtualbox" do |v|
      v.memory = 768
      v.cpus = 1
    end
    MACHINES[:inetRouter2][:net].each do |ipconf|
      if ipconf[:type] == "hostonly"
        box.vm.network "private_network", ip: ipconf[:ip], adapter: ipconf[:adapter], netmask: ipconf[:netmask], type: "dhcp"
      else
        box.vm.network "private_network", ip: ipconf[:ip], adapter: ipconf[:adapter], netmask: ipconf[:netmask], virtualbox__intnet: ipconf[:virtualbox__intnet]
      end
    end
    box.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh
      cp ~vagrant/.ssh/auth* ~root/.ssh
      apt-get update
      apt-get install -y traceroute
    SHELL
  end

  [:centralRouter, :centralServer, :office1Router, :office1Server, :office2Router, :office2Server].each do |boxname|
    config.vm.define boxname do |box|
      box.vm.box = MACHINES[boxname][:box_name]
      box.vm.host_name = MACHINES[boxname][:vm_name]
      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
      end
      MACHINES[boxname][:net].each do |ipconf|
        box.vm.network "private_network", ip: ipconf[:ip], adapter: ipconf[:adapter], netmask: ipconf[:netmask], virtualbox__intnet: ipconf[:virtualbox__intnet]
      end
      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
        apt-get update
        apt-get install -y traceroute
      SHELL
      if boxname.to_s == "office2Server"
        box.vm.provision "ansible" do |ansible|
          ansible.playbook = "ansible/provision.yml"
          ansible.inventory_path = "ansible/hosts"
          ansible.host_key_checking = false
          ansible.limit = "all"
        end
      end
    end
  end
end