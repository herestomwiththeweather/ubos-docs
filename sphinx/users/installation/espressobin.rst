Run UBOS on ESPRESSObin
=======================

You can run UBOS on your `ESPRESSObin <https://espressobin.net/>`_ by downloading an image,
writing it to an SD card, and booting your ESPRESSObin with that card. (Alternatively you
can keep running your existing Linux distro on your ESPRESSObin, and run UBOS in a Linux
container. This is :doc:`described here <aarch64_container>`.)

#. Download a UBOS boot image from ``depot.ubos.net``.
   Beta images for the ESPRESSObin are at
   `http://depot.ubos.net/yellow/aarch64/images <http://depot.ubos.net/yellow/aarch64/images>`_.
   Look for a file named ``ubos_yellow-aarch64-espressobin_LATEST.img.xz``.

#. Optionally, you may now verify that your image downloaded correctly by following the instructions
   at :doc:`verifying`.

#. Uncompress the downloaded file. This depends on your operating system, but might be as easy as
   double-clicking it, or executing

   .. code-block:: none

      % sudo xz -d ubos_yellow_aarch64-espressobin_LATEST.img.xz

   on the command line.

#. Write this image file "raw" to an SD card appropriate for your ESPRESSObin. This
   operation depends on your operating system:

   * :doc:`/users/writing-image/windows`
   * :doc:`/users/writing-image/macosx`
   * :doc:`/users/writing-image/linux`

#. On first boot, you need to have a serial terminal connected to your ESPRESSObin. This is
   because you likely have to change your boot loader options.

#. Remove the SD card and insert it into your ESPRESSObin. If you created a UBOS staff,
   insert the staff into the USB port. Then, connect the ESPRESSObin's USB power port to
   your computer.

#. From your computer, attach a serial terminal. How to do that depends on your operating
   system. The ESPRESSObin site has a
   `description <http://wiki.espressobin.net/tiki-index.php?page=Serial+connection>`_ how to
   do this for Windows and Linux. The baudrate is 115200.

#. Connect the 12V power supply to your ESPRESSObin.

#. When prompted on the serial terminal, interrupt the boot process by pressing a key. You
   get a promot that looks like:

   .. code-block:: none

      Marvell>>

#. Enter the following commands to import the new environment variables to boot from the SD card:

   .. code-block:: none

      mmc dev 0
      ext4load mmc 0 $loadaddr /uEnv-sdcard.txt
      env import -t $loadaddr $filesize
      saveenv
      boot

   If you do not want to make permanent changes to your bootloader setup, leave out the
   ``saveenv`` command. If you do not execute ``saveenv``, you will have to type the
   commands above every time you boot your ESPRESSObin into UBOS.

#. Should booting fail, see below for the uBoot factory configuration.

#. When the boot process is finished, log in as user ``root`` from the attached keyboard
   and monitor. By default, there is no password on the console. If you used a UBOS staff,
   you can log in over the network instead as described :doc:`here <../shepherd-staff>`.

#. Now: wait. UBOS needs to generate a few cryptographic keys before it is ready to use
   and initialize a few other things on the first boot. That might take 5 or 10 minutes.
   To determine whether UBOS ready, execute:

   .. code-block:: none

      % systemctl is-system-running

   Wait until the output has changed from ``starting`` to ``running``. If it is anything else, consult
   :doc:`troubleshooting<../troubleshooting>`.

#. If you have Ethernet plugged in, and your network has a DHCP server (most networks do),
   your computer should automatically acquire an IP address. You can check with:

   .. code-block:: none

      % ip addr

   Make sure you are connected to the internet before attempting to proceed. In the default setup,
   the Ethernet port closest to the ESPRESSObin's USB 3 port (the one that is blue) is set up as
   "upstream" interface, and the other two can manage local area networks, just like a home
   router.

#. Update UBOS to the latest and greatest:

   .. code-block:: none

      % sudo ubos-admin update

#. You are now ready to :doc:`set up your first app and site </users/firstsite>`.

Optional: boot from a SATA disk, instead of an SD card
------------------------------------------------------

In the previous section, you installed UBOS on an SD card and booted from it. If you would
like to use a SATA disk instead, do this:

#. Acquire a suitable SATA power connector. The ESPRESSObin has a male power connector on
   the board, which is very unusual. It may be difficult to find a suitable power
   connector.

#. Assuming you have the disk connected and powered up, boot the ESPRESSObin from a
   UBOS SD card as described above.

#. Once booted, execute:

   .. code-block:: none

      % lsblk

   This will show all attached block devices, including the attached disk. Determine which
   of the shown devices is your disk. It might be ``/dev/sda``, which we'll assume from
   now.

