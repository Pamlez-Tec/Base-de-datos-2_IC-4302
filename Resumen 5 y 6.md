# **Spanner: Becoming a SQL System**

**Google’s Spanner**  

* Almacén clave - valor
* Múltiples filas
* Consistencia externa
* Conmutación por error
* En el transcurso de los años: Sistema de base de datos relacional.
* Evolucionó la arquitectura de la pila de almacenamiento.
* Las demandas del procesador de consultas, hizo que evolucionara la forma en que se almacena y administra los datos.  

Spanner en la actualidad se usa como sistema de administración de base de datos OLTP para datos estructurados en Google. El sistema brinda consistencia transaccional y réplicas altamente consistentes, así como alta disponibilidad.  

Las consultas en Spanner implementan el dialecto SQL, se basa en ANSI SQL estándar y utiliza ARRAY y tipo de fila (struct). El procesador de consultas atiende una combinación de cargas de trabajo transaccionales y analíticas que admite consultas de baja latencia y de larga duración.  

**Aspectos exclusivos de Spanner:**  

* Ejecución de consultas distribuidas, compilación y ejecución de uniones.
* Extracción de rango, el cual se decide cúal servidor debe procesar una consulta.
* Manejo de fallas del lado del servidor cuando la consulta se reinicia. Spanner oculta las fallas transitorias.
* Adoptan un dialecto SQL común. Cumplen la compatibilidad entre Spanner y otros sistemas.
Reemplazo de pila SSTable.

**Background**  

Sistema  relacional fragmentado y replicado geográficamente. Fragmentos se distribuyen entre varios servidores y luego se replican en varios centros de datos separados geográficamente.  

Las transacciones utilizan registro de rehacer de escritura anticipada. En la arquitectura de Spanner se utiliza multipaxos , en donde se elige un único líder de larga duración que confirma la entrada de registros en paralelo que busca alcanzar un alto rendimiento.   

Combinación de bloqueo pesimista y marcas de tiempo  para el control de la concurrencia. El bloqueo de dos fases garantiza la serialización dentro de un grupo de paxos, la confirmación de dos fases garantiza serialización en toda la BD.  

Marco del coprocesador, determina qué grupo de paxos posee los datos a los que se dirige y busca la réplica más cercana de ese grupo, que esté actualizada para la concurrencia especificada. Además oculta fallas de servidor, de red, etc.  

**Distribución de consultas**  

Compilación de consultas distribuidas: Construye árbol de operadores de álgebra relacional y lo optimiza usando reescrituras equivalentes. Existe un escaneo global de la tabla en donde se reemplaza por una operación distribuida explícita que realiza escaneo local de los fragmentos de la tabla, ordena los fragmentos con el orden de las claves de la tabla.  

Permite el intercalado de tablas y si se hace una unión entre estas tablas, son empujadas hacia abajo. Suelen ser uniones antepasadas-descendientes en la jerarquía de la tabla y cuando se ejecutan se hacen como uniones locales en fragmentos de tabla.  

**Ejecución distribuida:**  
La poda de fragmentos aprovecha las claves de rango de los fragmentos y depende de empujar hacia abajo las condiciones de las claves de fragmentos de las tablas hasta el nivel inferior. Antes de realizar una llamada remota para ejecutar su subconsulta, Distributed Union realiza un análisis en tiempo de ejecución del filtro de clave de fragmentación para extraer un conjunto de rangos de claves de fragmentación. Se garantiza que el conjunto de rangos extraídos cubre  todas las filas de la tabla en las que la subconsulta puede generar resultados.  

Distributed Union, genera subconsultas a cada fragmento o grupo en paralelo, esto hace que se reduzca la latencia. Durante la ejecución, DU detecta que los fragmentos de destino están alojados localmente en el servidor y evita realizar la llamada remota y la ejecuta localmente.  

**Uniones distribuidas:**  
La unión de aplicación por lotes, se utiliza para unir un índice secundario y su tabla base de forma independiente. Distributed Apply, permite minimizar la cantidad de llamadas entre máquinas para uniones basadas en claves y paralelizar la ejecución.  

