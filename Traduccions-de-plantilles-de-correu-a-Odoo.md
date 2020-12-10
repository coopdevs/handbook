Des d'un compte d'admin, anem a Configuració > Tècnic > Correu electrònic / Plantilles

Des d'allà podem escriure el filtre segons a què apliquen les plantilles. Per exemple, a correus relacionats amb "Factura".

Com **NO** traduir les plantilles 
* Clicant Edita, i fent servir l'editor gràfic. Depèn dels canvis fets, pot canviar l'HTML d'una manera que des de la mateix editor no es pot desfer. A més, afegeix atributs style="" llargs i redundants.
* Picar el botó "Previsualitzar" amb canvis no desats, sobreescriu i desa automàticament amb els canvis.

Com **SÍ** traduir les plantilles
1. Cliquem Edita i anem a la [icona de traducció](https://fontawesome.com/v4.7.0/icon/language)
2. Cliquem a la traducció per la llengua que vulguem.
3. Abans d'editar-la, per seguretat copiem la traducció original a un arxiu propi.

---

## La feblesa de les plantilles

Els templates són jinja, i fan servir els "line statements". Això vol dir que cada expressió jinja [ha de començar la línia](https://jinja.palletsprojects.com/en/2.10.x/templates/#line-statements) seguida o no d'espais i tabuladors. 

Això a nivell d'arxiu de text. A nivell de HTML, els `\n` i espais fora de les etiquetes no tenen efecte. L'editor gràfic gestiona HTML, i per tant, pot canviar o no els '\n' sense mostrar-ho visualment.