Vagrant.configure("2") do |config|
# Defining a New VM named server and assign to a variable named dev
  config.vm.define "virtualbox" do |dev1| 
    dev1.vm.hostname = "ubuntu-dev"
    dev1.vm.box = "geerlingguy/ubuntu2004" 
    dev1.vm.memory = "2048"
  end 

  config.vm.define "virtualbox" do |dev2| 
    # Set the hostname of the VM 
    dev2.vm.hostname = "rhel-dev"
    dev2.vm.box = "generic/rhel8" 
    dev2.vm.memory = "2048"
  end
    
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbooks/check_disk_space.yml"
    ansible.become = true
  end

end


