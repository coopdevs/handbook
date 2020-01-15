A continuació es detalla com es sincronitzen actualment usuaris de Katuma a una llista de Mailchimp per a poder adreçar-nos-hi amb campanyes de mail.

Ara per ara, el procés és plenament manual. Obtenim els emails dels usuaris de la base de dades i els afegim a una llista existent de Mailchimp a través d'un CSV.

## Exportar contactes de Postgres

Entra al servidor de producció i executa la següent comanda des d'una sessió de psql. Pots obrir-ne una executant `bundle exec rails dbconsole` o amb l'habituals `psql -h localhost openfoodnetwork ofn_user`.

### Exportar usuaris individuals

Executa la següent sentència a la base de dades:

```sh
openfoodnetwork=> \COPY (select email as "Email Address" from spree_users) TO '/tmp/user_emails.csv' WITH (FORMAT CSV, HEADER);
```

Això crearà l'arxiu `/tmp/users_emails.csv` amb el format que s'espera Mailchimp i no hauria de caldre cap modificació.

Ara pots descarregar l'arxiu al teu ordinador executant la següent comanda:

```sh
$ scp openfoodnetwork@app.katuma.org:/tmp/user_emails.csv ~/Documents/.
```

Open Food Network no emmagatzema el nom de l'usuari i per tant a Mailchimp només disposarem del mail. Una opció a valorar seria exportar els consumidors però això només contindria els membres de grups de consum o qualsevol organització que els afegeixi com a consumidors per a qualsevol altre ús.

### Exportar contactes d'organització

Executa la següent sentència a la base de dades:

```sh
openfoodnetwork=> \COPY (select email_address as "Email Address", contact_name as "First Name" from enterprises) TO '/tmp/organizations_emails_and_contact_name.csv' WITH (FORMAT CSV, HEADER);
```

Això crearà l'arxiu `/tmp/organizations_emails_and_contact_name.csv` amb el format que s'espera Mailchimp i no hauria de caldre cap modificació.

Ara pots descarregar l'arxiu al teu ordinador executant la següent comanda:

```sh
$ scp openfoodnetwork@app.katuma.org:/tmp/organizations_emails_and_contact_name.csv /tmp/.
```

Això, al contrari que pels usuaris individuals exporta tant el mail com el nom de la persona de contacte de l'organització. Això omplirà els camps "Email Address" i "First Name" de l'audiència a Mailchimp.

## Importar CSV a Mailchimp

Primer de tot cal crear una nova audiència si volem fer una campanya en especial. Normalment el que volem és enviar un mail a tots els usuaris de Katuma. En aquest cas haurem d'actualitzar l'audència "Usuaris de Katuma". Si per contra només volem contactar amb els gestor d'organització haurem d'utilitzar l'audiència "Organitzacions registrades a Katuma".

Un cop a la pàgina de gestió de l'audiècia, clica a _Add contacts > Import contacts_. Això iniciarà el procés d'importació. Quan el sistema demani el format especifica _CSV or tab-delimited text file_.

![Importació d'usuaris de Katuma a Mailchimp](https://github.com/coopdevs/handbook/wiki/img/import_contacts_mailchimp.png)

Un cop seleccionat i pujat el document CSV hi ha un últim pas de confirmació. Repassa'l i assegura't que tots els valors són correctes, entre ells l'estat que tindran els contactes importats. Per ara hem fet servir _Subscribed_ però hi ha altres opcions com _Unsubscribed_ que poden servir per netejar la llista de contactes no desitjats, per exemple.

Per últim, a la mateixa pàgina de confirmació a la secció _Sync with existing contacts_ és recomanable seleccionar _Update existing contacts_ per mantenir l'audiència actualitzada. Ara per ara, com que l'única informació que proporcionem és el correu electrònic no provocarà cap canvi.
