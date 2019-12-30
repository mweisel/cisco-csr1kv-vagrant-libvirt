A procedure for creating a Cisco CSR 1000V Vagrant box for the [libvirt](https://libvirt.org) provider.

## Prerequisites

  * [Git](https://git-scm.com)
  * [Python](https://www.python.org)
  * [Ansible](https://docs.ansible.com/ansible/latest/index.html)
  * [libvirt](https://libvirt.org) with client tools
  * [QEMU](https://www.qemu.org)
  * [Expect](https://en.wikipedia.org/wiki/Expect)
  * [Telnet](https://en.wikipedia.org/wiki/Telnet)
  * [Vagrant](https://www.vagrantup.com)
  * [vagrant-libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt)

## Steps

0. Verify the prerequisite tools are installed.

```
which git python ansible libvirtd virsh qemu-system-x86_64 expect telnet vagrant
vagrant plugin list
```

1. Clone this GitHub repo and _cd_ into the directory.

```
git clone https://github.com/mweisel/cisco-csr1kv-vagrant-libvirt
cd cisco-csr1kv-vagrant-libvirt
```

2. Log in and download the Cisco Cloud Services Router 1000V (csr1000v-\*-serial.qcow2) software from your [Cisco](https://software.cisco.com/download/home/284364978/type) account.

3. Copy (and rename) the disk image file to the `/var/lib/libvirt/images` directory.

```
sudo cp $HOME/Downloads/csr1000v-universalk9.16.09.04-serial.qcow2 /var/lib/libvirt/images/cisco-csr1kv.qcow2
```

4. Modify the file ownership and permissions. Note the owner will differ between Linux distributions. A couple of examples:

> Arch Linux
```
sudo chown nobody:kvm /var/lib/libvirt/images/cisco-csr1kv.qcow2
sudo chmod u+x /var/lib/libvirt/images/cisco-csr1kv.qcow2
```

> Ubuntu 18.04
```
sudo chown libvirt-qemu:kvm /var/lib/libvirt/images/cisco-csr1kv.qcow2
sudo chmod u+x /var/lib/libvirt/images/cisco-csr1kv.qcow2
```

5. Start the `vagrant-libvirt` network (if not already started).

```
virsh -c qemu:///system net-list
virsh -c qemu:///system net-start vagrant-libvirt
```

6. Run the Ansible playbook. 

```
ansible-playbook main.yml
```

7. Add the Vagrant box. 

```
vagrant box add --provider libvirt --name cisco-csr1000v-16.09.04 ./cisco-csr1kv.box
```

## Debug

To view the telnet session output for the `expect` task:

```
tail -f ~/csr1kv-console.explog
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
