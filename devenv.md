# devenv
## ¿Qué es y cómo funciona?
Es un script bash para crear y manejar entornos de desarrollo utilizando contenedores linux LXC privilegiados.

devenv se compone de tres ficheros:
* Makefile: es el archivo que define las tareas que puede ejecutar el comando `make`. En este caso `install`, que copia el script create-container.sh en /usr/sbin/devenv y el fichero config que importa create-container.sh en /etc/devenv; `uninstall`, que borra /usr/sbin/devenv y /etc/devenv; y `all` que llama a `install`.

* config: fichero de configuración para el comando devenv.

* create-container: el script que crea el contenedor importando el fichero de configuración anterior. Comprueba que se han creado los ficheros y variables necesarios para la creación del contenedor durante su ejecución y si no es así da instrucciones a la usuaria para solucionar el error. También configura las claves SSH del contenedor, instala `sudo` si es necesario, crea un usuario para el proyecto e instala el intérprete de python y el SSH server.


