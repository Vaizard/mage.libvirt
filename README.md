# mage.libvirt
Ansible role to install and manage kvm/libvirt guests.

## Tips

Create an unmanaged/testing Windows guest:

```
virt-install --name="TestWin10" --os-type=windows --network network=default --disk path=/home/user/testwin10.qcow2,size=15 --cdrom=/home/user/Win10_1703_Czech_x64.iso  --graphics spice --ram=2048
```
