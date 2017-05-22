# mage.libvirt
Ansible role to install and manage kvm/libvirt guests.

## Tips & tricks

```sh
### Set up a Windows guest:

# fetch a Windows 10 ISO from https://www.microsoft.com/cs-cz/software-download/windows10ISO

# set up a basic/testing vm in your home dir
virt-install --name="vm-win10" --os-type=windows --network network=default --disk path=/home/user/vm-win10.qcow2,size=15 --cdrom=/home/user/Win10_1703_Czech_x64.iso  --graphics spice --ram=2048

# set up a production/high performance vm (use viostor driver during windows install to detect disks)
virt-install --name="vm-win10" --os-type=windows --os-variant=win10 --network network=default,model=virtio --disk path=/var/lib/libvirt/images/vm-win10.qcow2,size=18,bus=virtio --disk path=/var/lib/libvirt/images/share.qcow2,size=5,bus=virtio --disk path=/var/lib/libvirt/images/iso/Win10_1703_Czech_x64.iso,device=cdrom --cdrom=/var/lib/libvirt/images/iso/virtio-win.iso --graphics spice,listen=0.0.0.0 --ram=3072 --vcpus=2 --noautoconsole
# High performance virtio drivers are in use. You will need to downloaded the ISO with the drivers (see https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso). While setting up disks and partitions, install the viostor driver to allow the Windows installer to see the virtio disks. After booting into the system for the first time, ensure to install the rest of the virtio drivers - look at the device management launched bz `mmc devmgmt.msc` to see which devices require a propper driver.


### Mount and unmount a virt image (read-only) 

mkdir -p /mnt/vm-win10-mount
sudo guestmount -a /home/user/vm-win10.qcow2 -m /dev/sda2 --ro /mnt/vm-win10-mount -v -o allow_other
sudo guestunmount /mnt/vm-win10-mount

### Resize VM's volume

virsh blockresize vm-win10 /home/user/vm-win10.qcow2 17G # safe to do on a running VM (win required a reboot nontheless to see the new space
virsh vol-resize /home/killua/vm-win10.qcow2 19G # safe on powered down VM
# don't forget to resize the guest's fs as well (on windows guests use diskmgmt.msc)

### virsh tips and tricks

virsh list --all --title # show the created guest (and all other guests too)
virsh --readonly dumpxml vm-win10 # dump xml (works also in manged environments such as those run by ovirt)
virsh dominfo vm-win10 # show info about vm-win10 (one of the above listed vms)
virsh domblklist vm-win10 # show info about vm-win10 block devices (one of the above listed vms)
sudo virsh pool-list
sudo virsh vol-list --pool default
sudo virsh vol-dumpxml --pool default testwin10.qcow2
virsh net-list # show networks
virsh net-dumpxml default # export xml of default (one of the above listed networks)
virsh net-dhcp-leases default # show ip addresses leased on the 'default' network
virsh edit vm-win10 # edit the virtual machine
virt-viewer --connect qemu+ssh://<user>@<server>:<ssh_port>/system guest # connect to a remote console over ssh
```
