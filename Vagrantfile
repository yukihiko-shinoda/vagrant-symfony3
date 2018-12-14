Dotenv.load
Vagrant.configure(2) do |config|
  config.vm.box = "futureys/centos-6-symfony-3"
  config.vbguest.no_install = true
  config.vm.network "public_network"
  config.vm.network "private_network", ip: "172.28.128.3"
  config.vm.provider "virtualbox" do |vm|
    vm.memory = 2048
  end

  config.vm.network :forwarded_port, host: 80, guest: 80
  config.vm.network :forwarded_port, host: 3306, guest: 3306

  config.vm.provision "application", type: :ansible_local do |ansible|
    ansible.install           = false
    ansible.provisioning_path = "/vagrant/ansistrano-symfony3"
    ansible.inventory_path    = "inventories/development"
    ansible.playbook          = "deploy.yml"
    ansible.verbose           = "vvv"
    ansible.galaxy_roles_path = "/home/vagrant/.ansible/roles"
    ansible.extra_vars        = {
        mysql_root_password: ENV['MYSQL_5_1_ROOT_PASSWORD'],
        application_database_password: ENV['MYSQL_5_1_USER_PASSWORD'],
        application_database_host: ENV['APPLICATION_DATABASE_HOST'],
        symfony_secret: ENV['SYMFONY_SECRET']
    }
  end

  config.vm.provision "file", source: ENV['GIT_PATH_TO_DEPLOY_KEY'], destination: "/home/vagrant/.ssh/id_rsa_deploy_key_github_yuk_hik_future_ansistrano_symfony3"
  config.vm.provision "shell", inline: "chmod 600 /home/vagrant/.ssh/id_rsa_deploy_key_github_yuk_hik_future_ansistrano_symfony3"
  config.vm.provision "shell", inline: "cp -fip /home/vagrant/workspace/repo/.idea/misc.xml.dist /home/vagrant/workspace/repo/.idea/misc.xml"
  config.vm.provision "shell", inline: "cp -fip /home/vagrant/workspace/repo/.idea/repo.iml.dist /home/vagrant/workspace/repo/.idea/repo.iml"
end