# https://github.com/ogrisel/my-linux-devbox/blob/master/Vagrantfile
# https://gist.github.com/davemkirk/90140b1edde8d18c8b83
# https://github.com/CentOS/sig-cloud-instance-build/issues/27
# run vagrant-vbguest plugin to add guest additions
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.network "forwarded_port", guest: 8888, host: 18888

  config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = "2"
  end

  config.vm.synced_folder "notebook/jupyter", "/home/vagrant/notebook", type: "virtualbox"

  config.vm.provision "shell", inline: <<-SHELL
      curl -O https://repo.continuum.io/archive/Anaconda2-4.3.1-Linux-x86_64.sh
      bash Anaconda2-4.3.1-Linux-x86_64.sh -b -p $HOME/anaconda2
      export PATH="$HOME/anaconda2/bin:$PATH"
      pip install salt
  SHELL

  config.vm.provision "shell", run: "always", inline: <<-SHELL
    export PATH="$HOME/anaconda2/bin:$PATH"
    jupyter-notebook --NotebookApp.token= --port=8888 --ip=0.0.0.0 --notebook-dir=/home/vagrant/notebook &
  SHELL

end
