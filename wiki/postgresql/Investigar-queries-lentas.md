# Introduccion

Comenzamos a investigar las queries que se realizaban al investigar un importador de CSVs que se utilizaba en una instancia de tryton 3.8.

Este importador cargaba en base de datos una serie de registros de consumo. Para ello se necesitaba conocer el contrato al que iban asociados, el tipo de producto al que pertenecía el consumo y la linea de factura a la que se enlazaría.

> *Podemos aprovechar para estudiar el modelo de datos que utilizamos, ya que la linea de factura ya contiene el tipo de producto y el contrato relacionadas. No se aprovecha el modelo relacional de Postgresql.*
este proceso, con todas sus comprobaciones, era muy costoso.


# Mejoras realizadas

## Código - Python/Tryton

Lo primero que hicimos fue estudiar el metodo que se encargaba de realizar las transformaciones de datos crudos en referencias a base de datos (`id`).

Rápidamente vimos que este método era muy genérico, por lo que decidimos hacer un *workaround* y crear un método para la importación de CSVs de consumos.
Esto mejoró el coste del importador.

Esta primera iteración sobre el algoritmo de importación mejoro la *performance* pero aun se podían hacer muchas mejoras, gestionando mejor la cache para disminuir el numero de accesos a base de datos. 

> *Estudiar si este  tipo de comportamientos influye en procesos concurrentes y si no estamos asumiendo el trabajo de Postgres desde el propio Tryton.*

## Base de datos - Postgresql

Al ver que aun así habían procesos de Postgresql que permanecían abiertos durante horas utilizando el comando `top`, investigamos que tipo de consultas se realizaban y cual era su coste.

```
  PID USUARIO   PR  NI    VIRT    RES    SHR S  %CPU %MEM     HORA+ ORDEN
  986 postgres  20   0  311008 134888 129732 R  77,0  1,7   1h:36.80 postgres
  887 ubuntu    20   0  853556 209940  26852 S  26,0  2,6   1h:20.66 python
```
Utilizando `htop` podemos saber que acción esta realizando en concreto:

```
  986 postgres  20   0  303M 147M 129732 R  77,3  1,9   1h:39.80 postgres: ubuntu test [local] SELECT
```

> *También podemos utilizar `ps faux` para ver los procesos que se están ejecutando.*

Decidimos capturar exactamente esa *query*, para ello utilizamos la tabla [`pg_stat_activity`](https://www.postgresql.org/docs/9.2/static/monitoring-stats.html#PG-STAT-ACTIVITY-VIEW).

### PG_STAT_ACTIVITY

Realizando una búsqueda por el PID del proceso: `select query from pg_stat_activity where pid = <PID>;`
```
# select query from pg_stat_activity where pid = 1824;
-[ RECORD 1 ]----+-------------------------------------------------------------------------------------------------------------------------------------
query            | SELECT "a"."id" AS "id" FROM "import_csv_log" AS "a" WHERE (("a"."parent" IN (3642820))) ORDER BY "a"."date_time" DESC, "a"."id" ASC`
```
Así podemos saber que consulta se ha quedado bloqueada.

Para loggear todas las queries que se realizan y calcular cual es la más costosa, decidimos probar el modulo de Postgresql [`pg_stat_statements`](https://www.postgresql.org/docs/current/static/pgstatstatements.html)

### LOGGIN QUERIES

#### PG_STAT_STATEMENTS

Este módulo monitoriza las consultas almacenando información sobre el coste (tiempo) de cada consulta, las rows que son devueltas, a cuantas accede a leer (comprobación de una condición) y muchos detalles más.
Debe activarse añadiéndolo al fichero de configuración de Postgresql:

```
# postgresql.conf
shared_preload_libraries = 'pg_stat_statements'
```
Más información en [`shared_preloaf_libraries` Postgres wiki entry](https://www.postgresql.org/docs/current/static/runtime-config-client.html#GUC-SHARED-PRELOAD-LIBRARIES).

Seteamos también las siguientes opciones:
```
pg_stat_statements.track = all
```

Una guardados los cambios y reiniciamos el servicio de Postgresql.

> `reaload` no cargará los cambios. `shared_preload_librearies` necesita un `restart` para hacerse efectivo. Si realizamos un `reload` encontramos en los logs:
```
2018-02-10 12:47:15 UTC [334-5] LOG:  parameter "pg_stat_statements.track" removed from configuration file, reset to default
2018-02-10 12:47:15 UTC [334-6] LOG:  parameter "shared_preload_libraries" cannot be changed without restarting the server
```

Una vez activado, debemos crear la extensión en la base de datos que queremos monitorizar:
```
# CREATE EXTENSION pg_stat_statements;
```

Una vez creada la extensión, realizamos los procesos que realizan las consultas que queremos invesstigar para obtener métricas.
*Idealmente se debe realizar en producción. Activandolo con tiempo para tener unos datos lo más realistas posibles.*

Una vez tenemos algo de volumen de datos, podemos realizar las cosnsultas sobre la tabla `pg_stat_statements`.
Para ello utilizamos las consultas propuestas en la [entrada de `pg_stat_statements` de la wiki de Postgres](https://www.postgresql.org/docs/current/static/pgstatstatements.html):
```
postgres=# SELECT query, calls, total_time, rows, 100.0 * shared_blks_hit /
               nullif(shared_blks_hit + shared_blks_read, 0) AS hit_percent
          FROM pg_stat_statements ORDER BY total_time DESC LIMIT 2;
-[ RECORD 1 ]---------------------------------------------------------------------
query       | UPDATE pgbench_branches SET bbalance = bbalance + $1 WHERE bid = $2;
calls       | 3000
total_time  | 9609.00100000002
rows        | 2836
hit_percent | 99.9778970000200936
-[ RECORD 2 ]---------------------------------------------------------------------
query       | UPDATE pgbench_tellers SET tbalance = tbalance + $1 WHERE tid = $2;
calls       | 3000
total_time  | 8015.156
rows        | 2990
hit_percent | 99.9731126579631345
```

Así vemos que llamadas son las más frequentes y ms costosas.
Podemos ir reiniciando las metricas obtenidas utilizando `# SELECT pg_stat_statements_reset();`

## Investigando una consulta - EXPLAIN ANALYZE

Una vez tenemos la consulta localizada, pasamos a analizar su ejecución. Para ello utilizamos el método `EXPLAIN` con el argumento `ANALYZE`.
Este nos devuelve los pasos que realiza la consulta para ejecutarse y al añadir el `ANALYZE` nos devuelve además del coste estimado que devuelve el `EXPLAIN` al no llegar a ejecutarse la consulta, el coste real, ya que la consulta si que se ejecuta.
