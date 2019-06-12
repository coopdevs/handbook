## Creació d'un nou contracte

Els passos per crear el contracte son:

1.  Obrim la pantalla de Contracts: `Facturació > Vendes > Contracts`
2.  Fem clic al botó Crear.
3.  Al nom indiquem el nom del contracte
4.  Seleccionem el client al qual volem facturar amb recurrència.
5.  Escollim si es pagarà abans del període o després (pre/post Paid)
6.  Escollim cada quant temps es farà la factura (diari/setmanal/mensual/...) i la data d'inici del contracte. 
7.  Podem escollir ["Contract Template"](#crear-un-contract-template) en cas que en tinguem. 
8.  Afegim, línies de factura.
9.  Desem


![Contractes](img/contracts/contracts2.png)


## Creació de factures partint d'un contracte

Les factures es crearan la data d'inici del contracte. 

Un administrador veu el botó "Create Invoices" que simularà el moment en què s'hauria de crear la següent factura. 

![Contractes](img/contracts/contracts3.png)

## Crear un Contract Template

Obrim la pantalla de Contract Templates: `Facturació > Configuració > Contract Templates` fem clic al botó Crear

![Contractes](img/contracts/contracts1.png)

4.  Omplim el formulari
  * Afegir les línies de factura que sabem que es cobraran periòdicament amb regularitat.
  * A la línea de factura podem marcar auto-price? per tal de que els preus s'actualitzin directament de un llistat de preus, o bé indicar el preu fixe a mà. 
  * Escollir "Pricelist" perquè agafi el preu del llistat de preus
  * Pre-paid/Post-paid si volem el pagament abans o després del període indicat

8.  Desar


## Llista de Preus

### Canviar el llistat de preus

Els preus dels serveis facturats mensualment es marquen a unes llistes anomenades 'Llistes de preus'. Aquestes llistes tenen un període de temps en què són valides (normalment un trimestre o quadrimestre).

Fent servir aquest llistat de preus i una quantitat que pot ser fixa o variant a les factures, calcularem automàticament el preu total de la factura.

Així doncs, haurem d'anar renovant aquests llistats de preu cada cert període de temps així com actualitzar als contractes vius quin és el llistat de preus que volem fer servir per a les properes factures.

### Creació de Llistats de preus

Per crear una nova llista de preus accedim a `Vendes > Catàleg > Llistes de Preus`.

Un cop en aquesta pantalla podem crear un nou llistat de preus fent clic a 'Crear' indicant al formulari el nom del nou llistat de preus, per exemple: 'Preus serveis 1r trimestre 2018'.

### Actualitzar preus en productes

Un cop creat aquest llistat de preus, accedim al llistat de productes (o serveis en el nostre cas) accedint a Vendes → Catàleg → Productes. Obrim un a un tots els productes que volem actualitzar el llistat de preus y a la pestanya de Vendes, afegim una nova tarifa indicant el llistat de preus y quin és el preu per aquesta tarifa d'aquest producte en concret.

### Actualitzar llistat de preus en contractes

Un cop tenim el llistat de preus el producte amb el preu actualitzat, podem actualitzar el Llistat de preus que fem servir a cada contracte actiu que tenim. Per fer això accedim al llistat de contractes actius accedint a Facturació → Vendes → Contractes. Un cop al llistat, seleccionem tots els contractes que volem actualitzar amb el checkbox de l'esquerra i executem l'acció: Acció → Canvi de llistat de preus.

Això obre un popup on podem indicar quin llistat de preus volem fer servir

## Seqüències de factura per cada tipologia de contracte 

És possible que necessitem diferents tipus de seqüències de factura per generar el nº/codi de factura per cada tipologia de contracte. Per exemple per un tipus concret de contracte (TCC) podríem voler que les factures fossin amb la seqüència TCC0001, TCC0002, ...

Per a fer-ho haurem de definir el diari (journal) del contracte amb la seqüència concreta que desitgem.

1. Anem a `Configuració > Diaris`
2. Entrem en el diari de vendes i el dupliquem
3. Anomenem el diari, el codi i el primer número de seqüència
4. A "Seqüència de la factura" creem una nova seqüència
5. Indiquem prefix o sufix, grandària del nombre (mida de seqüència, per obtenir 0001 la "mida de seqüència" seria 4)
6. Finalment anem a `Vendes > Contracts` escollim el contracte al qual volem aplicar el diari que hem creat


![Contractes](img/contracts/contracts4.gif)

Un cop s'executi la creació de factura del contracte ja ens el crearà amb la nova seqüència. 

![Contractes](img/contracts/contracts5.png)

