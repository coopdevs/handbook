## Tenim creat el proveïdor?
Igual que en el cas de venda, ens hem d'assegurar que existeix el proveïdor, en cas contrari [introduir les dades](https://github.com/coopdevs/handbook/wiki/Emetre-una-factura-de-venda#tenim-creat-el-client) com ho faríem per client.  

## Elements a introduir 
 * "Referència de Proveïdor": és el nº de factura que indica el proveïdor
 * "Data de factura": data de emisio' de la factura
 * Els productes amb l'IVA que pertoqui

## Adjuntar factura original
Sempre adjuntarem una foto/pdf de la factura original a la factura de Odoo. Això ho podràs fer sols despres de desar la factura, apareixerà el menú.

![Adjuntar Factura](https://github.com/coopdevs/handbook/wiki/img/adjuntar_factura.png)

## Abans de validar
 * Ens hem d'assegurar que l'IVA quadra. Si el proveïdor arrodoneix el IVA de manera diferent que Odoo, tenim que retocar-lo manualment a Odoo perqué coincideixi amb la factura del proveïdor.

## Factures de empreses fora de Espanya
La regla general es que si tant el proveïdor com el client estan registrats al Registre d'IVA intracomunitari l'import de l'IVA es 0.

:warning: Coopdevs Treball SCCL **SI** esta' al Registre d'IVA intracomunitari

:warning: Coopdevs Associacio **NO** esta' al Registre d'IVA intracomunitari

Al cas de Coopdevs Treball SCCL els codis d'IVA que es poden utilitzar per productes/serveis de proveïdors que estan al registre son:
* `IVA 21% Adquisición de servicios intracomunitarios` (en cas de serveis)
* `IVA 21% Adquisición Intracomunitaria. Bienes corrientes` (en cas de productes)

## Factures fora de trimestre
Si ens adonem que tenim una factura de proveïdor pendent d'introduir a Odoo i la data de factura no forma part del trimestre actual podem introduir-la amb normalitat tenint en compte: 
* Introduir-la i indicar al camp 'data de factura': dia 1 del primer mes del trimestre actual. (1 de gener, 1 de abril, 1 de julio, 1 octubre) 
* Podem indicar en el camp final "Notes addicionals" que es tracta d'una factura introduïda amb retràs i els motius.