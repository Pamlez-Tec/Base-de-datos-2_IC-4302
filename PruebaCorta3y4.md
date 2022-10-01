1. **Explique el concepto de Write Concern en MongoDB. (10 pts)**  

"Write concern" existe para que se pueda tener confirmacion de que los datos se han escrito en el número de nodos indicado,por ejemplo,  me confirma si se terminó la escritura cuando se guardó en disco, me confirma si se terminó la escritura cuando lo tiene en memoria principal. Tiene que ver con las operaciones de escritura y a una mejor durabilidad de datos. 


2. **Explique diferencias entre bases de datos NoSQL y SQL, tome como ejemplo las
bases de datos estudiadas en clase y utilizadas en proyectos y tareas: (40 pts)**
a. MongoDB
b. Elasticsearch
c. SQL Server
d. MySQL
e. PostgreSQL  

Tomando como ejemplo MariaDB, SQL Server y una vista general. La primera diferencia es el esquema de datos que manejan, por ejemplo en mongoDB es dinámico y evolutivo con SQL Server es un esquema fijo que está predefinido(No cambia), otra diferencia es la optimización de las consultas, las bases de datos NoSQL son mejores en este caso cuando se trata de consultas con Big Data.En mongoDB el escalado es horizotal, en donde los datos se distribuyen entre clústers, en cambio en SQL Server es un escalado vertica, aquí se agregan recursos físicos o virtuales al servidor donde se aloja la BD. De manera general también se puede indicar que no todas las bases de datos NoSQL tienen el atributo de atomicidad, por lo general su consistencia es eventual en las SQL hay atomicidad se hace o no la operación entera.

3. **Desde un punto de vista de una base de datos de series de tiempo, ¿Porqué la
localidad de datos es relevante para la escogencia del hardware a utilizar?, puede
justificar su respuesta utilizando los data tiers de algún motor de bases de datos
como Elasticsearch. (40 pts)**  

Porque depende de cómo se distribuya los data tier para saber qué tipo de hardware se va a escoger. El patrón de los usuarios cambia, una información puede ser warm, cold, frozen. Cuando tenemos un data tier warm, el procesamiento que deseamos no debe ser extremadamente poderoso, pero la memoria sí lo debe ser, en este caso se debe escoger un hardware que cumpla con esos requisitos, por lo general pueden ser más caros, Cuando los datos son de tipo Frozen a estos no les podemos cambiar los datos y y el storage es más barato.

4. **Explique el concepto de Federated Queries y el impacto que tienen estas en el
rendimiento de bases de datos. (10 pts)**  

Las federated queries son consultas que unen tablas de diferentes conjuntos de datos, en otras palabras podemos hacer uso de de múltiples fuentes en una sola consulta. Por ejemplo podemos usar datos que se encuentren en otro data lake o data warehouse sin necesidad de duplicar los datos. Con respecto al rendimiento, es un impacto positivo pues la implementación se hace de una manera super rapida y el costo se disminuye, antes consolidar los datos de varias fuentes era muy tedioso y largo, traducido a un menor rendimiento.

Referencias:

Trianz.(2021). Query Federation – Is It Worth the. Recuperado de: Hype?https://www.trianz.com/insights/query-federation-is-it-worth-the-hype

Carros.R.(2022) ¿QUÉ ES UN WRITE CONCERN (COMETIDO DE ESCRITURA) EN MONGODB? ¿QUÉ ES UN READ CONCERN (COMETIDO DE LECTURA) EN MONGODB?, Recuperado de: 
https://www.ramoncarrasco.es/es/content/es/kb/140/que-es-un-write-concern-cometido-de-escritura-en-mongodb-que-es-un-read-concern