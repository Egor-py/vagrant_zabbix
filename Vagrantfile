Vagrant.configure("2") do |config|
  
  config.vm.define "mon_server" do |mon_server|
    mon_server.vm.box = "ubuntu/focal64"
    mon_server.vm.hostname = "zserver"
    mon_server.vm.network "private_network", ip: "192.168.56.100"
    mon_server.vm.network "forwarded_port", guest: 80, host: 24012
    mon_server.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--nic3", "intnet"]
    end
    mon_server.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/#g' /etc/ssh/sshd_config
      service ssh restart
    SHELL
    mon_server.vm.provision "file", source: "zabbix_key", destination: "/home/vagrant/.ssh/"
    mon_server.vm.provision "file", source: "zabbix_key.pub", destination: "/home/vagrant/.ssh/"
    mon_server.vm.provision "file", source: "netconf/zabbix_server/50-vagrant.yaml", destination: "/home/vagrant/"
    mon_server.vm.provision "shell", inline: <<-SHELL
      sudo apt update && sudo apt --assume-yes install ansible
      sudo sysctl -w net.ipv4.ip_forward=1
      sudo sysctl -p 
      chmod 600 /home/vagrant/.ssh/zabbix_key
      chmod 600 /home/vagrant/.ssh/zabbix_key.pub
      sudo cp 50-vagrant.yaml /etc/netplan/
      sudo netplan generate
      sudo netplan apply
    SHELL
    mon_server.vm.provision "file", source: "./ansible", destination: "/home/vagrant/"
  end

  config.vm.define "icinga_server" do |icinga_server|
    icinga_server.vm.box = "ubuntu/focal64"
    icinga_server.vm.hostname = "iciserver"
    icinga_server.vm.network "forwarded_port", guest: 80, host: 24021
    icinga_server.vm.network "private_network", ip: "192.168.56.101"
    icinga_server.vm.provider "virtualbox" do |vb|
      vb.cpus = "1"
      vb.memory = "1024"
    end
    icinga_server.vm.provision "file", source: "zabbix_key", destination: "/home/vagrant/.ssh/"
    icinga_server.vm.provision "file", source: "zabbix_key.pub", destination: "/home/vagrant/.ssh/"
    icinga_server.vm.provision "shell", inline: <<-SHELL
      sudo apt update && sudo apt --assume-yes install ansible
      sudo sysctl -w net.ipv4.ip_forward=1
      sudo sysctl -p 
      chmod 600 /home/vagrant/.ssh/zabbix_key
      chmod 600 /home/vagrant/.ssh/zabbix_key.pub
    SHELL
    icinga_server.vm.provision "file", source: "./ansible", destination: "/home/vagrant/"
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
    h1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    h1.vm.provision "file", source: "netconf/H1/network", destination: "/etc/config/"
    h1.vm.provision "shell", inline: <<-SHELL
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys
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
    h2.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    h2.vm.provision "file", source: "netconf/H2/network", destination: "/etc/config/"
    h2.vm.provision "shell", inline: <<-SHELL
      /etc/init.d/network restart
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys
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
    h3.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    h3.vm.provision "file", source: "netconf/H3/network", destination: "/etc/config/"
    h3.vm.provision "shell", inline: <<-SHELL
      /etc/init.d/network restart
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys
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
    h4.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    h4.vm.provision "file", source: "netconf/H4/network", destination: "/etc/config/"
    h4.vm.provision "shell", inline: <<-SHELL
      /etc/init.d/network restart
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys
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
    r1.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    r1.vm.provision "file", source: "netconf/R1/network", destination: "/etc/config/"
    r1.vm.provision "shell", inline: <<-SHELL
      /etc/init.d/network restart
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys

      uci set firewall.@defaults[-1].forward='ACCEPT'
      uci commit firewall
      /etc/init.d/firewall restart
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
    r2.vm.provision "file", source: "zabbix_key.pub", destination: "/root/"
    r2.vm.provision "file", source: "netconf/R2/network", destination: "/etc/config/"
    r2.vm.provision "shell", inline: <<-SHELL
      /etc/init.d/network restart
      sudo opkg update
      sudo opkg install python3
      chmod 600 /root/zabbix_key.pub
      cat /root/zabbix_key.pub >> /etc/dropbear/authorized_keys

      uci set firewall.@defaults[-1].forward='ACCEPT'
      uci commit firewall
      /etc/init.d/firewall restart
    SHELL
  end 

end
