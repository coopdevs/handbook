## Tenim creat el client?

![Client](https://github.com/coopdevs/handbook/wiki/img/client.png)


Si el client no surt al llistat clients, hem de crear-lo per fer-ho serà indispensable introduir: 

* Imatge
* Nom de la empresa (tal i com apareix legalment, en cas de tenir un nom més facilment recordable tenim el camp "Nom Comercial"
* NIF
* Adreça
* Idioma del client (serà l'idioma en que li sortirà factura impresa)

Opcionalment podem omplir altres dades de contacte del client. 

## Emetre una nova factura

![Factura](https://github.com/coopdevs/handbook/wiki/img/factura.png)

1. Botó "crea"
2. Seleccionem el client en qüestió. 
3. Introduïm Data factura i Data de Venciment, habitualment no cal omplir aquestes dades i per defecte la data es la actual.
4. Mètode de pagament: Transferència
5. A la pestanya de sota "Altra informació" seleccionar compte bancari pel fer el pagament. 

Ja tenim tota la informació necessària relativa a la factura, ara sols falta introduir els conceptes. Són aquelles coses que volem facturar: "Hores de programador", "SysAdmin",  "Optimització de processos empresarials", ... 

1. Clic a "Afegeix un element" (en podem afegir tants com necessitem)
2. Seleccionem un producte, al llistat apareixen tots els productes que oferim com Coopdevs
3. Si es necessari podem afegir una descripció per exemple "hores per al projecte X setembre 2018"
4. Podem modificar la quantitat(per exemple les hores dedicades) i el preu (per exemple en el cas que ho haguem vengut a un preu diferent a les tarifes)
5. Ens assegurem de que es sumen els impostos necessaris i ja podem desar la factura, quedaria en estat "Esborrany" si no era una prova podem fer clic a "Validar" per avançar al estat "Obert"

### Estats de la factura
* Esborrany: sols en cas de que no estiguem segurs, encara no té un número de factura i és fàcil d'esborrar i crear una nova. 
* Obert: Dins de la factura podem fer clic a validar i ens canviarà a l'estat Obert, ara la factura ja té una numeració i no s'hauria de canviar, tot i que amb els permisos necessàris es pot tornar a esborrany i modificar.
* Pagat: Un cop s'ha rebut el pagament per aquesta factura i s'ha conciliat a la comptabilitat