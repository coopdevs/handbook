A continuació es descriuen els passos per incorporar a un nou treballador a la dinàmica de treball. El que en anglès s'anomena com _Onboarding_. Sí, la nostra professió comporta un clar biax anglosaxó.

## Índex

[Compte d'email](#compte-demail)<br>
[Usuari i empleat a Odoo](#usuari-i-empleat-a-odoo)<br>
[Xat de treball](#xat-de-treball)<br>
[Accés al gestor de contrasenyes](#accés-al-gestor-de-contrasenyes)<br>
[Permisos per repositoris](#permisos-per-repositoris)<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Github](#github)<br>
&nbsp;&nbsp;&nbsp;&nbsp;[Gitlab](#gitlab)<br>
[Calendari](#calendari) <br>
[Drive/Nextcloud]() **PENDENT**

Demanar dades per publicar a la Web (potser crear pàgina "Procés d'onboarding: comunicació"

## Compte d'email

És important fer aquest pas el primer. D'aquesta manera totes les invitacions a serveis i aplicacions es podran fer usant aquest compte de correu separant així les qüestions laborals del compte de correu personal.

Accedeix des del [panell de CDmon](https://admin.cdmon.com/es/acceso) a la gestió de l'allotjament de mail clicant al _hosting mail_ corresponent a coopdevs.org. Des d'aquí clica a _Gestión de cuentas_ i _Crear cuenta_. Recorda seguir la convenció de nom.cognom@coopdevs.org i genera una contrasenya aleatòria inicial.

Un cop ho hagis fet, comunica-li a la persona i indica-li que és preferible que se l'enllaci al seu client de correu habitual. A més, ha de tenir present que rebrà invitacions al seguit de serveis que s'expliquen a continuació. Per tant, serà necessari que comprovi que pot accedir al nou compte de correu.

### Configuració amb altres clients d'email

CDmon té instruccions detallades de com [configurar](https://ticket.cdmon.com/es/support/solutions/articles/7000006292-c%C3%B3mo-configurar-el-correo-electr%C3%B3nico-de-cdmon-en-gmail) el compte a Gmail. Tot i així, CDmon ofereix un client web a [mail.coopdevs.org](http://mail.coopdevs.org).

Les dades específiques del servidor de correu són les següents

```
Servidor IMAP imap.coopdevs.org port: 143 / port SSL/TLS: 993
Servidor POP3 pop3.coopdevs.org port: 110 / port SSL/TLS: 995
Servidor SMTP smtp.coopdevs.org port i port SSL/TLS: 25, 578 o 587
```

## Usuari i empleat a Odoo

### Crear usuari a Odoo

Anem a Configuració > Users & Companies > Usuaris i creem un nou usuari. Omplim el nom, mail i Companyies permeses (poden ser varies)

#### Permisos d'accès 

Depenent de les aplicacions que tinguem instal·lades ens apareixeran un seguit d'opcions de permissos. Si seleccionem la opció buida l'usuari no tindrà accés a aquesta aplicació, les següents opcions per ordre ascendent aniran atorgant més i més permisos dins de cada aplicació. 

#### Preferències de l'usuari 

A la pestanya **Preferències** podrem seleccionar aspectes com l'idioma del usuari, zona horària o si volem que rebi les comunicacions d'Odoo per email o bé a través del mateix Odoo.  

### Crear Empleat a Odoo

Feu clic al submenú "empleats" de l'esquerra (hi ha els empleats existents) i després en el botó "Crea". Es mostra un formulari. Completa tota la informació que necessitis en les diferents pestanyes.

A la pestanya "Configuració RH", **vinculem l'empleat al usuari en qüestió**. 

### Afegir dies disponibles de vacances a Odoo

A Absències, assignar dies de vacances restants del any en curs

## Xat de treball

### Telegram

Fem servir Telegram i Zulip per a les comunicacions de la cooperativa. Telegram més aviat per una comunicació informal amb tota la cooperativa i Zulip per coordinar feina. 

**Telegram**

Per a afegir a la nova persona, des del client web, cal que la tinguis als contactes del telèfon prèviament. Un cop fet, entra al grup, accedeix a la seva informació clicant a la capçalera i a continuació a _Add member_. En aquest punt et permetrà cercar i seleccionar el contacte del teu telèfon. Un cop n'estiguis segur clica _Next_ i ja estarà fet.

**Zulip**
https://coopdevs.zulipchat.com > "Invite more users" afegim a les persones, normalment com a members i els donem acces als Streams que ja sabem que hi participaran 


### Mattermost

Alguns projectes ja fan servir Mattermost https://coopdevs.cloud.mattermost.com/, per convidar a nous membres cal anar al menú principal, a dalt a la esquerra -> "invite people" 

## Accés al gestor de contrasenyes

Fem servir BitWarden. Això és necessari per disposar de les credencials d'accés als serveis externs que usem a Coopdevs.

Primer de tot, navega a l'administració de l'organització Coopdevs clicant al nom al menú lateral anomenat _Organitzacions_ de la pàgina inicial del teu compte de Bitwarden. Seguidament desplaçat a la pestanya _Administra_. Des d'aquí pots convidar a la persona clicant a _Convida usuari_. 

Al modal que t'apereixerà, introdueix el correu electrònic de la persona, selecciona el tipus d'usuari _Usuari_ i marca _Aquest usuari pot accedir i modificar tots els elements._ com a control d'accés. Per ara, confiem plenament en els companys que contractem, cosa que simplifica la gestió.

Per acabar, clica a _Guardar_.

**Confirmar usuari**

Per poder finalitzar el procés, un cop la persona hagi acceptat la invitació caldrà que des del compte de Coopdevs de Bitwarden la confirmem.

Des de la mateixa secció d'administració del compte esmentada al punt anterior, clica a la pestanya Acceptat i a l'engranatge que apareix sobre el nou usuari. Si selecciones confirma, això obrirà un diàleg per confirmar definitivament l'acceptació de l'usuari. Fins que no facis aquest pas no hi podrà accedir.

**Donar accés a les col·leccions**

Les contrasenyes les agrupem per col·leccions. Perquè la nova usuària pugui accedir a les dades cal donar permisos a les col·leccions que es vulguin exposar. Per fer-ho anem a la secció de configuració de la nostra organització i accedim a l'espai d'_Administra_ i seguidament a _Col·leccions_. Ací veiem un llistat de totes les col·leccions i accedint a la configuració de la col·lecció amb el botó d'engranatge de la dreta, seleccionem _Usuari_. Aquí podrem marcar els usuaris que tinguin accés i quin tipus d'accés a la col·lecció.

## Permisos per repositoris

A diferència de la resta de punts és millor evitar crear un compte nou associat al nou correu electrònic. Normalment, al tractar-se de programadors, és molt probable que la persona ja tingui compte com a mínim a Github. A banda, és millor tenir-hi una única identitat. En cas que no en tingui, cal que el creï previament.

Per això aquí només es tracta de donar els permisos necessaris per tenir accés als repositoris que tenim allotjats a Github i Gitlab.

### Github

Cal afegir l'usuari a l'equip de _Contributors_ de l'organització de Coopdevs a Github. Això sembla ser que només ho poden fer els usuaris que tenin rol de _Maintainer_ de l'organització: a data d'avui l'Enrico i en Naoise.

### Gitlab

Idealment el millor seria poder replicar els dos equips que tenim configurats a Github però sembla que els permisos de Gitlab a nivell d'organització només funcionen amb rols. Així doncs, tant en Dani com l'Enrico i en Pau són _Owners_ de de coopdevs. La resta han de ser _Maintainers_. Això els atorga permisos suficients per no haver de dependre de la resta.

Navega a _Members_ des del menú lateral de la [pàgina d'organització](https://gitlab.com/coopdevs). Des de la nova pàgina senzillament afegeix el nom d'usuari o mail de la persona, assignali el rol _Mantainer_ i clica a _Add to group_.

## Calendari

Per compartir el calendari de Coopdevs, actualment de Google, navega a _Configuració i compartir_ clicant sobre el calendari. Un cop a la configuració, clica al botó _Afegeix persones_, afegeix el seu mail i selecciona l'opció _Veure tota la informació de l'esdeveniment_ o fer _Fer canvis a esdeveniments_ segons convingui.

### Configuració amb un gestor de calendari

Per aquells que vulguin configurar-se el calendari al seu gestor local, des de la mateixa pàgina de configuració, desplaça't a la secció _Integra el calendari_ i comparteix les adreces en format iCal amb la persona interessada. Amb això hauria de ser suficient.

### Documents

Donar accés al Drive i al NextCloud de Coopdevs Treball SCCL