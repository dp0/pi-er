# Pi-er

It's pronounced pi-er!

A Raspberry Pi provisioning helper utility.

Requirements
------------

There are no specific requirements for using pi-er.

Role Variables
--------------

`pi_er_sd_device` - The SD card device to use

`pi_er_image_file` - The image to write to the SD card

Dependencies
------------

This role has no dependencies on any other roles.

Example Playbook
----------------

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
```

License
-------

[BSD 3-Clause](LICENSE.txt)
