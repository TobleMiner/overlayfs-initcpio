Overlayfs support for initcpio
==============================

This set of initcpio scripts adds support for overlayfs as rootfs to initcpio.

# Installation

Use `sudo ./install` to install the initcpio install and hook scripts


# Configuration

Configuring your system to use an overlayfs as the rootfs is a two-step
process.

## 1. Configure initcpio to use overlayfs

Add overlayfs to HOOKS in `/etc/mkinitcpio.conf`. Put the hook after 'block'.

The resulting HOOKS line could look like this:

```sh
HOOKS="base udev autodetect modconf keyboard block overlayfs encrypt filesystems fsck"
```

## 2. Configure your bootloader to pass the required kernel parameters

The overlayfs initcpio hook needs the following kernel parameters to be present:

 - root: The device containing the base filesystem
 - overlayrw: The device containing the overlay

See `initcpio/install/overlayfs` for more parameters.

How to configure the kernel cmdline depends on your bootloader.
In the case of GRUB a possible GRUB_CMDLINE_LINUX in `/etc/default/grub` might look like this:

```sh
GRUB_CMDLINE_LINUX="root=/dev/sda1 overlayrw=/dev/sda2"
```


# Troubleshooting

You might be hitting the message

`overlayfs: failed to verify upper root origin`

this had me confused for a moment, too. It happens if the overlayfs has been
mounted before with a different lowerdir. In this case you will need to erase upperdir and
workdir.
