
# This is a script to create two servers in a hyperversor like virtualbosx using vagrant. The first is an ubuntu machine and serves a static website using apache2 and the second server is a centos machine and is a database server
Vagrant.configure("2") do |config|
	config.vm.define "web01" do |web01|
		web01.vm.box = "ubuntu/bionic64"
		# allocating ip address to the web server
		web01.vm.network "private_network", ip: "192.168.34.10"
		web01.vm.network "public_network"
                # Allocating RAM and CPU
		web01.vm.provider "virtualbox" do |vb|
		  vb.memory = "1024"
		  vb.cpus = 2
		end
                # The below is an inline bash script to automatically set up the web server
		web01.vm.provision "shell", inline:<<-SHELL
			sudo -i
			apt update -y
			apt install wget unzip apache2 -y
			systemctl start apache2
			systemctl enable apache2
			cd /tmp/
			wget https://www.tooplate.com/zip-templates/2130_waso_strategy.zip
			unzip -o 2130_waso_strategy.zip
			cp -r 2130_waso_strategy/* /var/www/html/
			systemctl restart apache2
		SHELL
	end
        
        # Provisioning the Database server
	config.vm.define "app01" do |app01|
		app01.vm.box = "geerlingguy/centos7"
		
		app01.vm.network "private_network", ip: "192.168.34.11"
		app01.vm.network "public_network"
		app01.vm.provider "virtualbox" do |vb|
		  vb.memory = "1024"
		  vb.cpus = 2
		end
                # To set up the database server, a bash script is included in the current directory. This vagrant script calls that bash script while provisioning and automatically sets up the database server
		app01.vm.provision "shell", path: "script.sh"
	end
end
