Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.network "forwarded_port", guest: 50001, host: 50001
  config.vm.network "forwarded_port", guest: 50002, host: 50002

  config.vm.network "forwarded_port", guest: 8333, host: 8333 #mainnet
  #config.vm.network "forwarded_port", guest: 18333, host: 18333 #testnet

  #config.vm.network "forwarded_port", guest: 8332, host: 8332 #mainnet RPC
  #config.vm.network "forwarded_port", guest: 18332, host: 18332 #testnet RPC

#  config.vm.network "public_network", ip: 192.168.1.201

  config.vm.provider "virtualbox" do |v|
    #v.memory = 2048
    v.memory = 4096  # jelectrum recommendations
    v.cpus = 2
  end

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo add-apt-repository -y ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install -y bitcoind

    sudo apt-get install -y supervisor
    sudo apt-get install -y git
    sudo apt-get install -y ant
    sudo apt-get install -y screen
    sudo apt-get install -y openjdk-7-jdk

    mkdir .bitcoin
    ln -s /vagrant/bitcoin.conf /home/vagrant/.bitcoin/

    sudo ln -s /vagrant/bitcoin_supervisor.conf /etc/supervisor/conf.d
    sudo supervisorctl reread
    sudo supervisorctl update

    git clone https://github.com/fireduck64/jelectrum.git
    ln -s /vagrant/jelly.conf /home/vagrant/jelectrum/
    ln -sf /vagrant/motd.txt /home/vagrant/jelectrum/
    ln -sf /vagrant/keystore.jks /home/vagrant/jelectrum/keystore.jks
    cd jelectrum
    ant jar

    sudo ln -s /vagrant/jelectrum_supervisor.conf /etc/supervisor/conf.d
    sudo supervisorctl reread
    sudo supervisorctl update
  SHELL
end
