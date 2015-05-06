# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
aws_configs = YAML.load_file("aws-credential.yml")
digoc_configs = YAML.load_file("digital_ocean_configs.yml")

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  # config.vm.box = "chef/centos-6.6"
  config.vm.box = "dummy"
  
  # EC2 (AWS) 
  config.vm.define "machine1" do |m|
    m.vm.provider :aws do |aws, override|
      aws.access_key_id = aws_configs["access_key_id"]
      aws.secret_access_key = aws_configs["secret_access_key"]
      aws.keypair_name = "vagrant-aws-2"
      aws.instance_type = "t2.micro"
      aws.security_groups = [ 'vagrant-security-group' ]
      aws.region = "ap-northeast-1" # Tokyo
      aws.availability_zone = "ap-northeast-1a"

      aws.tags = {
          "Name" => "vtest"
      }

      # My AMI
      aws.ami = "ami-6ae5296a"
      aws.elastic_ip = aws_configs["elastic_ip"]

      override.ssh.username = "ec2-user"
      override.ssh.private_key_path = "~/.ssh/vagrant-aws-2.pem"
    end
  end

  # DigitalOcean
  config.vm.define "machine2" do |m|
    m.vm.provider :digital_ocean do |digoc, override|

      override.vm.hostname = "vagrant"
      override.vm.box = "digital_ocean"
      override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"
      override.ssh.username = "vagrant"
      override.ssh.private_key_path = "~/.ssh/id_rsa"
        
      digoc.token = digoc_configs["token"]
      digoc.ssh_key_name = digoc_configs["ssh_key_name"]
      digoc.size = '512mb'
      digoc.region = "sgp1"
      digoc.image = "centos-6-5-x64"
      digoc.setup = true
    end
  end
  

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  config.vm.provision "shell", inline: "yum update -y"
end
