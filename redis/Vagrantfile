$script = <<-SCRIPT
  apt update
  apt -y install make gcc libc6-dev tcl
  wget http://download.redis.io/redis-stable.tar.gz
  tar xvzf redis-stable.tar.gz
  cd redis-stable
  sudo make install
  sudo chown -R vagrant:vagrant /home/vagrant/redis-stable
  sudo chmod -R 775 /home/vagrant/redis-stable
  
  echo "Add replicas to cluster (Example)"
  echo "redis-cli --cluster add-node 192.168.111.122:6379 127.0.0.1:6381 --cluster-slave --cluster-master-id 093ff6d178f3127b5d9b7c81cce38d8cb905cc62"
SCRIPT

$redis1 = <<-SCRIPT
  ln -s /vagrant/configs/redis1-node/*.conf /home/vagrant/redis-stable/
  redis-server /home/vagrant/redis-stable/a_master.conf
  redis-server /home/vagrant/redis-stable/b_replica.conf
  redis-server /home/vagrant/redis-stable/c_replica.conf
SCRIPT

$redis2 = <<-SCRIPT
  ln -s /vagrant/configs/redis2-node/*.conf /home/vagrant/redis-stable/
  redis-server /home/vagrant/redis-stable/a_replica.conf
  redis-server /home/vagrant/redis-stable/b_master.conf
  redis-server /home/vagrant/redis-stable/c_master.conf
  redis-cli --cluster create 192.168.111.121:6379 192.168.111.122:6380 192.168.111.122:6381 --cluster-yes
  redis-cli --cluster add-node 192.168.111.122:6379 192.168.111.121:6379 --cluster-slave
  redis-cli --cluster add-node 192.168.111.121:6380 192.168.111.121:6379 --cluster-slave
  redis-cli --cluster add-node 192.168.111.121:6381 192.168.111.121:6379 --cluster-slave
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.provision "shell", inline: $script

  config.vm.define "redis1" do |redis1|
    redis1.vm.hostname = "redis1"
    redis1.vm.network "public_network", ip: "192.168.111.121", bridge: "wlo1"
    redis1.vm.provision "shell", inline: $redis1
  end

  config.vm.define "redis2" do |redis2|
    redis2.vm.hostname = "redis2"
    redis2.vm.network "public_network", ip: "192.168.111.122", bridge: "wlo1"
    redis2.vm.provision "shell", inline: $redis2
  end

end

