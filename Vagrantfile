# -*- mode: ruby -*-
# vi: set ft=ruby :

module OS
  def OS.windows?
    (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
  end

  def OS.mac?
    (/darwin/ =~ RUBY_PLATFORM) != nil
  end

  def OS.unix?
    !OS.windows?
  end

  def OS.linux?
    OS.unix? and not OS.mac?
  end
end

if Vagrant::VERSION < "1.8.1"
  puts "This Vagrant environment requires Vagrant 1.8.0 or higher."
  puts "See: https://www.vagrantup.com/downloads.html"
  exit 1
end

Vagrant.configure(2) do |config|

  config.ssh.forward_agent = true

  # start with a Ubuntu install
  config.vm.box = "ubuntu/xenial64"

  # configure root user .profile to avoid 'stdin: is not a tty' messages
  # see https://github.com/mitchellh/vagrant/issues/1673
  config.vm.provision "configure-root-profile", type: "shell" do |s|
    s.privileged = false
    s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
  end

  # configure vagrant user .profile
  config.vm.provision "configure-user-profile", type: "shell", path: "bootstrap-user-profile.sh", privileged: false

  # install required software
  config.vm.provision "bootstrap-root", type: "shell", path: "bootstrap-root.sh"
  config.vm.provision "bootstrap-user", type: "shell", path: "bootstrap-user.sh", privileged: false

  # port-forwarding for the hyperledger docker containers
  config.vm.network :forwarded_port, guest: 7501, host: 7501, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7502, host: 7502, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7503, host: 7503, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7504, host: 7504, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7505, host: 7505, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7506, host: 7506, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7507, host: 7507, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7508, host: 7508, host_ip: "localhost", auto_correct: true
  
  config.vm.network :forwarded_port, guest: 8080, host: 8080, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 3000, host: 3000, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 4200, host: 4200, host_ip: "localhost", auto_correct: true
  config.vm.network :forwarded_port, guest: 7070, host: 7070, host_ip: "localhost", auto_correct: true

  # disable the default synced folder always tries to set up
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder "./learning", "/home/ubuntu/learning"
  
  # set private ip address
  config.vm.network "private_network", ip: "192.168.40.2"

  config.vm.provider :virtualbox do |v|

    # Optionally increase memory allocated to vm (for build tasks)
    v.memory = 4096
  end

  # allow image to use OSX VPN
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end
end
