## ¿Qué es Ansible?
[Ansible](https://github.com/ansible/ansible) es una herramienta de automatización con licencia GPL-3.0 que permite trabajar con aprovisionamiento de software, gestión de configuraciones y despliegue de aplicaciones de una forma sencilla.

## ¿Por qué es útil?
Porque nos permite montar máquinas con software concreto y configuraciones concretas de forma automática y sencilla. Ahorrándonos así tiempo y complejidad en las instalaciones.

## ¿Cómo funciona?
Para Ansible existen dos tipos de máquinas, el *control node* y el *managed node* o *host*. 
El *control node* es la máquina desde la que comienza la orquestación, desde la que lanzamos las órdenes de Ansible y donde éste está instalado. El *managed node* es la máquina orquestada, es decir, aquella a la que accedemos por SSH y dónde se ejecutan las órdenes de Ansible, debe tener instalado Python.

### Modules
Ansible está organizado en módulos que son las unidades de código que ejecuta. Se puede invocar un solo módulo con `ansible -m <module-name>` o varios diferentes en un *playbook*. Existen módulos predefinidos que Ansible facilita, pero también se pueden crear propios.

### Inventory
Es el fichero donde se define la lista de *managed nodes*, grupos de éstos y sus variables, como el puerto ssh. También se le llama *hostfile*.

### Playbook
El *playbook* es la lista de *tasks* escrita en [YAML](https://es.wikipedia.org/wiki/YAML), ordenada y guardada para que se puedan ejecutar en un de forma secuencial repetidamente. Puede contener también variables.

### Roles
Los roles son grupos de ficheros -como *templates* y variables- y *tasks* basados en una estructura de ficheros concreta. Existen roles de Ansible y roles que aporta la comunidad en [galaxy](https://galaxy.ansible.com/).

### Tasks
Son las unidades de acción en Ansible.
Se definen en `tasks/main.yml` dentro de la carpeta de cada rol.

## ¿Para qué lo usamos en Coopdevs?
En Coopdevs usamos Ansible en nuestras instalaciones para asegurarnos de que los diferentes servidores que gestionamos están en el estado que queremos y, de esta manera, poder mantener nuestro software de manera más eficiente y sencilla; acortar el tiempo de *deploy* de nuevas máquinas, y de recuperación en caso de fallos graves; y facilitar la creación de entornos de trabajo paralelos: desarrollo, pruebas y producción.