# -*- mode: ruby -*-
# vi: set ft=ruby :

# Based on:
#  https://brettops.io/blog/custom-raspberry-pi-image-no-hardware/
#  https://raspberrypi.stackexchange.com/questions/13137/how-can-i-mount-a-raspberry-pi-linux-distro-image
#  https://www.raspberrypi.com/news/raspberry-pi-bullseye-update-april-2022/
$script = <<-SCRIPT

# download and decompress the raspios
wget -q $1
sudo apt-get install -y xz-utils
unxz -v $2

# resize image
sudo apt-get install -y qemu-utils
sudo qemu-img resize -f raw $3 +100M
sudo growpart $3 2

# mount image
mkdir rootfs
DEVICE=$(sudo losetup -f --show -P $3)
sudo mount ${DEVICE}p2 ./rootfs/
sudo mount ${DEVICE}p1 ./rootfs/boot

# enable SSH
sudo touch ./rootfs/boot/ssh

# create default user with username:pi and password:raspberry
echo 'pi:'$(echo 'raspberry' | openssl passwd -6 -stdin) > ./rootfs/boot/userconf

# close image
sudo umount rootfs/boot
sudo umount rootfs
sudo losetup -d $DEVICE

cp $3 ./sync
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 3
    vb.memory = "4096"
  end
  config.ssh.insert_key = false
  config.vm.synced_folder './sync', '/home/vagrant/sync'
  config.vm.provision "shell" do |s|
    s.inline = $script
    s.args   = [
      "https://downloads.raspberrypi.com/raspios_lite_arm64/images/raspios_lite_arm64-2023-12-11/2023-12-11-raspios-bookworm-arm64-lite.img.xz",
      "2023-12-11-raspios-bookworm-arm64-lite.img.xz",
      "2023-12-11-raspios-bookworm-arm64-lite.img"
    ]
  end
end