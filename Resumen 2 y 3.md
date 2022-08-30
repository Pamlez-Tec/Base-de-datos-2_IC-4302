# Resumen 2 y 3
# **Bigtable:  Un sistema de almacenamiento distribuido para datos estructurados**

Es un sistema distribuido para almacenar datos estructurados en Google. Está creado para escalar a un tamaño muy grande, en otras palabras, a petabytes de datos en muchos servidores.

**Bigtable ha conseguido varios objetivos:**

* Una amplia aplicabilidad,
* Escalabilidad,
* Un alto rendimiento y
* Una alta disponibilidad.

No admite un completo modelo de datos relacional, lo que busca es la simplicidad en su modelo de datos y para brindar esa simplicidad a los clientes, concede sobre el diseño y el formato un control dinámico. Aprueba que los clientes razonen sobre las propiedades de localidad de los datos representados en el almacenamiento subyacente.

Con respecto a los datos, estos se indexan por medio de nombres de filas y columnas que pueden ser cadenas arbitrarias. El bigtable lo que hace es manejar los datos como cadenas no interpretadas, pero los clientes lo que hacen es serializar varias formas de datos estructurados y semiestructurados en esas cadenas. Estos también se les permite manejar la localidad de sus datos por medio de elecciones metódicas en sus esquemas. En bigtable los parámetros permiten a los clientes que los datos sean entregados desde la memoria o desde el disco, todo de manera dinámica.

**Data Model**

El bigtable se puede definir como un mapa ordenado multidimensionalmente:
* Persistente,
* Es distribuido y 
* Disperso

Este mapa definido anteriormente está indexado por:
* Una clave de fila, 
* Una clave de columna y 
* Una marca de tiempo.

Además, cada valor en el mapa es una matriz no interpretada de bytes.

**Rows**

Se hacen particiones de manera dinámica en los rangos de las filas de una tabla. Estos rangos de fila, cada uno se llama tablet, que consiste en la unidad de distribución y equilibrio de carga.
Como consecuencia, las lecturas de rangos de filas cortos son eficientes y usualmente necesitan comunicación con una cantidad de máquinas muy pequeña.

Con respecto a las claves de las filas, estas son cadenas arbitrarias que en la actualidad tienen un tamaño de 64 KB aproximadamente. Los datos se mantienen en un orden lexicográfico por clave de fila, en el bigtable.

**Column Families**

Para nombrar una clave de columna se usa la sintaxis: family:qualifie. Estos nombres son imprimibles y los calificadores son cadenas arbitrarias.

 Todos los datos almacenados en una familia de columnas suelen ser del mismo tipo (los datos de la misma familia de columnas se comprimen juntos). Estas familias de columnas son conjuntos de agrupaciones de claves de columnas, éstas forman la unidad básica de control de acceso. Se debe crear una familia de columnas antes de que los datos se puedan almacenar bajo cualquier clave de columna en esa familia. Cuando ya se ha creado una familia, se puede utilizar cualquier clave de columna dentro de la familia.

**Timestamps**

Las marcas de tiempo son números enteros de 64 bits.
* Tiempo real: Cuando son asignados por Bigtable en microsegundos 
* Explicicitamente: Cuando los asigna el cliente.

Cada celda de Bigtable puede contener varias versiones de los mismos datos. Estas versiones están indexadas por marca de tiempo.

**API**

Brinda funciones para:
* Crear
* Eliminar tablas y familias de columnas.

Brinda funciones para cambiar los metadatos de: 
* Grupos
* Tablas y 
* Familias de columnas, como los derechos de control de acceso.

**Building Blocks**

* **Chubby:** Servicio de bloqueo distribuido persistente y de alta disponibilidad. 
* Un servicio Chubby tiene cinco réplicas activas, una se elige para ser la maestra y esta se ocupa activamente de las solicitudes. Sabemos que el servivio está activo cuando la mayoría de las réplicas se están ejecutando y logran comunicarse entre sí.
* Se utiliza el sistema de Google distribuido (GFS) para almacenar archivos de registro y datos. 
* Un clúster de Bigtable opera en un grupo compartido de máquinas que ejecutan una gran variedad de otras aplicaciones distribuidas.
* Los procesos de Bigtable a menudo comparten las mismas máquinas con procesos de otras aplicaciones. 

**Implementation**

