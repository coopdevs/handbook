## Goal
To make it easy to create virtual machines in qemu almost as easy as [devenv project](https://github.com/coopdevs/devenv/) does with linux containers.

## References

* [Debian wiki on KVM, qemu and libvirt](https://wiki.debian.org/KVM)
* [Libvirt wiki](https://wiki.libvirt.org/page/Main_Page)

Com que l'entorn instaŀla nginx i docker al host, ho fico dins una màquina virtual amb qemu.

## tuto per a un host debian 9:

### instaŀlem paquets

`# apt install qemu-kvm libvirt-clients libvirt-daemon-system`

### descarregar images en format cow2:

* ubuntu https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.img
* debian https://cdimage.debian.org/cdimage/openstack/current/

### assegurem permisos

#### grups

```
# adduser <youruser> libvirt
# adduser <youruser> libvirt-qemu
```

#### config

```
#  /etc/libvirt/qemu.conf
# caldria provar amb ~/.local/etc/libvirt/qemu.conf
user = "<youruser>"
group = "libvirt-qemu"
```

#### kvm device

`sudo chown :kvm /dev/kvm`

#### images

```
permisos 660 (perquè el grup pugui també escriure)
user propietari: youruser
grup propietari: libvirt-qemu

.local/var/lib/libvirt/images/
```
Nota: per defecte, molta docu assumeix la ubicació com /var/lib/libvirt/images/ , però això és propietat de root, i nosaltres volem executar les vm com a usuari normal.

### Crea una imatge de configuració per canviar les credencials

```
# apt install cloud-image-utils

# echo '#cloud-config
password: contrasenyainseguraohno
chpasswd: { expire: False }
ssh_pwauth: True
' > cloud-config.conf

# cloud-localds cloud-init.img cloud-init.conf
cp cloud-init.img ~/.local/var/lib/libvirt/images/
```
Aquesta imatge la inserirem com un CD a la màquina virtual, i això farà que activi l'autenticació per contrasenya després d'un reboot.

### Crea la imatge amb la GUI (corba d'aprn. més ràpida)

1. connexió amb kvm està creada
2. crea nova imatge
3. tria "una imatge existent" i selecciona la teva img cow2
4. tria xarxa nat
5. siguiente siguiente fins que es queixi: "custom cpu not suported" → vas a editar les opcions abans d'instaŀlar, cpu, "copia la cpu del host" → aplica → instaŀla

### Arrenca la màquina per consola

```
# virsh
> connect qemu:///system
> list
> attach-disk debian9 /path/to/cloud-init.img sdb
> reboot debian9
> console debian9
```
Aquest últim pas ens hauria de donar un login on autenticar-nos amb `debian` i `contrasenyainseguraohno`. Fem ping a coopdevs.org per comprovar que tenim sortida ip i dns funcionant :)

### Neteja posterior

El paquet cloud-init està instaŀlat a la màquina host. El primer cop està bé que hi sigui, perquè ens permet canviar configuració, però després molestarà molt perquè ralenteix una barbaritat l'arrencada. Systemd calcula quant ha trigat cada servei en cadena:

```
ferran@debian:~$ sudo systemd-analyze blame
sudo: unable to resolve host debian.localdomain
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
per tant, un cop hem comprovat que ens podem loguejar amb les credencials que volem, deshabilitem el servei fent `sudo systemctl disable cloud-init`

### glossari

* kvm: suport hardware de virtualització
* qemu: virtualització que pot fer servir kvm. Pot gestionar vm, però a cada crida cal dir-li amb quanta ram, cpus, etc. un pal.
* libvirt gestiona qemu, xen, lxc... i permet desar configs de vm i gestionar-les de forma similar
* `virsh` consola de libvirt. cheatsheet abaix. CLI que farem servir
* `virt-admin` consola que no sé què fa.
* `virt-manager` GUI tipus vbox molt amable. Conceptes: connection és amb el dimoni qemu+kvm o qemu sense kvm, o altres motors. Imatge és equivalent a vbox.