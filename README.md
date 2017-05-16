# mage.libvirt
Ansible role to install and manage kvm/libvirt guests.

## Tips & tricks

```sh
### Create an unmanaged/testing Windows guest:

virt-install --name="kvm-win10" --os-type=windows --network network=default --disk path=/home/user/kvm-win10.qcow2,size=15 --cdrom=/home/user/Win10_1703_Czech_x64.iso  --graphics spice --ram=2048

### Mount and unmount a virt image (read-only) 

mkdir -p /mnt/kvm-win10-mount
sudo guestmount -a /home/user/kvm-win10.qcow2 -m /dev/sda2 --ro /mnt/kvm-win10-mount -v -o allow_other
sudo guestunmount /mnt/kvm-win10-mount

### virsh tips and tricks

virsh list --all --title # show the created guest (and all other guests too)
virsh --readonly dumpxml kvm-win10 # dump xml (works also in manged environments such as those run by ovirt)
virsh dominfo kvm-win10 # show info about kvm-win10 (one of the above listed vms)
virsh domblklist kvm-win10 # show info about kvm-win10 block devices (one of the above listed vms)
virsh net-list # show networks
virsh net-dumpxml default # export xml of default (one of the above listed networks)
virsh net-dhcp-leases default # show ip addresses leased on the 'default' network
```
