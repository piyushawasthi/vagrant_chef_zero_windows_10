$devenv = <<'DEVENV'
invoke-restmethod -uri https://raw.githubusercontent.com/piyushawasthi/vagrant_chef_zero_windows_10/master/install.ps1 -Outfile c:\users\vagrant\desktop\install.ps1
DEVENV

Vagrant.configure(2) do |config|
  config.vm.define "w10"
  config.vm.box = "jacqinthebox/windows10LTSB"
  config.vm.hostname = "w10"
  config.vm.communicator = "winrm"
  config.vm.guest = :windows
  config.winrm.username = "vagrant"
  config.winrm.password = "vagrant"
  config.vm.network "forwarded_port", guest: 3389, host: 13389, auto_correct: true
  config.vm.network "forwarded_port", guest: 5985, host: 15985, id: "winrm", auto_correct:true
  config.vm.network "private_network", ip: "192.168.56.60"
  
  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
  end

 config.vm.provision :shell, :inline => $devenv

  config.vm.provision "chef_zero" do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.nodes_path = "nodes"
    
    chef.add_recipe "apache"
    chef.add_recipe "php-windows"

    chef.add_role "web" 
  end
end