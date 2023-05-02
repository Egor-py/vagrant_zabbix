Vagrant.configure("2") do |config|
  
  config.vm.define "zabbix_server" do |zabbix_server|
    zabbix_server.vm.box = "ubuntu/focal64"
    zabbix_server.vm.hostname = "zserver"
    zabbix_server.vm.network "public_network", ip: "10.0.12.3", inline: "route add option gw 10.0.12.1"
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
      #net.ipv4.ip_forward=1
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
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    h1.vm.network "forwarded_port", guest: 80, host: 24015
    h1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h1.vm.provision "file", source: "netconf/H1/network", destination: "/etc/config/"
    h1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart
    SHELL
  end 

  config.vm.define "h2" do |h2|
    h2.vm.box = "vladimir-babichev/openwrt-21.02"
    h2.vm.hostname = "H2"
    h2.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    h2.vm.network "forwarded_port", guest: 80, host: 24016
    h2.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h2.vm.provision "file", source: "netconf/H2/network", destination: "/etc/config/"
    h2.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart
    SHELL
  end 

  config.vm.define "h3" do |h3|
    h3.vm.box = "vladimir-babichev/openwrt-21.02"
    h3.vm.hostname = "H3"
    h3.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    h3.vm.network "forwarded_port", guest: 80, host: 24017
    h3.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h3.vm.provision "file", source: "netconf/H3/network", destination: "/etc/config/"
    h3.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart
    SHELL
  end 

  config.vm.define "h4" do |h4|
    h4.vm.box = "vladimir-babichev/openwrt-21.02"
    h4.vm.hostname = "H4"
    h4.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    h4.vm.network "forwarded_port", guest: 80, host: 24018
    h4.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    h4.vm.provision "file", source: "netconf/H4/network", destination: "/etc/config/"
    h4.vm.provision "shell", inline: <<-SHELL
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
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    r1.vm.network "forwarded_port", guest: 80, host: 24019
    r1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    r1.vm.provision "file", source: "netconf/R1/network", destination: "/etc/config/"
    r1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart

      uci set firewall.@defaults[-1].forward='ACCEPT'
      uci commit firewall
      service firewall restart
    SHELL
  end

  config.vm.define "r2" do |r2|
    r2.vm.box = "vladimir-babichev/openwrt-21.02"
    r2.vm.hostname = "R2"
    r2.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "124"
      vb.customize ["modifyvm", :id, "--nic2", "intnet"]
    end
    r2.vm.network "forwarded_port", guest: 80, host: 24020
    r2.vm.provision "file", source: "zabbix_key.pub", destination: "/root/.ssh/"
    r2.vm.provision "file", source: "netconf/R2/network", destination: "/etc/config/"
    r2.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/.ssh/zabbix_key.pub
      cat /root/.ssh/zabbix_key.pub >> /root/.ssh/authorized_keys
      /etc/init.d/network restart

      uci set firewall.@defaults[-1].forward='ACCEPT'
      uci commit firewall
      service firewall restart
    SHELL
  end 


end
