Vagrant.configure("2") do |config|
  
  config.vm.define "zabbix_server" do |zabbix_server|
    zabbix_server.vm.box = "ubuntu/focal64"
    zabbix_server.vm.hostname = "zserver"
    zabbix_server.vm.network "private_network", ip: "192.168.56.40"
    zabbix_server.vm.network "forwarded_port", guest: 80, host: 24012
    zabbix_server.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "1024"
    end
    zabbix_server.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
    SHELL
    config.vm.provision "file", source: "zabbix_key", destination: "/home/vagrant/.ssh/"
    config.vm.provision "file", source: "zabbix_key.pub", destination: "/home/vagrant/.ssh/"
    zabbix_server.vm.provision "shell", inline: <<-SHELL
      sudo apt update && sudo apt --assume-yes install ansible
      chmod 600 /home/vagrant/.ssh/zabbix_key
      chmod 600 /home/vagrant/.ssh/zabbix_key.pub
    SHELL
    zabbix_server.vm.provision "file", source: "./ansible", destination: "/home/vagrant/"
  end

  config.vm.define "zabbix_host_ubuntu" do |zabbix_host_ubuntu|
    zabbix_host_ubuntu.vm.box = "ubuntu/focal64"
    zabbix_host_ubuntu.vm.hostname = "zhostu"
    zabbix_host_ubuntu.vm.network "private_network", ip: "192.168.56.41"
    zabbix_host_ubuntu.vm.network "forwarded_port", guest: 80, host: 1234
    zabbix_host_ubuntu.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "1024"
    end
    config.vm.provision "file", source: "zabbix_key.pub", destination: "/home/vagrant/.ssh/"
    zabbix_host_ubuntu.vm.provision "shell", inline: <<-SHELL
      chmod 600 /home/vagrant/.ssh/zabbix_key.pub
      cat /home/vagrant/.ssh/zabbix_key.pub >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "zabbix_host_openwrt" do |zabbix_host_openwrt|
    zabbix_host_openwrt.vm.box = "vladimir-babichev/openwrt-21.02"
    zabbix_host_openwrt.vm.hostname = "H1"
    zabbix_host_openwrt.vm.network "private_network", ip: "192.168.56.42"
    zabbix_host_openwrt.vm.network "forwarded_port", guest: 80, host: 24015
    zabbix_host_openwrt.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
    end
    zabbix_host_openwrt.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    zabbix_host_openwrt.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
    SHELL
  end

  config.vm.define "H1" do |h1|
    h1.vm.box = "openwrt/x86_64"
    h1.hostname = "H1"
    h1.vm.network "private_network", ip: "192.168.56.42"
    h1.vm.network "forwarded_port", guest: 80, host: 24016
    h1.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
    end
    h1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
    SHELL




  end

end
