VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/centos-7.5"
  config.ssh.insert_key = false


    # configs for vagrant-hostmanager
    if Vagrant.has_plugin?("HostManager")
      config.hostmanager.enabled = true
      config.hostmanager.manage_host = true
    else
      raise "ERROR can't find vagrant-hostmanager! Please install it w/ vagrant plugin install vagrant-hostmanager"
    end
  
  config.vm.define "hadoopmaster" do |c|
    c.vm.hostname = "hadoopmaster"
    c.vm.network :private_network, ip: "192.168.50.11"
    c.vm.provider :virtualbox do |vb|
      vb.customize ['modifyvm', :id, '--memory', 1280]
      vb.customize ['modifyvm', :id, '--cpuexecutioncap', '70']
      vb.customize ['modifyvm', :id, '--cpus', 1]
      vb.customize ["modifyvm", :id, "--audio", "none"]

    end
  end

  config.vm.define "hadoopslave1" do |c|
    c.vm.hostname = "hadoopslave1"
    c.vm.network :private_network, ip: "192.168.50.12"
    c.vm.provider :virtualbox do |vb|
      vb.customize ['modifyvm', :id, '--memory', 1024]
      vb.customize ['modifyvm', :id, '--cpuexecutioncap', '50']
      vb.customize ['modifyvm', :id, '--cpus', 1]
      vb.customize ["modifyvm", :id, "--audio", "none"]

    end
  end
end
