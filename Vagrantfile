MANAGERINSTANCES = 3
WORKERINSTANCES = 1

Vagrant.configure(2) do |config|
  config.ssh.insert_key = false
  config.hostmanager.enabled = true
  #config.cache.scope = :box

  config.vm.box = "mansm/CentOS-7"
  
  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
  end

  MANAGERINSTANCES.times do |i|
    config.vm.define "manager#{i+1}" do |vmconfig|
      vmconfig.vm.hostname = "manager#{i+1}"
      vmconfig.vm.network "private_network", ip: "172.16.50.%d" % (10 + i + 1)

      if i+1 == MANAGERINSTANCES
        vmconfig.vm.provision :ansible do |ansible|
          ansible.playbook = "ansible/playbook.yaml"
          ansible.sudo = true
          ansible.verbose = "v"
          ansible.host_key_checking = false
          ansible.limit = "managers"
          ansible.groups = {
            "managers" => ["manager[1:#{MANAGERINSTANCES}]"],
            "workers" => ["worker[1:#{WORKERINSTANCES}]"]
          }
        end
      end
    end
  end

  WORKERINSTANCES.times do |i|
    config.vm.define "worker#{i+1}" do |vmconfig|
      vmconfig.vm.hostname = "worker#{i+1}"
      vmconfig.vm.network "private_network", ip: "172.16.50.%d" % (20 + i + 1)

      if i+1 == WORKERINSTANCES
        vmconfig.vm.provision :ansible do |ansible|
          ansible.playbook = "ansible/playbook.yaml"
          ansible.sudo = true
          ansible.verbose = "v"
          ansible.host_key_checking = false
          ansible.limit = "workers"
          ansible.groups = {
            "managers" => ["manager[1:#{MANAGERINSTANCES}]"],
            "workers" => ["worker[1:#{WORKERINSTANCES}]"]
          }
        end
      end
    end
  end

  # config.vm.provision "ansible" do |ansible|
  #   ansible.playbook = "ansible/playbook.yaml"
  #   ansible.sudo = true
  #   ansible.verbose = "v"
  #   ansible.host_key_checking = false
  #   ansible.groups = {
  #     "managers" => ["manager[1:#{MANAGERINSTANCES}]"],
  #     "workers" => ["worker[1:#{WORKERINSTANCES}]"]
  #   }
  # end
end