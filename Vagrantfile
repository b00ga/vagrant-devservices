Vagrant.configure("2") do |config|
  config.vm.hostname = "hq.boulderhill.net"

  config.vm.box = "bento/centos-8"
  config.vm.synced_folder "shared-folder", "/vagrant"
  config.vm.synced_folder "playbooks", "/ansible", :mount_options => ["ro"]

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.network "forwarded_port", guest: 80, host: 9000, id: "http"
  config.vm.network "forwarded_port", guest: 3001, host: 9001, id: "gitea"
  config.vm.network "forwarded_port", guest: 9090, host: 9003, id: "cockpit"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "requirements.yml"
    ansible.galaxy_roles_path = "/etc/ansible/roles"
    ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
    ansible.provisioning_path = "/ansible"
    ansible.compatibility_mode = "2.0"
    #ansible.verbose = '-vvvv'
  end
end
