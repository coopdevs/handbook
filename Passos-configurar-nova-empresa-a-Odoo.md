Un cop creat la nova instància o base de dades de Odoo hem de realitzar les següents accions per tal de configurar la nova empresa. 

## Canviar password usuari admin
Per defecte es crear un usuari admin/admin, entrem al usuari 
Si volem tenir accès guardar a Bitwarden (collection: Odoo)

![Canviar pass](img/odoo-canviar-pass.gif)

## Afegir llengües
`Menú Principal > Configuració > Traduccions`
Per defecte ve instal·lat l'anglès. Afegim habitualment castellà (es_ES) i català (ca_ES)
Cal fer clic al "botó d'actualitzar termes" 🔄

## Canviar llengüa admin al català 
Si es vol, tota la docu que fem es pel odoo en català

##  Editar empresa, nom, logo, altres dades...

odoo@processos.org

## Configuració general -> check "Restablir contrasenya"

##  Configurar SMTP: Configuració/Servidors de correu sortint

* Mandrillapp
* Host smtp.mandrillapp.com
* Seguretat TLS
* Port 587
* SMTP Username Coopdevs
* SMTP Password any valid API key (Entrar a Maigun)

##  Crear un usuari principal al Client 
A `Menú Principal > Configuració > Users & Companies > Usuaris`
Amb accès top a tots els mòduls i `Administració` amb **poder assignar permisos**

Enviar invitació al usuari per mail 