Instructions for syzkaller compatibility found here: 
https://github.com/google/syzkaller/blob/master/docs/linux/setup_linux-host_qemu-vm_arm-kernel.md


We will use buildroot to create the disk image. You can obtain buildroot here: https://buildroot.uclibc.org/download.html. Instructions were tested with buildroot c665c7c9cd6646b135cdd9aa7036809f7771ab80. First run:

$> make qemu_arm_vexpress_defconfig
$> make menuconfig

Choose the following options:

    Target packages
	    Networking applications
	        [*] dhcpcd
	        [*] iproute2
	        [*] openssh
    Filesystem images
	        exact size - 1g

Unselect:

    Kernel
	    Linux Kernel

Run make

$> make

Then add the following line to output/target/etc/fstab:

debugfs	/sys/kernel/debug	debugfs	defaults	0	0

Then replace output/target/etc/ssh/sshd_config with the following contents:

PermitRootLogin yes
PasswordAuthentication yes
PermitEmptyPasswords yes

Run make again.

$> make
