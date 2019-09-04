# Secure TCP/IP Connections with SSL

One requeriment in the OTRS project is to expose the database to read the clients of them. OTRS need a URL, User and Password to access to our DB.

Exevit proposes to use a VPN to secure the connection, but this is a overkill security requirement beacuse need a lot of mantainance to be working propertly.

On the other hand, we propose a [Secure TCP/IP Connection with SSL](https://www.postgresql.org/docs/9.6/static/ssl-tcp.html).

To create this secure connection we need do the nexts steps:

### 1º Open postgres to listen a TCP/IP connection
In the Postgres configuration file we need to change the next lines:

* Enable TCP/IP connections to listen IPv4: `listen_addresses = 0.0.0.0`
* Change the default listening port: `port = 11392` --> Use a secure port, out of next range 1024 - 10000

> One tip to be more secure is deny the superuser access: `superuser_reserved_connections = 0` and remove the superuser line of the `pg_hba.conf` if exists.

### 2º Enable SSL access
In the Postgres configuration file we need to change the next lines:

* Enable SSL connections: `ssh: true`

### 3º Creates the SQL view with the data needed
We propose create a SQL view to acces to the data. We do not expose the entire data schema, only the data that the provider needs. By this reason, we create a SQL `view` to expose only the data that we want expose:

* Create a SQL `view` with the script `psql -U eticom -W eticom < scripts/sql/party_otrs_view.sql`

### 4º Creates a user (role) and add GRANT permissions
We need create a user to access our data:

* Create user using the script: `./create_OTRS_readonly_role.sh <db-name> <new-password>`

### 5º Add access in the `pg_hba.conf`
Once time we have the user, the view with the data and the GRANT permissions, we need access to this user from out of the server with a URI, user and password.
To make this possible, we need change the `pg_hba.conf` file:

* Add the next line to `pg_haba.conf`: `hostssl <db-name> <username> <IP>/32  md5`

> You need to be sure that you don't have other entry that give access with other permission to the db. Only the localhost and the IP that you know.

### 6º More Security

* We can change the `schema` where the view is created and only create GRANT permission rules for this `schema`. With this change only this schema relations can to be viewed by th user.
* We can use Certificates to do more secure the connection and aboid the attack named `'man in the middle'`
* https://serverfault.com/questions/627169/how-to-secure-an-open-postgresql-port