# **Resumen: Enterprise data warehousing on aws**

## **Introduction**

Para que una empresa pueda crecer se debe: 

* Guardar todos los datos que son importantes para su empresa.

* Que pueda permitir acceder a estos datos e interpretarlos.

Antes era muy costoso crear un almacén de datos y además eran muy complicados de configurar. Estas arquitecturas tradicionales de almacenamiento de datos tienen plazos muy largos para adquirir y actualizar el hardware, muy costosas, no pueden separar los datos que se usan con mayor frecuencia de los que no, antidemocratización de datos. 

## **Introducing Amazon Redshift**

Fue lanzado en febrero del 2013, es uno de los servicios AWS que ha crecido más rápidamente. Algunas empresas importantes que migraron: 

* Johnson & Johnson

* McDonalds

* NTT DOCOMO

* FINRA, etc.

Las empresas cambiaron el pensamiento que tenían sobre los almacenes de datos gracias a aquellos almacenes en la nube como Amazon Redshift. 

El costo disminuyó enormemente y el complicado trabajo de implementar un almacenamiento normal. Un beneficio indiscutible es que no se compromete el rendimiento.

Este almacén en la nube tiene una escala de petabytes, es rápida, simple y rentable que se puede usar con BI existentes. Se pueden obtener motores de almacenamiento que realizan procesamiento paralelo masivo a una décima parte del costo. Con este almacén se puede crecer hasta tener una escala de exabytes.

## **Modern Analytics and Data Warehousing Architecture**

Para entender mejor por qué se utilizan los almacenes de datos y no solo se hacen consultas de análisis directamente en una base de datos, se debe comprender lo que es un almacén de datos y  una base de datos OLT.

* **Almacenes de datos:** Estos son aquellos que hacen operaciones de escritura por lotes y lectura de grandes volúmenes de datos. Usan esquemas desnormalizados como Star scheme y Snowflake scheme; esto porque se necesita un alto rendimiento de datos.

* **Bases de datos OLT:** Estas son aquellas que hacen operaciones de escritura continua y grandes volúmenes de pequeñas operaciones de lectura. Usan esquemas de normalización; esto porque necesitan un alto rendimiento de transacciones. 

**AWS Analytics Services:** Etapas de una tubería de canalización de análisis:

* **Recopilar datos:** Puede tener diferentes tipos de datos , AWS puede almacenarlos por cada tipo.

* **Almacenar los datos:** Se pueden almacenar en lake house, data warehouse o data mart.

* **Procesar los datos:** Se puede dar por lotes(OLAP), por tiempo real (OLTP).

* **Analiza y visualiza los datos:** Se pueden utilizar herramientas como MySQL Workbench, con Tableau o MicroStrategy.


## **Data Warehouse Technology Options**

Bases de datos orientadas a filas: Almacenan filas completas, la lectura rápida se logra por índices secundarios. Algunas bases de datos de este tipo son: Oracle Database Server, Microsoft SQL Server, MySQL, etc.

Para optimizar de estas bases de datos:

* Vistas materializadas.

* Tablas de resumen.

* Creación de índices en todas las combinaciones.

* Uniones basadas en índices.

Bases de datos orientadas a columnas: Organizan cada columna en su propio conjunto de bloques físicos. Son más eficientes (E/S) en consultas de solo lectura. Son mejores para el almacenamiento de datos. Algunas bases de datos de este tipo son: Amazon Redshift, Vertica, Greenplum, etc.

## **Amazon Redshift Deep Dive**

Ofrece consultas rápidas y rendimiento de entrada y salida para cualquier tamaño de datos mediante el uso de almacenamiento columnar. Paraleliza y distribuye consultas a través de múltiples nodos.

* **Integración con data lake:** Tiene una característica llamada Redshift Spectrum que facilita la consulta como la estructura de datos.

* **Performance:** Para que tenga un rendimiento rápido se tiene un hardware de alto rendimiento, Advanced Query Accelerator (memoria caché), almacenamiento eficiente, vistas materializadas, gestión automática de carga de trabajo y caché de resultados.

* **Durabilidad y Disponibilidad:** AR Detecta y reemplaza cualquier nodo fallido en su clúster de almacenamiento y hace que su nodo de reemplazo esté inmediatamente disponible. AR mantiene tres copias: La original, la réplica y una copia de seguridad.

* **Elasticidad y Escalabilidad:** Se puede escalar el almacenamiento y procesamiento de forma independiente. Una forma de elasticidad de Amazon Redshift es el cambio de tamaño elástico (cambia tamaño del clúster, agregando nodos). Otra forma de elasticidad es el escalado de concurrencia, se pueden admitir usuarios simultáneamente y consultas.

## **Operations**

### Ideal Usage Patterns

Es utilizado por las empresas para:

* Ejecutar informes.

* Analizar los datos de ventas globales para 
muchos productos.

* Almacenar datos históricos de operaciones bursátiles.

* Analizar impresiones de anuncios.

* Analizar tendencias sociales, etc.

### Anti-Patterns

No es ideal para:

* OLTP.

* Datos no estructurados: No se admite una estructura de esquema arbitraria, deben estar estructurados por un esquema definido.

* Datos BLOB: Si se planea almacenar archivos binarios de objetos grandes, almacenar en S3 y hacer referencia a la ubicación en Amazon Redshift.