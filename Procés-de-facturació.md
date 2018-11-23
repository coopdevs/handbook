Entra al Odoo de Coopdevs

:warning: _Assegurat a quina organització estás Associació o Cooperativa_ :warning:

![Selector de Empresa](https://github.com/coopdevs/handbook/wiki/img/selector_company.png)

:warning: _Una factura ha de portar el NIF del emissor i el NIF del receptor, si tens només un ticket on no hi surt el NIF de Coopdevs no pots introduir-lo com factura i ho hauràs de fer com una despesa "Expense"_ :warning:


## Mòdul a Facturació (Invoicing) 

![Facturació](https://github.com/coopdevs/handbook/wiki/img/facturacio1.gif)

En primer lloc hem d'assegurar-nos quin tipus de factura volem introduir. Si es una factura a ingressar es una factura de venda i en cas de ser una despesa es una factura de proveïdor.

Tant si introduïm una factura de **Venda** com una factura de **Compra** la estructura i el procediment és anàleg. 

# Factura de Venda
## Tenim creat el cient?

![Client](https://github.com/coopdevs/handbook/wiki/img/client.png)


Si el client no surt al llistat clients, hem de crear-lo per fer-ho serà indispensable introduir: 

* Imatge
* Nom de la empresa (tal i com apareix legalment, en cas de tenir un nom més facilment recordable tenim el camp "Nom Comercial"
* NIF
* Adreça
* Idioma del client (serà l'idioma en que li sortirà factura impresa)

Opcionalment podem omplir altres dades de contacte del client. 

## Nova factura de client

![Factura](https://github.com/coopdevs/handbook/wiki/img/factura.png)

1. Botó "crea"
2. Seleccionem el client en qüestió. 
3. Introduïm Data factura i Data de Venciment, habitualment no cal omplir aquestes dades i per defecte la data es la actual.
4. Mètode de pagament: Transferència
5. A la pestanya de sota "Altra informació" seleccionar compte bancari pel fer el pagament. 

Ja tenim tota la informació necessària relativa a la factura, ara sols falta introduir els conceptes, els conceptes son aquelles coses que volem facturar: "Hores de programador", "SysAdmin",  "Optimització de processos empresarials", ... 

1. Clic a "Afegeix un element" (podem afegir tants com necessitem)
2. Seleccionem un producte, al llistat apareixen tots els productes que oferim com Coopdevs
3. Si es necessari podem afegir una descripció per exemple "hores per al projecte X setembre 2018"
4. Podem modificar la quantitat(per exemple les hores dedicades) i el preu (per exemple en el cas que ho haguem vengut a un preu diferent a les tarifes)
5. Ens assegurem de que es sumen els impostos necessaris i ja podem desar la factura, quedaria en estat "Esborrany" si no era una prova podem fer clic a "Validar" per avançar al estat "Obert"

### Estats de la factura
* Esborrany: sols en cas de que no estiguem segurs, encara no té un número de factura i és fàcil d'esborrar i crear una nova. 
* Obert: Dins de la factura podem fer clic a validar i ens canviarà a l'estat Obert, ara la factura ja té una numeració i no s'hauria de canviar, tot i que amb els permisos necessàris es pot tornar a esborrany i modificar.
* Pagat: Un cop s'ha rebut el pagament per aquesta factura i s'ha conciliat a la comptabilitat

# Factura de Compra
## Tenim creat el proveïdor?
Igual que en el cas de venda, ens hem d'assegurar que existeix el proveïdor, en cas contrari introduir les dades com ho faríem per client.  

Aspectes diferèncials respecte les factures de clients: 
 
 * A la factura hem d'introduir "Referència de Proveïdor" que és el nº de factura que indica el proveïdor.
 * Ens hem d'assegurar que l'IVA quadra i si no modificar-lo (de tant en tant passa que el proveïdor arrodoneix el IVA diferent que Odoo i tenim que retocar-lo manualment a Odoo perqué coincideixi amb la factura del proveïdor)
 * Sempre adjuntarem una foto/pdf de la factura original a la factura de Odoo. Això ho podràs fer sols despres de desar la factura, apareixerà el menú. 

![Adjuntar Factura](https://github.com/coopdevs/handbook/wiki/img/adjuntar_factura.png)
