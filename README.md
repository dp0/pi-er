# Pi-er

It's pronounced pi-er!

A Raspberry Pi provisioning helper utility. Currently, this provides an Ansible
playbook for writing an image to a local SD card. It makes the assumption that
a Raspbian-like OS is used, and will also enable an sshd server within the OS.

## Example Usage

Firstly, a host must be supplied in the `inventory.yaml` file. At the moment,
no variables within this are used. Note that if multiple hosts are supplied in
the `inventory.yaml` file, then you must limit your playbook execution to a
single host at the moment. For convenience, a single host is provided in the
sample `inventory.yaml` file with this repository.

It is then possible to execute `ansible-playbook write.yaml`. This will ask for
information about the SD card device and the location of the image you want to
use. It will then unmount the SD card in order to safely write the image to it.
Once the playbook has finished, it should be safe to remove the SD card for
use.
