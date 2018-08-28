Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-16.04"

  config.vm.provider "virtualbox" do |v|
    v.name = "mms-dev-data"
    v.gui = false
  end
  
  # SYSTEM RESOURCES
  # disk space (statically allocated) 
  config.disksize.size = "100GB"

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
  end

  # NETWORKING
  # forward ISDB port
  config.vm.network "forwarded_port", guest: 27017, host: 27017 

  # forward a range of ports for your all your mongod and mongos needs
  # (warning: sometimes you'll hit a port collision here. if netstat
  # doesn't identify a process running on this port, retry)
  for i in 64000..65535
      config.vm.network :forwarded_port, guest: i, host: i
  end

  # AD-HOC CONFIG
  # shared folders to transfer files between host OS and guest OS
  config.vm.synced_folder "./shared", "/shared_from_host"
  config.vm.synced_folder "~/mms/scripts", "/usr/local/scripts"

  # https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/
  config.vm.provision "shell", inline: <<-SHELL
     sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
     echo "deb [ arch=amd64,arm64,ppc64el,s390x ] http://repo.mongodb.com/apt/ubuntu xenial/mongodb-enterprise/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
     apt-get update
     apt-get install -y mongodb-enterprise
 
     mkdir -p /data/db
     mkdir -p /data/db/backups
     mkdir -p /home/vagrant/mms/data

     # start up isdb
     mongod --bind_ip=0.0.0.0 --dbpath=/home/vagrant/mms/data --logpath=/home/vagrant/mms/data/log --fork
   SHELL
end