# vi: set ft=ruby :

n = 2

Vagrant.configure("2") do |config|
  config.vm.define "loadbalancer" do |loadbalancer|
    loadbalancer.vm.box = 'ubuntu/trusty64'
    loadbalancer.vm.hostname = "loadbalancer"
    loadbalancer.vm.network :private_network, ip: "10.10.10.10"
    loadbalancer.vm.network "forwarded_port", guest: 80, host: 28000
    loadbalancer.vm.network "forwarded_port", guest: 8080, host: 28080
    loadbalancer.vm.provision :ansible do |ansible|
      ansible.playbook = "playbooks/provision_lb.yml"
    end
  end

  n.times do |i|
    config.vm.define "app-#{i+1}" do |app|
      app.vm.box = 'ubuntu/trusty64'
      app.vm.hostname = "app-#{i+1}"
      app.vm.network :private_network, ip: "10.10.10.#{10+i+1}"
      app.vm.provision :ansible do |ansible|
        ansible.playbook = "playbooks/provision_app.yml"
      end
    end
  end

  config.vm.define "database" do |database|
    database.vm.box = 'ubuntu/trusty64'
    database.vm.hostname = "database"
    database.vm.network :private_network, ip: "10.10.10.20"
    database.vm.provision :ansible do |ansible|
      ansible.playbook = "playbooks/provision_db.yml"
    end
  end
end