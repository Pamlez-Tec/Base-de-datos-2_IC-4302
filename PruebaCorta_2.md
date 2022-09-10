Pamela González López - 2019390545

#### Prueba corta #2

1. Explique el concepto de shard, replica y partition.
**Shard:** Es un fragmento de datos. La idea del shard, especialmente en base de datos NoSQL, es algo que pueda entrar en memoria completamente. Lo que viene a hacer un shard a nivel de base de datos es que cuando se parte la info en shards estos deberían distribuirse dentro de todos los workers que tenemos, pues cuando un usuario hace una consulta, esta debe ejecutarse en todas las réplicas por igual.
**Replica:** Consiste en tener copias de datos distribuidas en varios servidores. Por ejemplo se tenemos una base de datos fragmentada en 4 partes a, b c, d, podríamos tener a y b guardadas en el servidor 1, b y c en el servidor 2 y a, d en el servidor 3.
**Partition:** En base de datos sql, las particiones son divisiones de esa base de datos. Por lo general esas particiones pueden ser distribuidas en varios nodos y los usuarios del nodo realizan transacciones de manera local en la partición.  Uno de los motivos de las particiones es para equilibrar la carga.
2. Explique la diferencia entre strong consistency y eventual consistency.
**Strong consistency:** Es cuando aseguramos completamente que la información está actualizada. Si se debe mantener una consistencia fuerte se debe evitar las actualizaciones simultaneas que puedan afectar los datos.
**Eventual consistency:** Eventual porque  se puede leer los datos aún sin ser la última versión. Por ejemplo, en aplicaciones como wish que muchas veces vemos un precio de un artículo en un momento determinado pero cuando lo vamos a comprar su precio cambió en ese lapso.
3. ¿En qué consiste warm replicas y hot replicas?
**Warm replicas:** Estamos forzando consistencia fuerte. Primary y slave deben ser iguales. No aceptará conexiones para que le pidan datos. 
**Hot replicas:** Estamos trabajando en consistencia eventual. Hay lectura del stand by, aun si los datos no son consistentes. Nodos aceptan conexion de lectura y están dispuestos a dar información bajo la salvedad de que esa información no está consistente.
4. ¿En qué consiste switch over y fail over?
**Switch over:** Consiste en que una base de datos en espera se convierte en una base de datos principal y la base de datos principal se convierte en una de espera. No hay perdida de datos.
**Fail over:** Es cuando una base de datos principal falla, una de las bases de datos en espera asume su función principal. Puede provocar o no la pérdida de datos.


Referencias:

Oracle help center (2022). Oracle Database Online Documentation 10g Release 2. Recuperado el 09 de septiembre del 2022, de https://docs.oracle.com/cd/B19306_01/server.102/b14230/sofo.htm

Reyes A. (2022). Uam.mx. Base de datos distribuidas. Recuperado el 09 de septiembre del 2022, de http://aisii.azc.uam.mx/areyes/archivos/Licenciatura/BDD/Replicacion.pdf
