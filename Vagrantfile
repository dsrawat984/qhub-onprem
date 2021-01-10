Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"

  # forces to use $HOME/.vagrant.d/insecure_private_key' we need the
  # ssh key to be the same when /home get mounted on nfs
  config.ssh.insert_key = false

  config.vm.define "hpc01-test", primary: true do |hpc01|
    hpc01.vm.hostname = "hpc01-test"
    hpc01.vm.provider "libvirt" do |libvirthpc01|
      libvirthpc01.memory = 2048
      libvirthpc01.cpus = 2
    end
  end

  config.vm.define "hpc02-test", primary: true do |hpc02|
    hpc02.vm.hostname = "hpc02-test"
    hpc02.vm.provider "libvirt" do |libvirthpc02|
      libvirthpc02.memory = 4096
      libvirthpc02.cpus = 2
    end
  end

  config.vm.define "hpc03-test", primary: true do |hpc03|
    hpc03.vm.hostname = "hpc03-test"
    hpc03.vm.provider "libvirt" do |libvirthpc03|
      libvirthpc03.memory = 4096
      libvirthpc03.cpus = 2
    end
  end

  config.vm.define "hpc04-test", primary: true do |hpc04|
    hpc04.vm.hostname = "hpc04-test"
    hpc04.vm.provider "libvirt" do |libvirthpc04|
      libvirthpc04.memory = 4096
      libvirthpc04.cpus = 2
    end
  end

  # # If you are having dns resolving issues uncomment for one provision step
  # # Related to issues with dnssec and https://github.com/lavabit/robox/issues/106
  
  
  config.vm.provision 'shell', inline: "if grep 'DNSSEC=yes' /etc/systemd/resolved.conf; then sed -i 's/DNSSEC=yes/DNSSEC=no/g' /etc/systemd/resolved.conf; systemctl restart systemd-resolved.service; fi", privileged: true

  ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
  config.vm.provision 'shell', inline: "echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys", privileged: false

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yaml"
    ansible.groups = {
      "hpc-master" => ["hpc01-test"],
      "hpc-worker" => ["hpc02-test", "hpc03-test", "hpc04-test"],
    }
  end
end
