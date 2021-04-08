En algunos proyectos, al modificar la configuración del firewall nos hemos quedado fuera de la máquina sin acceso por SSH.

Esto es debido a que el rol que usamos para configurar el firewall empieza haciendo un _flush_ de las reglas existentes:

https://github.com/geerlingguy/ansible-role-firewall/blob/master/tasks/main.yml#L5


La consola de Hetzner se conecta utilizando el [`qemu-ga` QEMU Guest Agent](https://wiki.qemu.org/Features/GuestAgent), por lo que debemos tener el servicio activo y configurado tal cual nos lo proporciona Hetzner.

Si nos encontramos en esta situación debemos seguir los siguientes pasos:

1. Acceder al panel de administración de Hetzner.
2. Localizar el servidor que queremos recuperar y abrir una consola utilizando el botón disponible en el panel.
3. Regenerar la contraseña de `root` desde el panel accediendo a _Rescue > Root Password_ y haciendo clic en _Reset Root Password_.
4. Se nos muestra un password que utilizaremos para acceder por la consola que hemos abierto en el paso 2.
5. Una vez dentro del servidor regeneramos la regla del firewall que nos permite acceder con SSH:
```
# iptables -A INPUT -i eth0 -p tcp --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT
```
6. Para asegurar que todo está correcto, podemos listar todas las reglas del firewall:
```
# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:ssh state NEW,ESTABLISHED

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```
7. Una vez asegurado que tenemos el firewall configurado para aceptar peticiones al puerto de SSH, podemos acceder con normalidad.

