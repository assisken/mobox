Vagrant.configure('2') do |config|
  config.vm.box = 'archlinux/archlinux'
  config.vm.network "forwarded_port", guest: 15151, host: 15151

  config.vm.provider 'virtualbox' do |vb|
    vb.cpus = 2
    vb.memory = '8192'

    # Do this as a linked clone if we can.
    vb.linked_clone = true if Gem::Version.new(Vagrant::VERSION) >= Gem::Version.new('1.8.0')

    vagrant_root = File.dirname(File.expand_path(__FILE__))
    file_to_disk = File.join(vagrant_root, '.vagrant/lxd_zpool.vdi')
    unless File.exist?(file_to_disk)
      # lxc images are tiny by default. Not much storage needed.
      vb.customize ['createhd', '--filename', file_to_disk, '--size', 5 * 1024]
    end
    # enabling hostiocache like this isn't safe, but it should be faster.
    vb.customize ['storagectl', :id, '--name', 'IDE Controller', '--hostiocache', 'on']
    vb.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
  end

  config.vm.provision :ansible do |ansible|
    # Run from root
    ansible.become = true
    ansible.playbook = 'playbook.yml'
  end
end
