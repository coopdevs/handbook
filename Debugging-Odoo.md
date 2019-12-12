Cosas a tener en cuenta al debugear una instancia de Odoo:

* Tener la configuraci'on de logs desactivada para que el output salga por stdout
* No utilizar workers ni multithreading
* Utilizar el argumento `--dev` con alguna de las opciones que podemos encontrar en https://www.odoo.com/documentation/11.0/reference/cmdline.html
* Utilizar `ipdb` para parar la ejecución:
  1. Debemos parar el proceso de `odoo`
  2. Revisar la configuracion para asegurar que no levantamos workers y que el output no va a un fichero de logs
  3. Añadir el brackpoint donde queramos: `import ipdb; ipdb.set_trace()`
  4. Levantar la aplicación manualmente (no systemd)