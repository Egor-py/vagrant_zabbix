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
    zabbix_host_ubuntu.vm.network "public_network", ip: "192.168.0.201"
    zabbix_host_ubuntu.vm.provision "shell", 
      run: "always", 
      inline: "route add default gw 192.168.0.1"
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

  config.vm.define "h1" do |h1|
    h1.vm.box = "vladimir-babichev/openwrt-21.02"
    h1.vm.hostname = "H1"
    h1.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "nat"]
    end
    h1.vm.provision "shell", inline: <<-SHELL
      uci set network.lan=interface
      uci set network.lan.type=bridge
      uci set network.lan.proto=static
      uci set network.lan.ipaddr=10.0.11.2
      uci set network.lan.netmask=255.255.255.0
      uci set network.lan.device=eth1
      uci commit network
    SHELL
    h1.vm.provision "shell", inline: <<-SHELL
      uci add network route
      uci set network.@route[-1].interface=lan
      uci set network.@route[-1].target=10.0.0.0/8
      uci set network.@route[-1].gateway=10.0.11.1
      uci commit network
    SHELL
    h1.vm.network "forwarded_port", guest: 80, host: 24015
    h1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart
    SHELL
  end

  config.vm.define "r1" do |r1|
    r1.vm.box = "vladimir-babichev/openwrt-21.02"
    r1.vm.hostname = "R1"
    r1.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "nat"]
    end
    r1.vm.provision "shell", inline: <<-SHELL
      uci set network.lan=interface
      uci set network.lan.type=bridge
      uci set network.lan.proto=static
      uci set network.lan.ipaddr=10.0.11.1
      uci set network.lan.netmask=255.255.255.0
      uci set network.lan.device=eth1
      uci commit network
    SHELL
    #r1.vm.provision "shell", inline: <<-SHELL
    #  uci add network route
    #  uci set network.@route[-1].interface=lan
    #  uci set network.@route[-1].target=10.0.0.0/8
    #  uci set network.@route[-1].gateway=10.0.11.1
    #  uci commit network
    #SHELL
    r1.vm.network "forwarded_port", guest: 80, host: 24016
    r1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    r1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart
    SHELL
  end


end
