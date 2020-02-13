## Necessitat

Alguns mòduls no tenen traducció al català o ni al castellà, per a pantalles que faran servir els nostres usuaris. Volem que ho puguin fer en la seva llengua amb la mínima fricció. També pot ser que existeixin traduccions però que alguns termes siguin molt confusos. El procediment és el mateix.

## El sistema d'Odoo

Odoo té molt en compte les traduccions i hi ofereix flexibilitat. Per exemple, permet:

* Editar traduccions locals des de la web mateixa.
* Importar traduccions des d'arxius externs, en formats estàndard.
* Generar arxius de format estàndard amb les traduccions actives localment.

De tota manera, per traduir un mòdul sencer, la interfície web pot ser confusa, perquè no permet filtrar els termes segons el mòdul que els insereix, només pels models, contingut... i la quantitat total de termes en tot Odoo és enorme.

Per això proposem un altre mètode

## Procediment per traduir

### Resum

1. Exporta les traduccions actuals
2. Desa les traduccions actuals al git
3. Tradueix amb poedit
4. Desa els canvis
5. Importa a Odoo, comprova els canvis
6. Merge request al mòdul

### Exporta les traduccions actuals

En una instància d'Odoo amb el mòdul que volem traduir instaŀlat, ves a Configuració > Traduccions > Exporta, després tria la llengua per la qual que vulguis traduir, format "PO" i selecciona escriu els noms del mòdul que vulguis traduir. 

Odoo Permet triar més d'un mòdul, i això ho combina tot en un arxiu. També hi ha entrades que no es defineixen en un mòdul, sinó en un altre, però es fan servir des del mòdul. Aquestes també apareixeran en el fitxer exportat. Cal tenir en compte que si canviem un d'aquests termes, és possible que canviïn per altres mòduls que estiguin fent servir el mateix terme.

