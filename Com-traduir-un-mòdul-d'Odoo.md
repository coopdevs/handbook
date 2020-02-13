## Necessitat

Alguns mòduls no tenen traducció al català o ni al castellà, per a pantalles que faran servir els nostres usuaris. Volem que ho puguin fer en la seva llengua amb la mínima fricció. També pot ser que existeixin traduccions però que alguns termes siguin molt confusos. El procediment és el mateix.

## El sistema d'Odoo

Odoo té molt en compte les traduccions i hi ofereix flexibilitat. Per exemple, permet:

* Editar traduccions locals des de la web mateixa.
* Importar traduccions des d'arxius externs, en formats estàndard.
* Generar arxius de format estàndard amb les traduccions actives localment.

De tota manera, per traduir un mòdul sencer, la interfície web pot ser confusa, perquè no permet filtrar els termes segons el mòdul que els insereix, només pels models, contingut... i la quantitat total de termes en tot Odoo és enorme.

Per això proposem un altre mètode

## Resum del procediment per traduir

1. Exporta les traduccions actuals
2. Desa les traduccions actuals al git
3. Tradueix amb poedit
4. Desa els canvis
5. Importa a Odoo, comprova els canvis
6. Merge request al mòdul

### Exporta les traduccions actuals

En una instància d'Odoo amb el mòdul que volem traduir instaŀlat, ves a Configuració > Traduccions > Exporta, després tria la llengua per la qual que vulguis traduir, format "PO" i selecciona escriu els noms del mòdul que vulguis traduir. Després d'uns instants, clica a l'enllaç semblant a "Aquí està l'arxiu de traducció exportat: [module_name.pot](#)". La `t` de `.pot` vol dir template.

Odoo Permet triar més d'un mòdul, i això ho combina tot en un arxiu. També hi ha entrades que no es defineixen en un mòdul, sinó en un altre, però es fan servir des del mòdul. Aquestes també apareixeran en el fitxer exportat. Cal tenir en compte que si canviem un d'aquests termes, és possible que canviïn per altres mòduls que estiguin fent servir el mateix terme.

## Desa les traduccions actuals al git

Com que el poedit fa alguns canvis com trencar les línies llargues, i més endavant voldrem editar aquest arxiu amb poedit, primer obre aquest arxiu amb poedit, i desa'l amb el nom de l'arxiu final (com `ca_ES.po`). Si fas un diff amb el descarregat, hauries de veure canvis estètics.

Has de saber on voldràs guardar les traduccions. Al repositori oficial (al qual tens accés)? A un fork? Sigui com sigui, obre una nova branca amb un nom com `i18n-ca_es-pos-sale`, i afegeix-hi l'arxiu que has exportat. Això farà que puguem saber amb git quines són les traduccions que has fet tu, i quines ja estaven a Odoo, o quines has deixat buides, etc.

## Tradueix amb poedit

Poedit té una versió lliure molt completa disponible als repositoris d'almenys Debian i Ubuntu. Un cop instaŀlada l'aplicació, obre-la, des d'ella obre l'arxiu descarregat des d'odoo (com `module_name.pot`) i tradueix les cadenes que entenguis necessàries. La llista pot ser molt llarga!

Dreceres:
* ctrl+enter per passar a la següent entrada
* ctrl+b per copiar el text original a la traducció (útil si només canvia una part d'un template xml, per exemple)

## Comprova els canvis

Des del mateix lloc on havies exportat l'arxiu, pots importar-ne de nous amb el format de gettext (po). Fes-ho, comprova per la UI els canvis, i tradueix el que t'hagis deixat. Quan estiguis satisfet/a amb la traducció, fes commit a la mateixa branca que l'arxiu exportat abans, matxacant-lo i obre una merge request al mòdul.