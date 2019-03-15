MACHINES = {
  :runner => {
        :box_name => "centos/7",
        :ip_addr => '192.168.11.150',
        :ram => '512'
  },
  :gitlab => {
        :box_name => "centos/7",
        :ip_addr => '192.168.11.151',
        :ram => '2048'
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s


          box.vm.network "private_network", ip: boxconfig[:ip_addr]
          box.vm.provision :hosts, :sync_hosts => true
          

          box.vm.provider :virtualbox do |vb|
          vb.customize ["modifyvm", :id, "--memory", boxconfig[:ram]]
        
          end
          
          config.vm.provision :hosts do |provisioner|
            provisioner.add_host '192.168.11.151', ['gitlab']
            provisioner.add_host '192.168.11.150', ['runner']
           
        end
    
          
          box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh
            sed -i '65s/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
            systemctl restart sshd
          SHELL
        end
      end
        config.vm.define 'gitlab' do |machine|
        config.vm.network :forwarded_port, guest: 443, host: 4567
        
    machine.vm.provision "ansible_local" do |ansible|
    ansible.verbose = "vvv"
    ansible.install_mode = "default"
    ansible.version = "latest"
    ansible.playbook = "provisioning/playbook.yml"
    ansible.inventory_path = "provisioning/hosts"
    ansible.limit = "all"
    ansible.become = true
          end

      end
    end
    

  
    
    

 

