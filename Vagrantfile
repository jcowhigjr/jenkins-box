# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  config.vm.hostname = "jenkins-box-librarian"

  # Every Vagrant virtual environment requires a box to build off of.
  #config.vm.box = "precise64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"


  config.vm.box = 'omnibus'
  config.vm.box_url = 'https://s3.amazonaws.com/gsc-vagrant-boxes/ubuntu-12.04-omnibus-chef.box'

  # Assign this VM to a host-only network IP, allowing you to access it
  # via the IP. Host-only networks can talk to the host machine as well as
  # any other machines on the same network, but cannot be accessed (through this
  # network interface) by any external networks.
  # config.vm.network :private_network, ip: "33.33.33.10"

  config.vm.network :private_network, ip: '10.255.255.10'
  config.vm.network :forwarded_port, guest: 8080, host: 8080

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.

  # config.vm.network :public_network

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider :virtualbox do |vb|
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  # end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  config.ssh.max_tries = 40
  config.ssh.timeout   = 120

  # The path to the Berksfile to use with Vagrant Berkshelf
  # config.berkshelf.berksfile_path = "./Berksfile"

  # Enabling the Berkshelf plugin. To enable this globally, add this configuration
  # option to your ~/.vagrant.d/Vagrantfile file
  #config.berkshelf.enabled = true

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to exclusively install and copy to Vagrant's shelf.
  # config.berkshelf.only = []

  # An array of symbols representing groups of cookbook described in the Vagrantfile
  # to skip installing and copying to Vagrant's shelf.
  # config.berkshelf.except = []

  config.vm.provision :chef_solo do |chef|

    chef.cookbooks_path = ['cookbooks']
    chef.add_recipe 'apt'
    chef.add_recipe 'git'

    chef.add_recipe 'rvm'
    chef.add_recipe 'rvm::vagrant'
    chef.add_recipe 'rvm::system'
    chef.add_recipe 'rvm::gem_package'

    chef.add_recipe 'mysql::server'
    chef.add_recipe 'jenkins::server'
    chef.add_recipe 'phantomjs::default'

    chef.add_recipe 'oh_my_zsh'
    chef.add_recipe 'locale'


    # configure recipes
    chef.json = {
      rvm: {
          vagrant: { system_chef_solo: '/opt/chef/bin/chef-solo' },
          rubies:       ['2.0.0'],
          default_ruby: '2.0.0',
          global_gems:  [{ name: 'bundler' }, { name: 'rake' }]
      },
      mysql: {
          server_root_password:   'password',
          server_repl_password:   'password',
          server_debian_password: 'password',
          service_name:           'mysql',
          basedir:                '/usr',
          data_dir:               '/var/lib/mysql',
          root_group:             'root',
          mysqladmin_bin:         '/usr/bin/mysqladmin',
          mysql_bin:              '/usr/bin/mysql',
          conf_dir:               '/etc/mysql',
          confd_dir:              '/etc/mysql/conf.d',
          socket:                 '/var/run/mysqld/mysqld.sock',
          pid_file:               '/var/run/mysqld/mysqld.pid',
          grants_path:            '/etc/mysql/grants.sql',
          root_network_acl:       '%',
          allow_remote_root:      true
      }
    }

    ## run recipes
    #chef.run_list = [
    #  'recipe[apt]',
    #  'recipe[jenkins::server]',
    #  'recipe[mysql::server]',
    #  'recipe[rvm]',
    #  'recipe[rvm::vagrant]',
    #  'recipe[rvm::system]',
    #  'recipe[rvm::gem_package]',
    #  'recipe[phantomjs::default]'
    #]
  end
end
