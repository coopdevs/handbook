## Tenim creat el proveïdor?
Igual que en el cas de venda, ens hem d'assegurar que existeix el proveïdor, en cas contrari introduir les dades com ho faríem per client.  

Aspectes diferèncials respecte les factures de clients: 
 
 * A la factura hem d'introduir "Referència de Proveïdor" que és el nº de factura que indica el proveïdor.
 * Ens hem d'assegurar que l'IVA quadra i si no el modifiquem(de tant en tant passa que el proveïdor arrodoneix el IVA diferent que Odoo i tenim que retocar-lo manualment a Odoo perqué coincideixi amb la factura del proveïdor)
 * Sempre adjuntarem una foto/pdf de la factura original a la factura de Odoo. Això ho podràs fer sols despres de desar la factura, apareixerà el menú. 

![Adjuntar Factura](https://github.com/coopdevs/handbook/wiki/img/adjuntar_factura.png)

## Factures fora de trimestre
Si ens adonem que tenim una factura de proveïdor pendent d'introduir a Odoo i la data de factura no forma part del trimestre actual podem introduir-la amb normalitat tenint en compte: 
* Introduir-la i indicar al camp 'data de factura': dia 1 del primer mes del trimestre actual. (1 de gener, 1 de abril, 1 de julio, 1 octubre) 
* Podem indicar en el camp final "Notes addicionals" que es tracta d'una factura introduïda amb retràs i els motius.