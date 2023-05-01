# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
 :ns01 => {
        :box_name => "centos/7",
        :vm_name => "ns01",
        :net => [
                   {ip: '192.168.50.10', adapter: 2, netmask: "255.255.255.0"}
               ]
  },
  
 :ns02 => {
        :box_name => "centos/7",
        :vm_name => "ns02",
        :net => [
                   {ip: '192.168.50.11', adapter: 2, netmask: "255.255.255.0"}
               ]
  },

 :client => {
        :box_name => "centos/7",
        :vm_name => "client",
        :net => [
                   {ip: '192.168.50.15', adapter: 2, netmask: "255.255.255.0"}
               ]
  },
 :client2 => {
        :box_name => "centos/7",
        :vm_name => "client2",
        :net => [
                   {ip: '192.168.50.16', adapter: 2, netmask: "255.255.255.0"}
               ]
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

       config.vm.define boxname do |box|
       box.vm.box = boxconfig[:box_name]
       box.vm.host_name = boxconfig[:vm_name]
       
              if boxconfig[:vm_name] == "client2"
                 box.vm.provision "ansible" do |ansible|
                  ansible.playbook = "playbook.yml"
                  ansible.host_key_checking = "false"
                  ansible.limit = "all"
               end
                         
          end
               
       boxconfig[:net].each do |ipconf|
       box.vm.network "private_network", ipconf
       
      end
               
          box.vm.provision "shell", inline: <<-SHELL
             mkdir -p ~root/.ssh
              cp ~vagrant/.ssh/auth* ~root/.ssh
             SHELL
     end      
   end
end
