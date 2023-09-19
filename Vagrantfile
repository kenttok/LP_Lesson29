MACHINES = {
  :inetRouter => {
        :box_name => "centos/7",
        :vm_name => "inetRouter",
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"},
                ]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :vm_name => "centralRouter",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"},
                ]
  },
  :inetRouter2 => {
        :box_name => "centos/7",
        :vm_name => "inetRouter2",
        :net => [
                   {ip: '192.168.255.3', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "router-net"},
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
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
        case boxname.to_s
        when "inetRouter"
          box.vm.provision "shell", run: "always", inline: <<-SHELL
            sudo bash -c 'echo "net.ipv4.conf.all.forwarding=1" >> /etc/sysctl.conf'; sudo sysctl -p
            sudo yum install -y iptables-services; sudo systemctl enable iptables && sudo systemctl start iptables;
	    sudo iptables -F; sudo iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE;sudo iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT;sudo iptables -A INPUT -p icmp -j ACCEPT;sudo iptables -A INPUT -i lo -j ACCEPT;sudo iptables -A INPUT -i eth1 -p tcp -m tcp --dport 22 -j DROP;sudo iptables -A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT;sudo service iptables save            
	    sudo bash -c 'echo "192.168.0.0/16 via 192.168.255.2 dev eth1" > /etc/sysconfig/network-scripts/route-eth1'; sudo systemctl restart network
            sudo yum install -y libpcap*
	    sudo rpm -ivh http://li.nux.ro/download/nux/dextop/el7Server/x86_64/knock-server-0.7-1.el7.nux.x86_64.rpm
	    sudo cp /vagrant/knockd.conf /etc/knockd.conf
	    sudo systemctl start knockd
	    sudo systemctl enable knockd
	    sudo sed -i 's/^PasswordAuthentication.*$/PasswordAuthentication yes/' /etc/ssh/sshd_config
	    sudo systemctl restart sshd.service
SHELL
        when "centralRouter"
          box.vm.provision "shell", run: "always", inline: <<-SHELL
            sudo bash -c 'echo "net.ipv4.conf.all.forwarding=1" >> /etc/sysctl.conf'; sudo sysctl -p
            echo "DEFROUTE=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0
            echo "GATEWAY=192.168.255.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
            sudo systemctl restart network
            sudo yum install -y nmap
            sudo cp /vagrant/knock.sh /var/knock.sh
            sudo chmod +x /var/knock.sh
            sudo yum install -y epel-release
            sudo yum install -y nginx
            sudo systemctl start nginx
            sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.255.3:8080
SHELL

       
   end

      end

  end
  
  
end
