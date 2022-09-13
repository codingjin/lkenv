# lkenv

# 1 prepare the packages

sudo apt update

sudo apt upgrade

sudo apt install -y build-essential cmake libssl-dev bison flex qemu qemu-utils debootstrap

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


  


