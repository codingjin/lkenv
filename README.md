# lkenv

# 1 prepare the packages

sudo apt update

sudo apt upgrade

sudo apt install -y build-essential cmake libssl-dev bison flex qemu qemu-utils qemu-system-x86 debootstrap

sudo apt update

# 2 Download the Linux Source

wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.19.8.tar.xz

tar -xvf linux-5.19.8.tar.xz

# 3 Compile the Linux kernel

cd linux-5.19.8

make x86_64_defconfig

make kvm_guest.config

make menuconfig 

  (config the E1000)

  Location: Device Drivers -> Network device support ->Ethernet driver support -> Intel devices

  Set all the options under Intel devices into “y/*”

![image](https://user-images.githubusercontent.com/55301130/189800700-656a4597-18ad-4384-8009-d10010caaf8f.png)

  (config virtio_balloon)
  
  Main menu -> Device Drivers -> Virtio drivers (Virtio balloon driver [=y] )
  
 (compile)
 
 make -j$(nproc)

(copy the bzImage out)

cp arch/x86/boot/bzImage ../

# 4 Create the QEMU image

qemu-img create qemuimg1 32g

mkfs.ext4 qemuimg1 

mkdir mountdir1

sudo mount qemuimg1 mountdir1

sudo debootstrap --arch amd64 focal mountdir1

cd mountdir1

sudo chroot .

passwd

sudo apt update; sudo apt upgrade; sudo apt install -y wget unzip git

(Get the Ubuntu 20 source list, https://gist.github.com/ishad0w/788555191c7037e249a439542c53e170, and write it into /etc/apt/sources.list)

sudo apt update; sudo apt upgrade

sudo apt install -y curl tar gcc make time flex bison python-dev libelf-dev libaudit-dev libslang2-dev libperl-dev binutils-dev liblzma-dev libnuma-dev vim screen usbutils build-essential cmake libssl-dev openssh-server numactl network-manager net-tools ifupdown


# 5 Run the VM

sudo qemu-system-x86_64 -kernel bzImage -nographic -drive format=raw,file=qemuimg1 -append "root=/dev/sda rw console=ttyS0" \
-m 100G -cpu host -enable-kvm -smp 32 -machine ubuntu,accel=kvm \
-device e1000,netdev=net0 -netdev user,id=net0,hostfwd=tcp::5556-:22

(Configure sshd)

vi /etc/ssh/sshd_config, to set [PermitRootLogin yes]

(Configure Internet)

ifconfig -a

![image](https://user-images.githubusercontent.com/55301130/189829330-cd290a07-aee2-46aa-bff0-ebca699e5f02.png)

(in-guest) echo "iface enp0s3 inet dhcp" >> /etc/network/interfaces

(in-guest, add this into ~/.bashrc) ifup enp0s3     

(in-guest) ping -c2 www.google.com

(Configure ssh server. Edit /etc/ssh/sshd_config 's PermitRootLogin yes; service sshd restart )

(From host machine, ssh root@localhost -p 5556. Now you should ssh-connect to the vm )





  


