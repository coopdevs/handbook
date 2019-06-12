## Pagament de factures de proveïdors amb SEPA

Abans de començar ens hem de assegurar que els proveïdors que desitgem pagar per aquest mètode tinguin un compte Bancari definit i el Mode de Pagament "SEPA Credit Transfer to suppliers". Podem editar aquests camps a `Facturació > Compres > Proveïdors` així totes les factures que rebem d'aquests proveïdors automàticament les podrem afegir a Payment Orders tipus "SEPA Credit Transfer to suppliers"

![SEPA Credit Transfer to suppliers](img/SEPA/sepa-payments2.gif)

1. Creem un Payment Order

Accedint a `Facturació > Payments > Payment orders` podem generar un nou Payment Order on afegir les factures a pagar. Indicarem **Mode de pagament : SEPA Credit Transfer to suppliers**, **Bank Journal** el compte Bancari des d'on volem fer els pagaments i desarem el Payment Order

2. Afegir factures al Payment Order

Un cop tenim el Payment Order, podem accedir a `Facturació > Compres > Factures de Proveidors`, seleccionar les factures que volem afegir a la remesa i fer clic a `Acció > Add to Payment/Debit Order`.

3. Extreure el fitxer SEPA

Per finalitzar el procés necessitem extreure el fitxer SEPA que importarem a l'aplicatiu de la nostra entitat bancaria, dins del Payment Order en qüestió podrem fer clic al botó `Confirm Payments` i descarregar el fitxer. 

![SEPA Credit Transfer to suppliers](img/SEPA/sepa-payments3.gif)

(WIP: provar que fa el botó `File Successfully Uploaded` un cop estigui el fitxer enviat al banc) 


## Pagament de nòmines de treballadors amb SEPA

1. Els empleats poden tenir mètode de cobrament (SEPA) i compte bancari, al igual que una empresa proveïdora.  Si anem a `Empleats` seleccionem l'empleat i anem a la pestanya `Private information > Private Address` aquí podrem associar l'empleat amb un "Contacte" dins del qual haurà de tenir un Compte Bancari dins la pestanya `Vendes i Compres` i Supplier Payment Mode: SEPA a dins la pestanya `Facturació`

2. En els assentaments comptables d'una nòmina tindrem sempre la quantitat a pagar contra el compte **465000 Remuneraciones pendientes de pago** i en el mateix assentament la empresa relacionada, en aquest cas serà el treballador. En [aquest vídeo](https://www.youtube.com/watch?v=Ih-xASGIEh0) trobaràs la explicació de com tenir comptabilitzada correctament un nòmina.

3. Creem un nou "Payment order" `Facturació > Payments > Payment orders` 
  * Metode de pagament "SEPA"
  * Clic al botó `Create Payment Lines from Journal Items`
  * "Due Date" avui
  * Afegim els assentaments comptables fent  "Afegeix un element" > "Filtrar" > "Afegir un filtre personalitzat "per "compte" conté "465" trobarem els assentaments per afegir

## Retorn dels diners de despeses a treballadors amb SEPA

1. Els empleats poden tenir mètode de cobrament (SEPA) i compte bancari, al igual que una empresa proveïdora.  Si anem a `Empleats` seleccionem l'empleat i anem a la pestanya `Private information > Private Address` aquí podrem associar l'empleat amb un "Contacte" dins del qual haurà de tenir un Compte Bancari dins la pestanya `Vendes i Compres` i Supplier Payment Mode: SEPA a dins la pestanya `Facturació`

2. Les Despeses que volem retornar han de complir:
  * Payment By: Employee (to reimburse)
  * Empleat: Un empleat que compleixi amb el punt 1

Un cop la despesa hagi estat aprovada i es faci clic al botó "Assentar assentaments" es podrá crear el fitxer Sepa

3. Creem un nou "Payment order" `Facturació > Payments > Payment orders`
  * Metode de pagament "SEPA"
  * "Due Date" avui
  * fem clic al botó "Create Payment Lines from Journal Items"
  * Afegim assentaments "Afegeix un element" > "Agrupar per" "Afegeix un grup personalitzat" "Despeses"
