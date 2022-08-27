**Bases de datos 2 - Quiz 1**
**26-08-2022**
**Pamela González López - 2019390545**

1) Explique en qué consisten los siguientes conceptos:
* Data Warehouse
* Data Lake
* Data Mart

**a) Data Warehouse:** Es un almacen de datos especializados para hacer consultas especializadas, se puede ejecutar análisis rápidos en grandes volúmenes de datos. Por ejemplo, en una empresa los usuarios consumen los datos mediante consultas SQL, con informes periódicos etc, esto con el fin de tomar decisiones comerciales.

**b) Data Lake:** Es un entorno de datos compartidos en su formato original que comprende múltiples repositorios. Permite consultar datos en su almacén de datos, lago de datos y bases de datos operativos para obtener información más rápida y profunda que de otro modo no sería posible.

**c) Data Mart:** El darta mart es un almacen que se centra en un tema específico o área determinada. Se puede tener data marts de un tema específico o especialidad en una empresa. Como un beneficio es que son fáciles de disear y administrar, como inconveniente es que las consultas pueden volverse complejas entre áreas especializadas.

2) ¿De qué forma se benefician las aplicaiones de uso de Columnar Storage? Explique.

Una aplicación se beneficia con el columnar storage pues cuando se tiene que analizar una gran cantidad de datos con este tipo de storage la cantidad de operaciones entrada y salidad resulta mucho menor, entonces se accede mucho menos al disco; esto se debe a la forma en que se distribuye y se acomoda la información. Al estar almacenados los datos de una categoría muy próximos entre sí, y si se quiere analizar un registro solo se necesita cargar un bloque y no leer toda una base de datos completa.

3) ¿En qué consiste streaming y batch processing?

**Streaming:** Es un flujo constante de datos. No se define un principio ni final de los datos, tampoco tiene una definición de tamaño en disco.

**Batch processing:** Información que vive en disco que se procesa en batches. Consiste en el procesamiento de un gran volumen de datos a la vez, completado simultaneamente, con orden secuencial y continuo. Es una forma eficiente de procesar grandes cantidades de datos que se van recopilando en cierto periodo de tiempo.

4) ¿En qué consiste datos estructurados, semi estructurados y no estructurados?

**Datos estructurados:** Son los datos que siguen un esquema fijo o tabular como la interpretación visual de una tabla de una base de datos relacional.

**Datos semi estructurados:** Son aquellos datos que no tienen los mismos atributos siempre, como los documentos JSON o XML.

**Datos no estructurados:** Estos datos son los que no siguen ningún patrón definido, como el texto. En base de datos estos datos se trabajan con herramientas NLP, las cuales permiten descubrir insights dentro de texto no estructurado.

#### Referencias bibliográficas

Amazon (2021). _Enterprise data warehousing on aws_. Recuperado el 26 de agosto del 2022, de https://docs.aws.amazon.com/iam/index.html?nc2=h_ql_doc_iam https://d1.awsstatic.com/whitepapers/enterprise-data-warehousing-on-aws.pdf

Shiff L.(2020). Real Time vs Batch Processing vs Stream Processing. bmcblogs. Recuperado de https://www.bmc.com/blogs/batch-processing-stream-processing-real-time/#:~:text=Batch%20processing%20is%20the%20processing,non%2Dstop%2C%20sequential%20order

Cuestiones técnicas.(2020). Base de datos columnar. Digital Guide IONOS. Recuperado de https://www.ionos.es/digitalguide/hosting/cuestiones-tecnicas/base-de-datos-columnar/
