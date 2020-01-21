## ¿Qué es pyenv?
Una herramienta con licencia MIT enfocada a la administración de múltiples versiones de Python en sistemas Linux. 
El proyecto fue *forkeado* de [rbenv](https://github.com/rbenv/rbenv) y [ruby-build](https://github.com/rbenv/ruby-build).

## ¿Para qué sirve?
* Te permite cambiar la version global de Python por usuario.
* Proporciona soporte para las versiones de Python por proyecto.
* Permite sobrescribir la versión de Python con una variable de entorno.
* Busca comandos de múltiples versiones de Python a la vez. Esto puede ser útil para testear a través de las versiones de Python con tox.

## ¿Por qué es útil?
Porque nos permite homogeneizar las versiones de Python en distintos entornos de forma sencilla y así evitar problemas de dependencias. Así como gestionar diferentes versiones en nuestra máquina local para, por ejemplo, *testear* proyectos que soportan varias versiones de Python. 

## ¿Cómo funciona?
Intercepta los comandos de Python utilizando shims inyectados en tu `PATH`, determina qué versión de Python ha sido especificada en tu aplicación y pasa tus comandos a la instalación correcta de Python. 

## ¿Para qué lo usamos en Coopdevs?
Para homogeneizar las versiones de Python en todas sus instalaciones y evitar problemas de dependencias. 

## Referencias externas para su uso
* Pyenv, Environment variables: https://github.com/coopdevs/timeoverflow/wiki/Build-development-environment-with-LXC-for-GNU-Linux
* Pyenv, Command Reference: https://github.com/pyenv/pyenv/blob/master/COMMANDS.md
* JONES, Logan, Managing Multiple Python Versions With pyenv: https://realpython.com/intro-to-pyenv/