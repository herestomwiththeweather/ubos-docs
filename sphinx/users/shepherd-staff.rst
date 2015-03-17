The UBOS Staff
==============

Overview
--------

Any standard USB flash drive can be used to securely initialize devices running UBOS
during boot. We call such a device the "UBOS Staff".

Today, this allows the user (the :term:`Shepherd`) to automatically
provision an ``ssh`` account for key-based administrator login on the device. In the future,
additional configuration information may be supported.

See also `blog post <http://upon2020.com/blog/2015/03/ubos-shepherd-rules-their-iot-device-flock-with-a-staff/>`_
with a longer discussion.

UBOS Staff device configuration
-------------------------------

Any standard USB flash drive can be used as the UBOS staff. It is recommended that such a
USB flash drive only be used as UBOS staff, and not for other purposes at the same time.

The drive must have a "VFAT" ("Windows") partition called ``UBOS-STAFF``; otherwise
UBOS will ignore the drive during boot. This can often be accomplished simply by inserting
a new USB flash drive in a computer, and renaming the device to ``UBOS-STAFF``.

UBOS expects configuration information in the following file system layout::

   shepherd/
       ssh/
           id_rsa.pub

I.e., a file called ``id_rsa.pub`` must be contained in a directory named ``ssh``, which
in turn must be contained in a directory called ``shepherd`` at the root level of the
directory hierarchy.

This file ``id_rsa.pub`` must contain a valid ``ssh`` public key. You can use any existing
``ssh`` public key for which you have the corresponding private key.

To generate a new ``ssh`` key pair, execute the following command on a computer that you
trust to keep your private key secret::

   > ssh-keygen

and answer the questions. This will generate two files in the directory you specified:
``id_rsa`` and ``id_rsa.pub``. The first one you want to secure, because anybody who has
that file will be able to log into your device as the shepherd. The second one is the file
you want to write to your Staff USB drive. It is not necessary to keep that file secure.

To log into a remote UBOS device as the shepherd
------------------------------------------------

On the computer that has the private ``id_rsa`` file, execute the following command::

   > ssh -i id_rsa shepherd@ip

where ``id_rsa`` is the private file from above, and ``ip`` is the IP address or
hostname of your device, such as ``ubos-pc.local`` (see :doc:`networking`)

UBOS boot behavior with Staff present
-------------------------------------

When UBOS boots, UBOS checks for the presence of a disk with a partition named
``UBOS-STAFF``. If it detects such a disk, it looks for the ``id_rsa.pub`` file in the
location described above.

If UBOS finds such a file, UBOS:

1. Creates a Linux user called ``shepherd`` unless it exists already.

2. Saves the content of ``id_rsa.pub`` verbatim as ``~shepherd/.ssh/id_rsa.pub``. This
   means that the user can log into the device over the network, as user ``shepherd``,
   as long as the user uses the corresponding private key for authentication.

3. UBOS gives the ``shepherd`` account certain administration rights, notably the
   ability to invoke ``sudo ubos-admin``.
