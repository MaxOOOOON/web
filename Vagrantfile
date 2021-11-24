# -*- mode: ruby -*-
# vim: set ft=ruby :


MACHINES = {
   :web => {
        :box_name => "centos/8",
        :memory => 512,
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.248", virtualbox__intnet: "web"},
                ]
  },

}

Vagrant.configure("2") do |config|

    MACHINES.each do |boxname, boxconfig|

        config.vm.define boxname do |box|

            box.vm.box = boxconfig[:box_name]
            box.vm.host_name = boxname.to_s

            boxconfig[:net].each do |ipconf|
            box.vm.network "private_network", ipconf
            end
            box.vm.provider :virtualbox do |vb|
                    vb.customize ["modifyvm", :id, "--memory", "512"]
            end        
      
            box.vm.network "forwarded_port", guest: 80, host: 8080
            box.vm.network "forwarded_port", guest: 81, host: 8081
            box.vm.network "forwarded_port", guest: 82, host: 8082



            box.vm.provision :ansible do |ansible|
                ansible.playbook = "./playbook.yml"
                # ansible.verbose = true
              end
            
            
        end
    end  
end