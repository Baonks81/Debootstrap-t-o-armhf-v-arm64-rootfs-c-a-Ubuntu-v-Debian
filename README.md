# Debootstrap-tao-rootfs-armhf-va-arm64-tren-Ubuntu-va-Debian
Công cụ để tạo rootfs

Link tham khảo: https://ubuntu-mate.community/t/bootstrapping-ubuntu-mate-for-an-armhf-base/556



Sau khi cài Ubuntu MATE 20.04.4 LTS, có thể dùng debootstrap để tạo armhf/arm64 rootfs của Ubuntu 22.04 LTS hoặc Debian bookworm và nén rootfs.tar.gz hoặc tạo *.img dùng loop để mount rồi debootstrap bên trong image thuận tiện hơn



# sudo su

# D=/path/to/folder

# mkdir -p $D

# apt update && apt upgrade -y

# apt install debootstrap



Ubuntu:

# debootstrap jammy $D http://ports.ubuntu.com



Debian:

debootstrap bookworm $D http://ftp.debian.org/debian



# mount -t proc none $D/proc

# mount -t sysfs none $D/sys

# nano $D/etc/apt/sources.list



Ubuntu:

   deb http://ports.ubuntu.com/ubuntu-ports/ jammy main restricted

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy main restricted



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-updates main restricted

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-updates main restricted



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy universe

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy universe



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-updates universe

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-updates universe



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy multiverse

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy multiverse

   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-updates multiverse

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-updates multiverse



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-backports main restricted universe multiverse

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-backports main restricted universe multiverse



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-security main restricted



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security universe

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-security universe



   deb http://ports.ubuntu.com/ubuntu-ports/ jammy-security multiverse

   # deb-src http://ports.ubuntu.com/ubuntu-ports/ jammy-security multiverse



Debian:

Dùng search-key để đăng ký keys update sources.list



# sudo gpg --keyserver keyserver.ubuntu.com --search-keys admin@mobian-project.org



# sudo gpg --keyserver keyserver.ubuntu.com --search-keys 951D61F2BC232697



# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 393F924A855FB27D



# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys DC30D7C23CBBABEE



# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 73A4F27B8DD47936



# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 4DFAB270CAA96DFA



# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys A48449044AAD5C5D



deb http://repo.mobian-project.org/ bookworm main non-free



deb http://deb.debian.org/debian bookworm main contrib non-free

deb http://security.debian.org/debian-security bookworm-security main contrib non-free

deb http://deb.debian.org/debian bookworm-backports main contrib non-free

deb http://deb.debian.org/debian bookworm-updates main contrib non-free



# chroot $D apt update && apt -y -u dist-upgrade



Ubuntu:

# chroot $D apt -y install ubuntu-standard initramfs-tools language-pack-en



Debian:

# chroot $D apt -y install locales initramfs-tools

# chroot $D tasksel install standard

# chroot $D tasksel --list-tasks



# chroot $D dpkg-reconfigure tzdata

# chroot $D dpkg-reconfigure locales



en_US.UTF-8



# cp /etc/resolv.conf $D/etc/

# chroot $D apt -y install lxqt-core sddm

Hoặc:

# chroot $D apt -y install xubuntu-core xdm

Hoặc:

# chroot $D apt -y install ubuntu-mate-desktop lightdm

Hoặc:

# chroot $D apt -y install phosh phosh-mobile-tweaks

# chroot $D apt -y install sudo nano bluez blueman htop neofetch iio-sensor-proxy perl x11-xserver-utils xinput rfkill network-manager dhcpcd5 iwd onboard cpufrequtils alsa-base alsa-utils oem-config-slideshow-ubuntu-mate users-admin lightdm-settings



# sudo nano $D/etc/fstab



LABEL=pmOS_root      /               ext4    defaults,noatime            0   0

LABEL=pmOS_boot      /boot           ext2    defaults                            0   1

tmpfs                /tmp            tmpfs   defaults,noatime,mode=1777          0   0

tmpfs                /var/tmp        tmpfs   defaults,noatime,mode=1777          0   0

tmpfs                /var/spool      tmpfs   defaults,noatime,mode=1777          0   0

tmpfs                /var/log        tmpfs   defaults,noatime,mode=0755          0   0



# nano $D/etc/hostname



   nexus7-tablet



# nano $D/etc/hosts



   127.0.0.1 localhost

   127.0.1.1 nexus7-tablet



# nano $D/etc/environment



   QT_IM_MODULE=qtvirtualkeyboard

   QT_QUICK_BACKEND=software

   LIBGL_ALWAYS_SOFTWARE=1

   QT_AUTO_SCREEN_SCALE_FACTOR=2

   #XCURSOR_SIZE=32

   MOZ_USE_XINPUT2=1

   MOZ_X11_EGL=1



# nano ~/.profile



   unset QT_IM_MODULE



Set root's passwd

# chroot $D passwd



Ubuntu:

Create oem user and passwd

# chroot $D useradd -mG sudo oem

# chroot $D passwd oem

New passwd: ubuntu

# chroot $D usermod -a -G adm,dialout,cdrom,audio,video,plugdev,dip,root,input ubuntu



Debian:

Create mobian user and passwd

# chroot $D useradd -mG sudo mobian

# chroot $D passwd mobian

New passwd: 1234

# chroot $D usermod -a -G adm,dialout,cdrom,audio,video,plugdev,dip,sudo,root,input mobian



# cp /etc/network/interfaces $D/etc/network/

# sudo mkdir -p $D/etc/network/interfaces.d



Kernel-5.15.0-rc4 copy all folder to rootfs

# cp -r /etc/* $D/etc/

# cp -r /lib/* $D/lib/

# cp -r /usr/* $D/usr/

# cp -r /var/* $D/var/



# umount $D/proc

# umount $D/sys



# cd $D



Ubuntu:

# tar cvfz /home/[username]/ubuntu-22.04-MATE-armhf+asus-grouper.tar.gz *



Debian:

# tar cvfz /home/[username]/debian-bookworm-MATE-armhf+asus-grouper.tar.gz *



Tạo rootfs.img như trong bài Ubuntu 22.04 LXQT để bung # tar xvfz *.tar.gz vào pmOS_root và chép vmlinuz-* vào pmOS_boot và flash bằng adb push *.img /dev/block/mmcblk0p-- (9: grouper - wifi, 10: tilapia - 3G rev. E1565)



Link image Ubuntu LXQT armhf tham khảo:

https://drive.google.com/drive/folders/1GRrZKMLB10q90PdJCnA8VaI_5w0YatZx



Link Debian base-system arm64 tham khảo:

https://drive.google.com/drive/folders/1-A9paOPd0m-ygeyd7CWapfQi6zuz3m0w
