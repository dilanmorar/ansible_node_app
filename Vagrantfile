# Install required plugins
## Install plugin so that vagrant can update host
required_plugins = ["vagrant-hostsupdater"]
## Loop of updating installing vagrant plugin in all files unless already in there
required_plugins.each do |plugin|
    exec "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

def set_env(vars)
  # Create code that will run some bash commands - HEREDOC allows multiple lines to be one string
  command = <<~HEREDOC
    echo "setting environment variables"
    source ~/.bashrc
  HEREDOC

  vars.each do |key, value|
    command += <<~HEREDOC
    if [ -z "$#{key}"]; then
      echo "export #{key}=#{value}" >> ~/.bashrc
    fi
    HEREDOC
  end

  command
end

# Class - number defines what kind of configuation
Vagrant.configure("2") do |config|
  #provisioner = Vagrant::Util::Platform.windows? ? :guest_ansible : :ansible
  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/xenial64"
    app.vm.network "private_network", ip: "192.168.10.100"
    app.hostsupdater.aliases = ["development.local"]
    app.vm.synced_folder "app", "/home/ubuntu/app"
    app.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "environment/app/app.yml"
      ansible.become = true
    end
    app.vm.provision "shell", inline: set_env({DB_HOST: "mongodb://192.168.10.150:27017/posts"}), privileged: false
  end

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/xenial64"
    db.vm.network "private_network", ip: "192.168.10.150"
    db.hostsupdater.aliases = ["database.local"]
    db.vm.synced_folder "environment/db/", "/home/ubuntu/environment"
    db.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "environment/db/db.yml"
      ansible.become = true
    end
  end

end
