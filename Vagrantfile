# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
    #by default, the folder where your Vagrantfile is
    #located is mapped into the VM as /vagrant, this means
    #you do NOT need to synch that folder. The follopwing statement
    #shares the parent folder of the directory that holds your
    #Vargantfile with the VM.For the example in class this is your
    #c:\users\your-home-folder\devops (Windows) or
    #/Users/your-home-dir/devops (Mac).
    # Doing this may be useful latter on in the class
    # as it will allow you to access any folders you
    #add
    config.vm.synced_folder "..","/vagrant"
    config.vm.box = "ubuntu/xenial64"
    config.vm.network "forwarded_port", guest: 3389, host: 3389,
    id:"rdp", auto_correct: true
    config.vm.network "forwarded_port", guest: 5901, host: 5901,
    id:"vnc", auto_correct: true
    config.vm.network 'private_network', ip: '10.1.1.30'

    config.vm.provision "coreinstall", type: "shell", inline: <<-SHELL
        sudo apt-get -y update && \
        sudo apt-get -y upgrade && \
        sudo apt-get -y install \
        build-essential git pkg-config zip unzip software-properties-common \
        python-pip python-dev \
        libgmp-dev gcc-multilib valgrind openmpi-bin openmpi-doc libopenmpi-dev \
        portmap rpcbind libcurl4-openssl-dev bzip2 imagemagick libmagickcore-dev \
        libssl-dev libffi6 libffi-dev llvm
        sudo pip install --upgrade pip
        sudo pip install requests future cryptography pyopenssl ndg-httpsclient pyasn1 nelson
    SHELL


    config.vm.provision "installxrdp", type: "shell", inline: <<-SHELL
        sudo apt-get -y update
        sudo apt-get -y install xrdp
        sudo sed -i 's/^fi/fi\\n\\nxfce4-session/' /etc/xrdp/startwm.sh
        sudo service xrdp restart
    SHELL



    config.vm.provision "installxfce", type: "shell", inline: <<-SHELL
        sudo apt-get -y update
        sudo apt-get -y install xfce4
        sudo apt-get -y install xfce4-terminal
        echo xfce4-session > /home/ubuntu/.xsession
        cat /vagrant/DevOps03/pw.txt | sudo passwd ubuntu
    SHELL


    # config.vm.provision "installvnc", type: "shell", inline: <<-SHELL
    #     sudo apt-get -y install tightvncserver
    # SHELL


    config.vm.provision "installvscode", type: "shell", inline: <<-SHELL
        sudo apt-get -y install libnss3
        sudo dpkg -i /vagrant/DevOps03/code_1.34.0-1557957934_amd64.deb
        sudo apt-get -y install -f
        cd /home/ubuntu
        mkdir  -p ./lib
        cp /usr/lib/x86_64-linux-gnu/libxcb.so.1.1.0 ./lib
        sed -i 's/BIG-REQUESTS/_IG-REQUESTS/' ./lib/libxcb.so.1.1.0
        cd lib
        ln -f -s libxcb.so.1.1.0 libxcb.so.1
        sudo sync
    SHELL

    config.vm.provision "reboot", type: "shell", inline: <<-SHELL
        sync
        sudo reboot
    SHELL

end
