# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/focal64"
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.hostname = "drupal"
  config.vm.network "private_network", ip: "192.168.56.5"

    # SSH default root user


  config.vm.provider :virtualbox do |v|
    v.name = "drupal"
    v.memory = 512
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  

 # Ansible provisioner.
 config.vm.provision "ansible" do |ansible|
  ansible.compatibility_mode = "2.0"
  ansible.verbose = "v"
  ansible.playbook = "ansible/playbook.yml"
  ansible.become = true
end

end
