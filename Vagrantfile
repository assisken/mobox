Vagrant.configure('2') do |config|
  config.vm.box = 'archlinux/archlinux'
  config.vm.network "forwarded_port", guest: 15151, host: 15151

  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 2
    vb.memory = 4096
  end

  config.vm.provision :ansible do |ansible|
    # Run from root
    ansible.become = true
    ansible.playbook = 'playbook.yml'
  end
end
