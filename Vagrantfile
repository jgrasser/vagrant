# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  if( "#{ARGV[1]}"=="")
    ARGV[1]='default'
  end

  config.vm.define :"#{ARGV[1]}" do |machine|
    machine.vm.hostname="#{ARGV[1]}"
  end

  config.vm.provider :"#{ARGV[1]}" do |machine|
    machine.name="#{ARGV[1]}" 
  end

  config.vm.provider "virtualbox" do |vm|
    vm.customize ["modifyvm", :id, "--name", "#{ARGV[1]}"] 
  end

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
   config.vm.box = "centos/7"
  
  # Turn off synced folder to fix rsync bug with centos 7 
   config.vm.synced_folder ".", "/vagrant", disabled: true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "10.100.100.10", auto_config: false, virtualbox__intnet:"intnet"
  
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
     vb.memory = 2048
     vb.cpus = 2
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  if "#{ARGV[1]}".start_with?('docker')
        config.vm.provision "shell", inline: <<-SHELL
            yum install -y yum-utils device-mapper-persistent-data lvm2
            yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
            yum install -y docker-ce
            systemctl enable docker
            systemctl start docker
         SHELL
   else
       config.vm.provision "shell", inline: <<-SHELL
          mkdir -p /share/{nfs,smb}/{user,group}
          mkdir -p /html/{user,group,open,cg-bin}
          echo "**USER ** THIS IS A USER CONTROLED DIR" > /html/user/index.html
          echo "**GROUP** THIS IS A GROUP CONTROLLED DIR" > /html/group/index.html
          echo "**OPEN ** EVERYONE CAN SEE THIS DIR" > /html/open/index.html
          groupadd dogs
          groupadd cats
          groupadd pets
          useradd persian -G cats,pets
          useradd puma -G cats
          useradd lab -G dogs,pets
          useradd golden -G dogs,pets
       SHELL
   end
end
