# Install required plugins
required_plugins = %w(vagrant-hostsupdater vagrant-berkshelf ) # allow vagrant to run berks vendor coobooks for us
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure("2") do |config|
  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.hostsupdater.aliases = ["development.local"]
    app.vm.synced_folder "app", "/home/ubuntu/app"
    app.vm.provision "shell", inline: "echo DB_HOST=192.168.10.150 >> .bashrc"
    app.vm.provision "chef_solo" do |chef|
      chef.arguments = "--chef-license accept"
      chef.add_recipe "development::default"
    end
end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.vm.synced_folder "environment/db", "/home/ubuntu/environment"
    db.vm.provision "chef_solo" do |chef|
      chef.arguments = "--chef-license accept"
      chef.add_recipe "nginx::default"

  end
end
end
