## Goal
To make it easy to create virtual machines in qemu almost as easy as [devenv project](https://github.com/coopdevs/devenv/) does with linux containers.

## Host OS where this focused on
* Debian 9
* Ubuntu 18.04

## General references

* [Debian wiki on KVM, qemu and libvirt](https://wiki.debian.org/KVM)
* [Libvirt wiki](https://wiki.libvirt.org/page/Main_Page)

---

## Check for hardware virtualization

Intel and AMD will show different flags to announce hardware virtualization support. Both are used by KVM, Xen and others to do a "full virtualization", that is, hardware based and not software based.

As they show in [cyberciti.biz](https://www.cyberciti.biz/faq/linux-xen-vmware-kvm-intel-vt-amd-v-support/), we have compatible hardware.
```
egrep -q 'vmx|svm' /proc/cpuinfo &&
echo "Hardware virtualization is supported" ||
echo "Hardware virtualization is NOT supported"
```
There are secondary flags as explained in the already referenced article.

## Install packages from repos. Other distros may have different package names.

`apt install qemu-kvm libvirt-clients libvirt-daemon-system virt-manager`

## Download the desired guest OS image

We chose to use "cloud" images because they are in qcow2 format, qemu's native format. At the same time, it's used on public clouds, so it may ease moving between environments (local and remote).

* ubuntu https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
* debian https://cdimage.debian.org/cdimage/openstack/current/

As of June 2019, we are using Ubuntu in most of our deployments

## Prepare permissions

To be able to run and manage vm's with non-root user, we need to ensure that our user joins a couple of additional groups, that the daemon runs from the desired user/group and the property of a file. This should fix any "permission denied" when trying to connect or run a vm or "domain".

### Groups

```
# adduser <youruser> libvirt
# adduser <youruser> libvirt-qemu
```

### Config

```bash
# ~/.local/etc/libvirt/qemu.conf
user = "<youruser>"
group = "libvirt-qemu"
```

### KVM device

`sudo chown :kvm /dev/kvm`

### Images

```bash
chown <youruser>:libvirt-qemu .local/var/lib/libvirt/images/
chmod 770 .local/var/lib/libvirt/images/
```
Note: default directory is `/var/lib/libvirt/images`, but we prefer to use a directory local to our user. Later on, from the UI we will have to add the 

## Configure credentials for the cloud image

_You can skip this step if you downloaded a regular USB installation image._

Cloud-init is a standard that "handles early initialization of a cloud instance", setting up from user and groups, to execution of chef and puppet recipes. However, we only want to change the default and unknown password to another one. Think if it's more convenient in your case to set up an ssh key.

Cloud init is also a service that is installed by default at cloud images. It checks for external media (virtual or not) at start up, and if it finds devices such as CD-ROM suitable to its format, it will apply the recorded configuration.

We will need an official tool to convert text config files to CD-ROM image.

```bash
apt install cloud-image-utils
```
To configure a password login

```bash
cat << EOF > cloud-init.conf
#cloud-config
password: unsafepassword
chpasswd: { expire: False } 
ssh_pwauth: True
EOF
```

To  configure an SSH certificate login

```bash
cat << EOF > cloud-init.conf
#cloud-config
ssh_authorized_keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAGEA3FSyQwBI6Z+THISisANexampleXlukKoUPND/RRClWz2s5TCzIkd3Ou5+Cyz71X0XmazM3l5WgeErvtIwQMyT1KjNoMhoJMrJnWqQPOt5Q8zWd9qG7PBl9+eiH5qV7NZ mykey@host
EOF
```
Once we've got the config file, only remains converting it to CD image.
```bash
cloud-localds cloud-init.img cloud-init.conf
cp cloud-init.img ~/.local/var/lib/libvirt/images/
```

## Gather all pieces together: create the "domain" for libvirt

As qemu by itself doesn't store vm's description and settings, libvirt does this job for us. We can use a one shot `virt-install` command or use the convenient GUI.

**WARNING** We don't know yet how to create the image with more virtual space. It looks like `size` parameter of `--disk ...,size=XX` is ignored with `--import`.

1. Check that there's a "connection" with QEMU/KVM
2. Create new image
3. Import from an existing image, and select your qcow2 image
4. Network NAT (the default one)
5. Edit before starting
6. Add new hardware → Storage → select an existing image: the cloud-init.img ; device type: cd-rom → Apply/save/end
7. Start vm

Alternatively, via cli:

```bash
virt-install \
 --name ubuntu-bionic \
 --memory 4000 \
 --vcpus 2 \
 --import \
 --disk path=/path/to/bionic-server-cloudimg-amd64.img,format=qcow2,bus=virtio,size=10 \
 --disk path=/path/to/cloud-init.img,device=cdrom \
 --network bridge=virbr0,model=virtio \
 --os-type=linux \
 --os-variant=ubuntubionic \
 --connect qemu:///system \
 --noautoconsole
```

### How to attach cloud-init cd-rom if domain is already installed or created

```
# virsh
> connect qemu:///system
> list --all
> start debian9
> attach-disk debian9 /path/to/cloud-init.img sdb
> reboot debian9
> console debian9
```

Once started the vm with the cloud init image attached, we will be able to log in with the credentials we set in the cloud-init config file :)

### Troubleshooting slow start

El paquet cloud-init està instaŀlat a la màquina host. El primer cop està bé que hi sigui, perquè ens permet canviar configuració, però després molestarà molt perquè ralenteix una barbaritat l'arrencada. Systemd calcula quant ha trigat cada servei en cadena:

There may be cases where cloud-init delays the OS booting a long time. The first time, when the config device is attached, it won't delay and it will just work. However, in subsequent vm boots, we may find ourselves with this:

```
ferran@debian:~$ sudo systemd-analyze blame
    4min 34.162s cloud-init.service
          1.513s cloud-init-local.service
           922ms cloud-config.service
           840ms docker.service
           625ms cloud-final.service
           467ms dev-vda1.device
           226ms networking.service
           186ms ntp.service
           146ms ssh.service
           116ms systemd-udevd.service
           108ms systemd-journald.service
           100ms systemd-journal-flush.service
           100ms systemd-udev-trigger.service
```
This is cloud-init staring at the clouds instead of saying "no config device here, my job's done!". Therefore we can either disable the service with `sudo systemctl disable cloud-init` or safely uninstall the package with `sudo apt purge cloud-init`.

## Glossary

* kvm: suport hardware de virtualització
* qemu: virtualització que pot fer servir kvm. Pot gestionar vm, però a cada crida cal dir-li amb quanta ram, cpus, etc. un pal.
* libvirt gestiona qemu, xen, lxc... i permet desar configs de vm i gestionar-les de forma similar
* `virsh` consola de libvirt. cheatsheet abaix. CLI que farem servir
* `virt-admin` consola que no sé què fa.
* `virt-manager` GUI tipus vbox molt amable. Conceptes: connection és amb el dimoni qemu+kvm o qemu sense kvm, o altres motors. Imatge és equivalent a vbox.