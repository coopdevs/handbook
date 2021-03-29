El código que muestra la jerarquía de carga de los .po está en odoo/addons/base/models/ir_translation.py

![image](https://user-images.githubusercontent.com/56868560/112803134-42b92200-9073-11eb-9651-c7b2e99ba596.png)

En comentarios podemos ver las diferentes jerarquías:
* _Step 1: for sub-languages, load base language first (e.g. es_CL.po is loaded over es.po)_
* _i18n_extra folder is for additional translations handle manually (eg: for l10n_be)_
* _Step 2: then load the main translation file, possibly overriding the terms coming from the base language_


En resumen:
- los strings/cadenas no traducidos (msgstr "") dentro del .po se ignoran y se buscan traducciones en el siguiente nivel de .po 
- si por ejemplo tenemos "ca_ES" como idioma activo en nuestro odoo, el orden de mayor a menor prioridad de búsqueda de cadenas es:
i18n/ca_ES.po
i18n_extra/ca.po
i18n/ca.po

Ejemplo: si ca_ES.po contiene una cadena vacía, se coge la cadena de ca.po : 

![Selección_208](https://user-images.githubusercontent.com/56868560/112803640-d4c12a80-9073-11eb-8792-6700e9c470fc.jpg)
![Selección_209](https://user-images.githubusercontent.com/56868560/112803643-d559c100-9073-11eb-8d3d-719c21acfce9.jpg)

pero si ca_ES.po contiene la misma cadena también traducida, entonces esta tendrá prioridad frente a la de ca.po : 

![Selección_211](https://user-images.githubusercontent.com/56868560/112803801-f8847080-9073-11eb-8ae4-44e5a9c5acec.jpg) 
![Selección_210](https://user-images.githubusercontent.com/56868560/112803800-f8847080-9073-11eb-9353-6577738cbc70.jpg)


Hay que apuntar que a la hora de recargar traducciones, se deberá marcar el checkbox de sobrescribir las cadenas/condiciones existentes:

![Selección_213](https://user-images.githubusercontent.com/56868560/112803920-17830280-9074-11eb-807d-ebec37adf821.jpg)

Esta forma de cargar las traducciones de Odoo es útil para no tener que rellenar el 100% de las cadenas dentro de un .po que contenga solo variaciones para alguna traducción genérica, ejemplo, aquí vemos algunas modificaciones para la variante mexicana(es_MX.po) de odoo sobre la genérica española "es.po" :  

![Selección_215](https://user-images.githubusercontent.com/56868560/112803930-181b9900-9074-11eb-8cf6-8a43504cfa85.jpg)
