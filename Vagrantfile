# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.ssh.forward_agent = true
  config.vm.synced_folder Dir.getwd, "/home/vagrant/roles/ansible-chronos", nfs: true

  # ubuntu 14.04
  config.vm.define 'ubuntu', primary: true do |c|
    c.vm.network "private_network", ip: "192.168.100.3"
    c.vm.box = "ubuntu/trusty64"
    c.vm.provision "shell" do |s|
      s.inline = "
        sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E56151BF;
        DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]');
        CODENAME=$(lsb_release -cs);
        echo \"deb http://repos.mesosphere.com/${DISTRO} ${CODENAME} main\" | tee /etc/apt/sources.list.d/mesosphere.list
        apt-get update -y;
        apt-get install python-dev python-pip mesos -y;
        pip install -U ansible;
        service mesos-master restart
        "
      s.privileged = true
    end
  end

  # centos 7:
  config.vm.define 'centos7' do |c|
    c.vm.network "private_network", ip: "192.168.100.5"
    c.vm.box = "centos/7"
    c.vm.provision "shell" do |s|
      s.inline = "
        hostname localhost;
        iptables -F;
        service iptables save;
        rpm -Uvh http://archive.cloudera.com/cdh4/one-click-install/redhat/6/x86_64/cloudera-cdh-4-0.x86_64.rpm;
        yum -y install zookeeper;
        zookeeper-server-initialize --myid=1;
        zookeeper-server start;
        rpm -Uvh http://repos.mesosphere.io/el/7/noarch/RPMS/mesosphere-el-repo-7-2.noarch.rpm;
        yum install -y epel-release;
        yum -y install mesos ansible;
        "
      s.privileged = true
    end
  end

end
