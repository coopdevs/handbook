> Esta pagina sale de una tarea de investigar el funcionamiento de las colas en Odoo. También hay algunas sugerencias de como podemos monitorear estas colas.

## Funcion del Queue Job

https://github.com/OCA/queue/blob/12.0/queue_job/jobrunner/runner.py#L10

```
* It starts as a thread in the Odoo main process or as a new worker
* It receives postgres NOTIFY messages each time jobs are
  added or updated in the queue_job table.
* It maintains an in-memory priority queue of jobs that
  is populated from the queue_job tables in all databases.
* It does not run jobs itself, but asks Odoo to run them through an
  anonymous ``/queue_job/runjob`` HTTP request.
```

Odoo levanta por defecto un solo worker con diferentes threads para ejecutar los cronjobs y el jobrunner:

Al indicar en el odoo conf que queremos más de un worker vemos que Odoo levanta:

* 2 WorkerCron
* 1 WorkerJobRunner
* 1 proceso que gestiona el longpolling, pero que es diferente al resto de workers.
* X WorkerHTTP, donde X es el número de workers indicado.

Los jobs se escriben en tablas que se van consultando.
Al generar un nuevo job en la tabla, PostgreSQL notifica al JobRunner de que hay un nuevo job en la cola para que este envie el job a ejecutar a los WorkerHTTP de Odoo por HTTP.

## Como se procesa un job?

1. El proceso prinpipal de Odoo (HTTP) aplaza la ejecucion de una tarea ("encola").
2. Esto genera un registro en db. PostgreSQL envia una NOTIFY anunciando que se ha modificado la tabla de jobs.
3. El JobRunner escucha esta notificación y busca el job en la tabla. [code](https://github.com/OCA/queue/blob/12.0/queue_job/jobrunner/runner.py#L420)
4. JobRunner hace una llamada HTTP al proceso HTTP de Odoo para que ejecute el job con el id del registro del job en DB. [code](https://github.com/OCA/queue/blob/12.0/queue_job/jobrunner/runner.py#L404)
5. Odoo (HTTP) ejecuta la tarea.
6. Envia la respuesta con el resultado de la tarea. Si esta no es con un status code de entre 400 y 600 la tarea se da por ejecutada, si no se vuelve a intentar o se da por fallida.

## Configuración

https://github.com/OCA/queue/tree/12.0/queue_job#id12

* Workers

Define el número de procesos de HTTPWorker que quieres levantar.

```
[options]
(...)
workers = 6
(...)
```

* Channel

Define los canales y los jobs que se pueden definir en paralelo de cada canal:

```
[queue_job]
channels = root:2
```

## Monitoring

Tenemos dos puntos que pueden generar problemas:

* El JobRunner que escucha a PostgreSQL
* El thread/worker HTTP que ejecuta el job

* Como sabemos cuantos jobs quedan por ejecutar? (DB)

```
select count(*) as total from queue_job where state in ('pending', 'enqueued');
```

* Estado de los jobs? (DB)

```
select count(*) as total, state from queue_job group by state;
 total | state  
-------+--------
     5 | done
    24 | failed
(2 rows)
```

También podemos sacar información si analizamos los logs.

No vemos que exista instrumentalización para el stack de Grafana, pero hemos encontrado este script que instrumentaliza las llamadas RPG:

https://gist.github.com/mlaitinen/f3ae45d1e3ee60a4af907fef7b585059