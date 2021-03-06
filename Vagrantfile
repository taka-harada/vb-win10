# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "win10"

  config.vm.guest = "windows"
  config.vm.communicator= "winrm"
  config.vm.hostname = "windows-node"
  config.vm.network "private_network", ip: "192.168.50.100"
  config.vm.synced_folder ".", "/vagrant", :mount_options => ['dmode=775', 'fmode=664']
  config.vm.boot_timeout=600

  config.winrm.username = "IEUser"
  config.winrm.password = "Passw0rd!"

  config.vm.provider "virtualbox" do |vb|
    vb.name = "vagrant-win10"
    vb.memory = 2048
    vb.cpus = 2
    # vb.gui = true
    vb.customize ["modifyvm", :id, "--vram", "256"]
    vb.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
    vb.customize ["modifyvm", :id, "--usb", "on"]
  end

  config.vm.provision "shell" do |shell|
    shell.path = "scripts/ConfigureRemotingForAnsible.ps1"
  end

  # "vagrant provision"でansible実行がコケたため、あとで調査。一旦直接ansibleコマンドでplaybookを実行する方法で制御
  # config.vm.provision "ansible" do |ansible|
  #   ansible.inventory_path = "inventries/hosts"
  #   ansible.playbook = "playbook/playbook.yml"
  #   ansible.limit = "windows"
  #   ansible.host_vars = {
  #     "default" => {
  #       "ansible_ssh_port" => 55986,
  #       "ansible_winrm_server_cert_validation" => "ignore"
  #     }
  #   }
  # end

  # trigger実行タイミングを設定（ここでは "vagrant reload" をtrigger）
  config.trigger.after :reload do |trigger|

    if File.file?(File.expand_path(".vagrant/machines/default/virtualbox/id"))
      # uuidをvagrantの設定ファイルから読み込む
      #id = File.read(".vagrant\\machines\\default\\virtualbox\\id") #ホストマシンがwinの場合はこっちかも？
      id = File.read(".vagrant/machines/default/virtualbox/id")

      # trigger実行時にidをログとして出力
      trigger.info = "Mount webcom to #{id}"

      # 取得したidを.1カメラとアタッチする
      trigger.run = {
        inline: "VBoxManage controlvm #{id} webcam attach .1",
      }
    end
  end

end
