Vagrant.configure(2) do |config|
  config.ssh.shell = "sh"

  config.vm.synced_folder "./conf", "/home/vagrant/.abuild"
  config.vm.synced_folder "./packages", "/home/vagrant/packages"

  %w(alpine32 alpine64).each do |box_name|
    config.vm.define box_name do |app|
      app.vm.provider "lxc" do |lxc, conf|
        conf.vm.box = "ysbaddaden/#{box_name}"
        lxc.container_name = "crystal-abuild-#{box_name}"
      end
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    if ! $(grep -q /home/vagrant/packages /etc/apk/repositories); then
      echo "file:///home/vagrant/packages/testing" >> /etc/apk/repositories
    fi
    ln -sf /home/vagrant/.abuild/*.pub /etc/apk/keys/

    apk update
    apk upgrade
    apk add alpine-sdk
    apk add crystal

    addgroup vagrant abuild
  SHELL
end
