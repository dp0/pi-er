# Pi-er

It's pronounced pi-er!

A Raspberry Pi provisioning helper utility.

Requirements
------------

There are no specific requirements for using pi-er.

Role Variables
--------------

`pi_er_sd_device` - The SD card device to use

`pi_er_sd_ptuuid` - (OPTIONAL) The disk identifier (PTUUID) of the target SD
card.

`pi_er_image_file` - The image to write to the SD card

`pi_er_pub_key` - The SSH public key that can be used to connect to the Pi
after provisioning.

`pi_er_netconf` - (OPTIONAL) The static IP configuration.

Dependencies
------------

This role has no dependencies on any other roles.

Example Playbook
----------------

A simple example can be found below, which shows the general idea of how pi-er
can be used.

```
- hosts: all
  gather_facts: no
  serial: 1
  connection: local
  become: yes
  tasks:
    - name: provision sd cards
      include_role:
        name: 'pi-er'
      vars:
        pi_er_sd_device: /dev/mmcblk0
        pi_er_image_file: /home/dp0/images/2019-09-26-raspbian-buster-lite.img
        pi_er_pub_key: /home/dp0/.ssh/id_rsa.pub
```

You most likely don't want to hardcode your `pi_er_sd_device` value, and
probably want to specify this at runtime.

```
- hosts: all
  gather_facts: no
  serial: 1
  connection: local
  become: yes
  tasks:
    - name: provision sd cards
      include_role:
        name: 'pi-er'
      var:
        pi_er_image_file: /home/dp0/images/2019-09-26-raspbian-buster-lite.img
        pi_er_pub_key: /home/dp0/.ssh/id_rsa.pub
```

And it can be run (for host `rpihost1` with an innvocation like:

```
$ ansible-playbook -K -l rpihost1 -e 'pi_er_sd_device=/dev/mmcblk0' sd.yaml
```

You may also wish to consider enforcing a per-host PTUUID as well, and this can
be achieved by passing a `pi_er_sd_ptuuid` value.


PTUUID Example
--------------

If you wish to use PTUUID checking, you will need to ensure your SD card has a
unique PTUUID. If you have ever copied a disk image to your SD card, you may
find that it has also copied the PTUUID from the image, so you may need to
generate a new PTUUID.

**Generate a new PTUUID** by creating a new MBR/MSDOS partition table:
```
$ sudo fdisk /dev/your_sd_card_device

Command (m for help): o
Created a new DOS disklabel with disk identifier 0x9cbe0348.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Synching disks.
```

The PTUUID generated (in the above case, it is `9cbe0348`, can now be passed
using `pi_er_sd_ptuuid`, and writing to the `pi_er_sd_device` will be rejected
unless the PTUUID matches.

If you want to retrieve the PTUUID of a device, you can use `fdisk` again:
```
$ sudo fdisk -l /dev/sdc | grep identifier
Disk identifier: 0x9cbe0348
```

Static IP Example
-----------------

Where you want a static IP, you can simply use `pi_er_netconf`, which is a
dictionary where keys are network interfaces, and values map to dchpcd
configuration options.

An example of a `pi_er_netconf` setting is:
```
pi_er_netconf:
  eth0:
    ip: 192.168.0.123/24
    routers:
      - 192.168.0.1
    dns:
      - 1.1.1.1
      - 1.0.0.1
```

*Hint: the `ip6` option is also available for setting a static IPv6 address.*

License
-------

[BSD 3-Clause](LICENSE.txt)