**API de distribución de consultas:**  
API de consumidor único (Proceso de cliente único consume los resultados de una consulta). API de consumidor paralelo (Consumir resultados de consultas en paralelo de varios procesos que se ejecutan en varias máquinas).  

**Extracción de rango de consulta:**  
Proceso de analizar una consulta y determinar a qué partes de las tablas hace referencia la consulta. Se emplean varios tipos de extracción:  

* Extracción de rango de distribución.
* Extracción de rango de búsqueda.
* Extracción de rango de bloqueo.  

Las diferencias entre los rangos de distribución, búsqueda y bloqueo están en que la elección de la longitud del prefijo de clave utilizada para especificar los intervalos de clave.  

**Las dos técnicas para extraer los rangos son:**  
Momento de compilación: Se normaliza y se reescribe  una expresión de exploración filtrada en un árbol que extrae los rangos para columnas clave sucesivas.  

**En tiempo de ejecución:**  
Se usa una estructura que se llama árbol de filtro para calcular los rangos a través de la aritmética de intervalo ascendente y para una evaluación de las condiciones posteriores al filtrado. Este árbol se comparte entre todos los análisis correlacionales producidos por la reescritura en tiempo de compilación. Este árbol memoriza los resultados de los predicados cuyos valores no han cambiado y reduce el cálculo del intervalo.  

**Query restarts:**  
La biblioteca del cliente reanuda la ejecución de consultas instantáneas si se producen errores u otros eventos, aún si se consumen resultados parciales.  

**Beneficio de estas query restarts:**  
Se ocultan los errores transitorios durante la ejecución de consultas, en otras palabras una transacción instantánea nunca devolverá un error en donde el cliente deba intentarlo de nuevo. Algunas de las fallas que se ocultan son:  

* Desconexiones de red, reinicios de máquinas y bloqueos de procesos.
* Espera distribuida, sobrecarga de servidor, falta de disponibilidad de los datos, etc.  

El beneficio de las actualizaciones continuas recurrentes, la compatibilidad con los reinicios de consultas complementa otras operaciones reanudables como lo son las actualizaciones de esquemas y la creación de índices en línea.  

Cuando el servidor no puede ejecutar una solicitud, por ejemplo, por exceso de CPU lo que se hace es devolver un código de error de reintento interno y confiar en el reinicio para no desperdiciar una cantidad considerable de recursos cuando se vuelve a intentar.  

**Requisitos para las query restarts:**  
Se acompañan con los token de reinicio. Este sistema utiliza reinicios de consulta para continuar la ejecución cuando un servidor pierde la propiedad de un fragmento o los límites de un fragmento cambian.   

**Dialecto SQL común:**  
Para garantizar coherencia entre los sistemas se crean varios componentes compartidos  como: Front-end del compilador, el segundo es una biblioteca de funciones escalares; el tercero es un marco de prueba compartida (pruebas de cumplimiento y pruebas de cobertura).  

**Almacenamiento en columna en bloques:**  
Utiliza SSTable(tabla de cadenas ordenadas) heredado de Bigtable está optimizado para datos NoSQL. Se utiliza Ressies que es un nuevo formato de almacenamiento de bajo nivel, que está diseñado para manejar consultas SQL en bases de datos distribuidas a gran escala que comprenden cargas de trabajo OLTP y OLAP.  

**Lecciones aprendidas, retos y conclusiones:**  
Inicialmente Spanner proporcionaba métodos NoSQL para búsquedas de puntos y escaneos de rango de tablas individuales e intercaladas pero que ahora se utilice SQL brinda un valor adicional al expresar patrones de acceso a datos más complejos y empujar el cálculo a los datos.  

Se proporcionan una amplia gama de opciones de diseño de datos físicos, como: ubicación geográfica, replicación , uso de búfer de protocolo, partición vertical de tablas dentro de fragmentos, entre otros.  

No se utilizan particiones de tablas estáticas, por lo tanto los límites de los fragmentos pueden cambiar dinámicamente debido al equilibrio de carga o la reconfiguración.  

Se utilizan varias configuraciones de replicación para reducir las latencias de lectura/escritura y el costo de replicación de cargas de trabajo diferentes.


