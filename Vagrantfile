Vagrant.configure("2") do |config|
  config.vm.hostname = "hq.boulderhill.net"

  config.vm.box = "bento/almalinux-8"
  config.vm.synced_folder "shared-folder", "/vagrant"
  config.vm.synced_folder "playbooks", "/ansible", :mount_options => ["ro"]

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.network "forwarded_port", guest: 9443, host: 9443, id: "althttps"
  config.vm.network "forwarded_port", guest: 9090, host: 9003, id: "cockpit"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "requirements.yml"
    ansible.galaxy_roles_path = "/etc/ansible/roles"
    ansible.extra_vars = { ansible_python_interpreter:"/usr/bin/python3" }
    ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p /usr/share/ansible/collections --force && sudo ansible-galaxy role install -r %{role_file} --roles-path=%{roles_path} --force"
    ansible.provisioning_path = "/ansible"
    ansible.compatibility_mode = "2.0"
    ##ansible.verbose = '-vvv'
    ##ansible.skip_tags = "389ds,cockpit,firewalld,gitea,nginx,nodejs,pki,postgres"
  end
end
