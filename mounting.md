#File systems and Understanding Mounting
- A mount is connection between device and a directory. A Linux File system is presented as one hierarchy, with the root directory (/) as it's starting point.  This hierarchy
may be distributed over different devices and even computer systems that
are mounted into the root directory.
In the process of mounting, a device is connected to a specific directory,
such that after a successful mount, this directory gives access to the device
contents. Without mounting a disk device, it cannot be accessed.
Mounting devices makes it possible to organize the Linux file system in a
flexible way. There are several disadvantages to storing all files in just one
file system, which gives several good reasons to work with multiple
mounts:
• High activity in one area may fill up the entire file system, which will
negatively impact services running on the server.
• If all files are on the same device, it is difficult to secure access and
distinguish between different areas of the file system with different
security needs. By mounting a separate file system, you can add mount
options to meet specific security needs, such as the noexec option,
which disallows running any executable file, which may make sense in
user home directories.
• If a one-device file system is completely filled, it may be difficult to make additional storage space available.

- To avoid these pitfalls, it is common to organize Linux file systems in
different devices (and even remote file shares on other computer systems),
such as disk partitions and logical volumes, and mount these devices into
the file system hierarchy. By configuring a device as a dedicated mount,
you also are able to use specific mount options that can restrict access to the
device. 
Some directories are commonly mounted on dedicated devices:
• /boot: This directory is normally mounted on a separate device
because it requires essential information your computer needs to boot.
Because the root directory (/) is often on a Logical Volume Manager
(LVM) logical volume, from which Linux cannot boot by default, the
kernel and associated files need to be stored separately on a dedicated
/boot device.

• /boot/EFI: If a system uses Extensible Firmware Interface (EFI) for
booting, a dedicated mount is required, giving access to all
configuration required in the earliest stage of the boot procedure.

• /var: This directory is often on a dedicated device because it grows in
a dynamic and uncontrolled way (for example, because of the log files
that are written to /var/log). By putting it on a dedicated device, you
can ensure that it will not fill up all storage on your server.

• /home: This directory often is on a dedicated device for security
reasons. By putting it on a dedicated device, you can mount it with
specific options, such as noexec and nodev, to enhance the security of
the server. When you are reinstalling the operating system, it is an
advantage to have home directories in a separate file system. The home
directories can then survive the system reinstall.

• /usr: This directory contains operating system files only, to which
normal users normally do not need any write access. Putting this
directory on a dedicated device allows administrators to configure it as
a read-only mount.

 ---
 More Notes : 
Apart from these directories, you may find servers that have other
directories that are mounted on dedicated partitions or volumes also. After all, it is up to the discretion of the administrator to decide which directories
get their own dedicated devices.
To get an overview of all devices and their mount points, you can use
different commands:
• The mount command gives an overview of all mounted devices. To
get this information, the /proc/mounts file is read, where the kernel
keeps information about all current mounts. It shows kernel interfaces
also, which may lead to a long list of mounted devices being
displayed.
---
Basically loaded Devices to the directory tree. 