Se tiene tres componentes principales: 
* Una biblioteca que está vinculada a cada cliente, 
* Un servidor maestro y 
* Muchos servidores de tabletas.

Ampliando:
Los servidores de tablets pueden ser agregados o eliminados dinámicamente de un clúster para adaptarse a los cambios en las cargas de trabajo.

El maestro se encarga de: 
* Asignar tablets a servidores de tablets,
* Detectar la adicción y el vencimiento de servidores de tablets, 
* Equilibrar la carga del servidor de tablets,
* Recolección de basura de archivos en GFS,
* Maneja los cambios de esquema, como la creación de familias de tablas y columnas.

**Tablet Location**

Se usa una jerarquía de tres niveles.
* El primer nivel: Es un archivo almacenado en Chubby que contiene la ubicación de la tabla raíz esta tabla contiene la ubicación de todas las tablas en especial la tabla de METADATOS.
* Segundo nivel: Otras tablas de metadatos.
* Tercer nivel: User table.

**Tablet Assignment**

Cuando una tablet no está asignada y hay disponible un servidor de tabletas con suficiente espacio, el maestro asigna la tablet enviando una solicitud de carga al servidor.

**Tablet Serving**

* El estado persistente de una tablet se almacena en GFS. 
* Las actualizaciones se confirman en un registro de confirmación que almacena registros. 
* Las actualizaciones más nuevas se almacenan en la memoria, en un búfer ordenado.
* Las actualizaciones más antiguas se almacenan en una secuencia.

**Compactions**

 Cuando el tamaño de la tabla de memoria alcanza un cierto tamaño entonces:
 * La tabla de memoria se congela
 * Se crea una nueva tabla de memoria
 * La tabla de memoria congelada se convierte en una SSTable y 
 * Se escribe en GFS. 
 
 Este proceso de compactación menor tiene dos objetivos: 
 1) Reduce el uso de memoria del servidor de la tableta.
 2) Si el servidor muere, reduce la cantidad de datos que deben leerse del registro de confirmación durante la recuperación.
 
 Las operaciones de lectura y escritura entrantes pueden continuar mientras se producen las compactaciones.

**Refinements**

Tiene como bases: 
* **Grupos de localidad:** Se pueden agrupar varias familias de columnas en ungrupo 
de localidad.
* **Compresión:** Se pueden controlar si se comprimen (Y el formato de compresión que se utilizará) o no las SSTables para un grupo de localidad.
* **Almacenamiento en caché para rendimiento de lectura:** Se utiliza Scan Cache, que es de nivel superior o Block Cache, que es de nivel inferior.
* **Filtros de floración:**  Permite preguntar si una SSTable podría contener 
algún dato para un par específico de fila/columna.
* **Implementación de registro de confirmación:** El uso de un registro de confirmación proporciona ventajas de rendimiento durante el funcionamiento normal, pero complica la recuperación.
* **Acelerar la recuperación de la tableta:** Si el maestro mueve una tableta se hace una compactación menor de esa tableta, esa compactación reduce el tiempo de recuperación.
* **Explotando la inmutabilidad:** La inmutabilidad de SSTables permite dividir tabletas rápidamente.

**Aplicaciones Reales**

_Google Analytics_
* Tiene la tabla de clics que mantiene una fila para cada sesión de usuario final y la tabla de resumen que contiene varios resúmenes predefinidos para cada sitio web.
* Es un servicio que ayuda a analizar patrones de tráfico en sus sitios web.
* Proporciona estadísticas, como la cantidad de visitantes únicos por día y las páginas vistas por URL por día.

_Google Earth_
* Este sistema utiliza una tabla para preprocesar los datos y un conjunto diferente de tablas para servir los datos del cliente.
* Permiten a los usuarios navegar por la superficie del mundo: pueden desplazarse, ver y anotar imágenes satelitales en muchos niveles diferentes de resolución. 

_Búsqueda personalizada_
* La Búsqueda personalizada almacena los datos de cada usuario en Bigtable. Cada usuario tiene un ID de usuario único y se le asigna una fila denominada por ese id de usuario. 
* Es un servicio que registra las consultas de los usuarios y los clics, como búsquedas web, imágenes y noticias. 
* Los usuarios pueden explorar sus historiales de búsqueda para revisar sus consultas y clics anteriores.



