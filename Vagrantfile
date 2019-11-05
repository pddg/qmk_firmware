# -*- mode: ruby -*-
# vi: set ft=ruby :
Vagrant.configure("2") do |config|

  config.vm.box = "hashicorp/bionic64"
    
  config.vm.provider "virtualbox" do |v|
    # USBデバイスのパススルーを許可
    v.name = "VM to flash firmware into ProMicro"
    v.customize ["modifyvm", :id, "--usb", "on"]
    v.customize ["modifyvm", :id, "--usbehci", "on"]
    v.customize ["usbfilter", "add", "0",
                 "--target", :id,
                 "--name", "Arduino Micro (bootloader)",
                 "--vendorid", "2341",
                 "--productid", "0037",
                 "--remote", "no"]
    v.customize ["usbfilter", "add", "0",
                 "--target", :id,
                 "--name", "Arduino Micro",
                 "--vendorid", "2341",
                 "--productid", "8037",
                 "--remote", "no"]
  end

  # カレントディレクトリを共有
  config.vm.synced_folder "./", "/qmk_firmware"
  config.vbguest.auto_update = true

  config.vm.provision "shell", privileged: false, inline: <<-SHELL 
    sudo apt-get update
    sudo apt-get install -y python3-pip avrdude
    /qmk_firmware/util/qmk_install.sh
    curl -fsSL https://raw.githubusercontent.com/platformio/platformio-core/master/scripts/99-platformio-udev.rules | sudo -H tee /etc/udev/rules.d/99-platformio-udev.rules
    sudo -H service udev restart
    sudo adduser vagrant dialout
   SHELL
end
