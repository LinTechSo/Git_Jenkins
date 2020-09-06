# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = "centos/7"
    jenkins.vm.hostname = "server01"
    jenkins.vm.network :private_network, ip: "192.168.122.12"
    jenkins.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always" 
    #jenkins.vm.synced_folder ".", "/vagrant", disabled: true
    jenkins.ssh.insert_key = false
    jenkins.vm.provider :libvirt do |l|
      l.qemu_use_session = false
      l.memory = 2048
    end
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "jenkins/jenkinsOverTomcat.yml"
      ansible.inventory_path = "inventory/hosts"
      ansible.limit = "all"
    end
  end

  config.vm.define "git" do |git|
    git.vm.box = "centos/7"
    git.vm.hostname = "server02"
    git.vm.network :private_network, ip:"192.168.122.11"
    git.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always" 
    #git.vm.synced_folder ".", "/vagrant", disabled: true
    git.ssh.insert_key = false
    git.vm.provider :libvirt do |l|
      l.qemu_use_session = false
      l.memory = 512
    end
    config.vm.provision "ansible" do |ansible|
      ansible.playbook = "GitLab/install.yml"
      ansible.inventory_path = 'inventory/hosts'
      ansible.limit = "all"
    end
  end
end