#. Install UBOS on that disk with the command:

   .. code-block:: none

      % sudo ubos-install /dev/sda

#. Shut down the ESPRESSObin and turn off power.

#. Remove the SD card from your ESPRESSObin. If you created a UBOS staff,
   insert the staff into the USB port. Then, connect the ESPRESSObin's USB power port to
   your computer.

#. From your computer, attach a serial terminal. How to do that depends on your operating
   system. The ESPRESSObin site has a
   `description <http://wiki.espressobin.net/tiki-index.php?page=Serial+connection>`_ how to
   do this for Windows and Linux. The baudrate is 115200.

#. Connect the 12V power supply to your ESPRESSObin.

#. When prompted on the serial terminal, interrupt the boot process by pressing a key. You
   get a promot that looks like:

   .. code-block:: none

      Marvell>>

#. Enter the following commands to import the new environment variables to boot from the SD card:

   .. code-block:: none

      scsi scan
      scsi dev 0
      ext4load scsi 0 $loadaddr /uEnv-sata.txt
      env import -t $loadaddr $filesize
      saveenv
      boot

   If you do not want to make permanent changes to your bootloader setup, leave out the
   ``saveenv`` command.

#. The ESPRESSObin will now boot from the SATA disk. If you have executed the ``saveenv``
   command, you can remove the SD Card; it will not be needed for future boots.

UBoot bootloader factory configuration
--------------------------------------

If you have difficulty booting the ESPRESSObin with the provided instructions, it may
be because you previously changed the ESPRESSObin's boot loader configuration from
the factory default. For reference, here is the ESPRESSObin's factory configuration
as determined by executing

.. code-block:: none

   > printenv

in uBoot of a brand-new device:

.. code-block:: none

   baudrate=115200
   bootargs=console=ttyMV0,115200 earlycon=ar3700_uart,0xd0012000 root=/dev/nfs rw ip=0.0.0.0:0.0.0.0:10.4.50.254:255.255.255.0:marvell:eth0:none nfsroot=0.0.0.0:/srv/nfs/
   bootcmd=mmc dev 0; ext4load mmc 0:1 $kernel_addr $image_name;ext4load mmc 0:1 $fdt_addr $fdt_name;setenv bootargs $console root=/dev/mmcblk0p1 rw rootwait; booti $kernel_addr - $fdt_addr
   bootdelay=3
   bootmmc=mmc dev 0; ext4load mmc 0:1 $kernel_addr $image_name;ext4load mmc 0:1 $fdt_addr $fdt_name;setenv bootargs $console root=/dev/mmcblk0p1 rw rootwait; booti $kernel_addr - $fdt_addr
   console=console=ttyMV0,115200 earlycon=ar3700_uart,0xd0012000
   eth1addr=00:00:00:00:51:82
   eth2addr=00:00:00:00:51:83
   ethact=neta0
   ethaddr=F0:AD:4E:03:6A:EA
   ethprime=egiga0
   fdt_addr=0x1000000
   fdt_high=0xffffffffffffffff
   fdt_name=boot/armada-3720-community.dtb
   fileaddr=2000000
   filesize=400000
   gatewayip=10.4.50.254
   get_images=mmc dev 0; fatload mmc 0 $kernel_addr $image_name; fatload mmc 0 $fdt_addr $fdt_name; run get_ramfs
   get_ramfs=if test "${ramfs_name}" != "-"; then setenv ramfs_addr 0x3000000; tftp $ramfs_addr $ramfs_name; else setenv ramfs_addr -;fi
   hostname=marvell
   image_name=boot/Image
   initrd_addr=0xa00000
   initrd_size=0x2000000
   ipaddr=10.4.50.4
   kernel_addr=0x2000000
   loadaddr=0x2000000
   loads_echo=0
   netdev=eth0
   netmask=255.255.255.0
   ramfs_addr=-
   ramfs_name=-
   root=root=/dev/mmcblk0p1 rw
   rootpath=/srv/nfs/
   serverip=10.4.50.5
   set_bootargs=setenv bootargs $console $root ip=$ipaddr:$serverip:$gatewayip:$netmask:$hostname:$netdev:none nfsroot=$serverip:$rootpath $extra_params
   stderr=serial
   stdin=serial
   stdout=serial

(Some of these values will necessarily be different on your device, e.g. the Mac
addresses.)

In an attempt to trouble-shoot, manually set the environment variables in your device's
uBoot configuration to these values as closely as possible, before attempting to boot
UBOS.
