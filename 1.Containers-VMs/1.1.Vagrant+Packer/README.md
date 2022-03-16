
#2.1 Create base box with Ubuntu 18.04 using packer, output format - vagrant:
#According to tutorial (https://www.serverlab.ca/tutorials/dev-ops/automation/how-to-use-packer-to-create-ubuntu-18-04-vagrant-boxes/) 
#create a typical directory structure and required files for Packer (ubuntu1804.json, preseed.cfg, init.sh, cleanup.sh).
#After we added all of the necessary parts for Packer, next step is build Vagrant Image using next command:
	
	packer build ubuntu1804.json
	
	![](screenshot/ packer_build.png)
	
#Next, adding new box to Vagrant:
	
	vagrant box add --name "serverlab/ubuntu1804" ubuntu-18.04.box 
	
	![Alt text](/screenshot/vagrant_box_add.png?raw=true "Adding new box to Vagrant")
	
#2.2 Running new VM in Vagrant  	
#Initialize Vagrant environment by creating an initial Vagrantfile by following command:

	vagrant init serverlab/ubuntu1804
	
	![Alt text](/screenshot/vagrant_init.png?raw=true "Initialize Vagrant environment")
	
#Running new VM:
	
	vagrant up 
	
	![Alt text](/screenshot/vagrant_up.png?raw=true "Running new VM")
	
	![Alt text](/screenshot/vagrant_ssh.png?raw=true "Vagrant ssh")
	
#2.3 Adding chef recipes for installing some software:
#Firstly for using chef recipes and download cookbook with all dependencies add some plugin to Vagrant by following command:

	vagrant plugin install vagrant-omnibus
	vagrant plugin install vagrant-berkshelf
	
#Then in our Vagrant environment create Berkshelf file:
	source "https://supermarket.chef.io"

	cookbook 'apache2', '~> 8.14.2'
	cookbook 'mariadb', '~> 5.2.3'
	cookbook 'mysql', '~> 11.0.4'
	cookbook 'java', '~> 11.0.1'
	cookbook 'tomcat', '~> 5.0.2'

#Install  cookbook	
	berks install

#In berkshelfs directory add folowing recipes in each cookbook by create recipes directory and default.rb file:
	#apache2
	service 'apache2' do
	  service_name lazy { apache_platform_service_name }
	  supports restart: true, status: true, reload: true
	  action :nothing
	end

	apache2_install 'default_install'
	apache2_module 'headers'
	apache2_module 'ssl'
	
	#mysql
	mysql_service 'default' do 
		port '3307'
		data_dir '/data'
		initial_root_password 'Pass1234'
		action [:create, :start]
	end

	mysql_client 'default' do
		action :create
	end
	
	#mariadb
	mariadb_server_install 'MariaDB Server install' do
	  version '10.3'
	end
	
	mariadb_client_install 'MariaDB Client install' do
	  version '10.3'
	end
	
	#java
	openjdk_install '11'
	
	#tomcat
	tomcat_install 'helloworld' do
		version '8.0.36'
	end

#Next is the  change Vagrantfile:
	config.berkshelf.enabled = true
	config.vm.provision :chef_solo do |chef|
		chef.add_recipe "apache2"
		chef.add_recipe "mysql"
		chef.add_recipe "java"
		chef.add_recipe "tomcat"
		chef.arguments = "--chef-license accept"
	end
	
	vagrant  reload --provision
	
#2.4 Forwarding port 
#Add some new line to Vagrantfile, also:

	config.vm.network :forwarded_port, guest: 22, host: 22022
	config.vm.network :forwarded_port, guest: 80, host: 22080
	config.vm.network :forwarded_port, guest: 443, host: 22443
	config.vm.network :forwarded_port, guest: 3306, host: 22306	
	
	vagrant reload 
	
#2.5 Template box
#Edited Vagrantfile, also change MySQL to MariaDB:

	chef.add_recipe "mariadb"
	
#Reload current VM and up new:	
	
	vagrant reload --provision
	
#Create new template Vagrant box:
	
	vagrant package --output templateMariadb.box 
	
	![Alt text](/screenshot/vagrant_ssh.png?raw=true "Template box")
	
	
