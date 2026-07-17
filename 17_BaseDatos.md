# Bases de datos en AWS

AWS ofrece varios servicios de bases de datos gestionadas, cada uno optimizado para distintos tipos de datos y patrones de uso.

+ Tipos de bases de datos
    - RDBMS (= SQL / OLTP): RDS, Aurora - ideal para uniones
    - Base de datos NoSQL - sin uniones, sin SQL: DynamoDB (~JSON), ElastiCache (pares
    clave / valor), Neptune (gráficos), DocumentDB (para MongoDB), Keyspaces (para Apache
    Cassandra)
    - Almacén de objetos: S3 (para objetos grandes) / Glacier (para copias de seguridad /
    archivos)
    - Almacén de datos - Data Warehouse (= SQL Analytics / BI): Redshift (OLAP), Athena, EMR
    - Búsquedas: OpenSearch (JSON) - texto libre, búsquedas no estructuradas
    - Gráficos: Amazon Neptune - muestra las relaciones entre los datos
    - Ledger: Base de datos de Amazon Quantum Ledger
    - Series temporales: Amazon Timestream

## Amazon RDS

Explicación:
- Servicio relacional gestionado para bases de datos SQL.
- Soporta MySQL, PostgreSQL, MariaDB, Oracle y SQL Server.

Características:
- Backups automáticos y restauración puntual.
- Replica en múltiples zonas (Multi-AZ).
- Escalado de cómputo y almacenamiento.
- Parches y mantenimiento gestionados.

Casos reales de uso:
- Aplicaciones web tradicionales que usan MySQL o PostgreSQL.
- Sistemas ERP/CRM que necesitan transacciones ACID.
- Pequeñas y medianas empresas que quieren evitar administrar servidores de bases de datos.
- Caso de uso: Almacenar conjuntos de datos relacionales (RDBMS / OLTP), realizar consultas SQL, transacciones.

## Amazon Aurora

Explicación:
- Base de datos relacional compatible con MySQL y PostgreSQL.
- Ofrece rendimiento mejorado y alta disponibilidad.

Características:
- Almacenamiento distribuido y auto-reparable.
- Réplicas de lectura con baja latencia.
- Failover rápido.
- Escala de hasta 15 réplicas de lectura.

Casos reales de uso:
- Aplicaciones con demanda variable que necesitan alta disponibilidad.
- Startups que buscan rendimiento similar al de bases de datos comerciales sin licencias costosas.
- Sistemas críticos con requisitos de recuperación rápida.
- Caso de uso: igual que RDS, pero con menos mantenimiento / más flexibilidad / más rendimiento / más funciones

## Amazon DynamoDB

Explicación:
- Base de datos NoSQL totalmente gestionada, basada en documento/clave-valor.
- Ideal para aplicaciones de baja latencia y alta escalabilidad.

Características:
- Escalado automático de capacidad.
- Rendimiento de milisegundos a una sola cifra.
- Soporta transacciones y Streams.
- Integración con AWS Lambda y otras funciones serverless.

Casos reales de uso:
- Juegos en tiempo real con millones de usuarios.
- Aplicaciones móviles y web que requieren lectura/escritura rápida.
- Gestión de sesiones y catálogos de productos.
- Caso de uso: desarrollo de aplicaciones sin servidor (documentos pequeños de 100 KB), caché distribuida sin servidor, no dispone de lenguaje de consulta SQL.

## Amazon DocumentDB

Explicación:
- Base de datos documental compatible con MongoDB.
- Gestionada y diseñada para cargas de trabajo de documentos JSON.

Características:
- Replicación automática de datos.
- Backups continuos hacia S3.
- Escala de lectura con réplicas.
- Escala automáticamente a cargas de trabajo con millones de peticiones por segundo

Casos reales de uso:
- Aplicaciones que ya usan MongoDB y buscan migrar a un servicio gestionado.
- Catálogos de productos flexibles y almacenamiento de documentos.
- Analítica de datos semiestructurados.

## Amazon Neptune

Explicación:
- Base de datos de grafos gestionada.
- Soporta modelos de grafos RDF y Property Graph.

Características:
- Consultas rápidas de relaciones complejas.
- Alta disponibilidad con réplicas de lectura.
- Integración con herramientas de aprendizaje automático y análisis de grafos.

Casos reales de uso:
- Redes sociales y recomendaciones.
- Gestión de fraude y detección de relaciones.
- Modelado de conocimiento y grafos semánticos.

## Amazon Redshift

Explicación:
- Almacén de datos en la nube para análisis y BI.
- Diseñado para consultas SQL de grandes volúmenes.

Características:
- Columnar storage y compresión de datos.
- Escalado de nodos para rendimiento.
- Integración con AWS Glue, QuickSight y S3.

Casos reales de uso:
- Informes empresariales y análisis de datos.
- Procesamiento de datos de eventos y logs.
- Data warehouses para marketing y finanzas.

## Amazon ElastiCache

Explicación:
- Servicio gestionado de caché en memoria.
- Soporta Redis y Memcached.

Características:
- Latencia ultrabaja para lectura/escritura.
- Escalado de clústeres en memoria.
- Replicación y persistencia opcionales (Redis).

Casos reales de uso:
- Almacenamiento en caché de sesiones de usuario.
- Mejora de rendimiento de aplicaciones web.
- Contadores en tiempo real y análisis de caché.
- Caso de uso: Almacén de claves/valores, lecturas frecuentes, menos escrituras, caché
de resultados para consultas a la base de datos, almacenamiento de datos de sesión
para sitios web, no puede utilizar SQL.

## Amazon Keyspaces

Explicación:
- Servicio gestionado compatible con Apache Cassandra.
- Base de datos NoSQL para grandes volúmenes y baja latencia.
- Sin servidor, escalable, de alta disponibilidad, totalmente administrado por AWS

Características:
- Escalado automático.
- Modelo de datos basado en columnas.
- Seguridad y cifrado gestionados.

Casos reales de uso:
- IoT y telemetría con grandes cantidades de eventos.
- Aplicaciones de mensajería y catálogos distribuidos.
- Sistemas que requieren escritura intensiva y alta disponibilidad.
- Casos de uso: almacenar información de dispositivos IoT, datos de series temporales.

## Amazon Timestream

Explicación:
- Base de datos de series temporales gestionada.
- Optimizada para métricas, eventos y datos de IoT.

Características:
- Almacenamiento jerárquico para datos recientes y de archivo.
- Consultas de series temporales y agregaciones.
- Compresión automática de datos.

Casos reales de uso:
- Monitoreo de dispositivos IoT.
- Datos de sensores y telemetría.
- Análisis de métricas operativas y de rendimiento.
- Casos de uso: Aplicaciones IoT, aplicaciones operativas, análisis en tiempo real

## Amazon S3

Explicación:
- S3 es un... almacén de claves / valores para objetos
- Genial para objetos grandes, no tan genial para muchos objetos pequeños
- Sin servidor, escala infinitamente, tamaño máximo de objeto 5 TB, capacidad de versionado
- Niveles: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + política de ciclo de vida

Características:
- Funciones: Versionado, Cifrado, Replicación, Eliminación de MFA, Registros de Acceso...
- Seguridad: IAM, políticas de bucket, ACL, puntos de acceso, Object Lambda, CORS, Object/Vault Lock
- Cifrado: SSE-S3, SSE-KMS, SSE-C, del lado del cliente, TLS en tránsito, cifrado por defecto
- Operaciones por lotes en objetos mediante S3 Batch, listado de archivos mediante S3 Inventory
- Rendimiento: Carga multiparte, aceleración de transferencias de S3, S3 Select
- Automatización: Notificaciones de eventos S3 (SNS, SQS, Lambda, EventBridge)

Casos reales de uso:
- Casos de uso: archivos estáticos, almacén de valores clave para archivos grandes, alojamiento de sitios web

## Amazon QLDB

Explicación:
- QLDB son las siglas de "Quantum Ledger Database".
- Un libro mayor es un libro que registra las transacciones financieras

Características:
- Totalmente gestionado, sin servidor, de alta disponibilidad, replicación a través de 3 AZ
- Se utiliza para revisar el historial de todos los cambios realizados en los datos de su aplicación a lo largo del tiempo
- Sistema inmutable: ninguna entrada puede ser eliminada o modificada, verificable criptográficamente
- Rendimiento 2-3 veces mejor que los marcos de blockchain de libro mayor común
- Diferencia con Amazon Managed Blockchain: no hay componente de descentralización, de acuerdo con las normas de regulación financiera

Casos reales de uso:
- las transacciones financieras

## CUESTIONARIO

**Pregunta 1:** ¿Qué base de datos te ayuda a almacenar conjuntos de datos relacionales, con compatibilidad con el lenguaje SQL y la capacidad de procesar transacciones como la inserción, la actualización y la eliminación?  
>  "Amazon RDS" porque es una base de datos relacional que admite SQL y permite el procesamiento de transacciones, como insertar, actualizar y eliminar datos. Esto se alinea con el objetivo de aprender sobre bases de datos que gestionan conjuntos de datos relacionales eficazmente. 

**Pregunta 2:** ¿Qué servicio de AWS te proporciona una capacidad de almacenamiento en caché compatible con la API de Redis?  
> Amazon ElastiCache es la opción correcta porque proporciona un almacenamiento en caché en memoria que es totalmente gestionado y compatible con la API de Redis, lo que lo hace ideal para mejorar el rendimiento de aplicaciones al almacenar datos frecuentemente utilizados de forma rápida.

**Pregunta 3:** Quieres migrar una base de datos MongoDB NoSQL on-premise a AWS. No quieres gestionar ningún servidor de base de datos, así que quieres utilizar una base de datos NoSQL gestionada, preferiblemente sin servidor, que te proporcione alta disponibilidad, durabilidad y fiabilidad, y la capacidad de llevar tu base de datos a nivel global. ¿Qué base de datos debes elegir?  
>  "Amazon DynamoDB" porque es una base de datos NoSQL completamente gestionada que no requiere que gestiones servidores, y proporciona alta disponibilidad, durabilidad y escalabilidad global, ideal para tu migración de MongoDB. 

**Pregunta 4:** Quieres realizar un Procesamiento de Transacciones en Línea (OLTP). Te gustaría utilizar una base de datos con capacidad de autoescalado integrada y que te proporcione el máximo número de réplicas para su almacenamiento subyacente. ¿Qué servicio de AWS recomiendas?  
> "Amazon Aurora" porque es una base de datos relacional que se autoescala de manera eficiente y puede manejar hasta 15 réplicas, lo que la hace ideal para el procesamiento de transacciones en línea (OLTP). Su capacidad de almacenamiento distribuido y su alta disponibilidad garantizan un rendimiento sólido y recuperación ante fallos, alineándose perfectamente con tus necesidades.

**Pregunta 5:** Como arquitecto de soluciones, una empresa emergente te ha pedido ayuda, ya que están trabajando en una arquitectura para un sitio web de redes sociales en el que los usuarios pueden ser amigos entre sí, y darles "me gusta" a las publicaciones de los demás. La empresa tiene previsto realizar algunas consultas complicadas, como "¿Cuál es el número de "me gusta" de las publicaciones que han publicado los amigos de Mike?". ¿Qué base de datos recomiendas?  
>  "Amazon Neptune" porque es un servicio de base de datos gráfica totalmente gestionado que se adapta perfectamente a aplicaciones con datos altamente conectados, como la tarea de calcular "me gusta" de publicaciones de amigos. Esto lo convierte en la opción ideal para cumplir con las consultas complejas de tu sitio web de redes sociales.

**Pregunta 6:** Tienes un conjunto de archivos, de 100 MB cada uno, que quieres almacenar en un almacén de valores clave fiable y duradero. ¿Qué servicio de AWS recomiendas?  
> "Amazon S3" porque es un servicio de almacenamiento diseñado específicamente para guardar objetos grandes, y funciona como un almacén de valores clave donde la clave es la ruta completa del objeto. Esto lo hace ideal para almacenar tus archivos de 100 MB de manera fiable y duradera.

**Pregunta 7:** Una empresa tiene un sitio web on-premise que utiliza ReactJS como frontend, NodeJS como backend y MongoDB como base de datos. Hay algunos problemas con la base de datos MongoDB autoalojada, ya que requiere mucho mantenimiento y no tienen ni pueden permitirse los recursos o la experiencia para manejar esos problemas. Así que se tomó la decisión de migrar el sitio web a AWS. Han decidido alojar la aplicación ReactJS del frontend en un bucket S3 y el backend NodeJS en un conjunto de instancias EC2. ¿Qué servicio de AWS pueden utilizar para migrar la base de datos MongoDB que les proporciona alta escalabilidad y disponibilidad sin hacer ningún cambio en el código?  
> "Amazon DocumentDB" porque es un servicio de base de datos totalmente gestionado que es compatible con las API de MongoDB, lo que permite migrar tu base de datos de MongoDB sin cambios en el código. Además, ofrece alta escalabilidad y disponibilidad, lo que resuelve los problemas de mantenimiento que enfrentabas con tu implementación autoalojada.

**Pregunta 8:** Una empresa que utiliza una base de datos Apache Cassandra autoalojada en sus instalaciones y que quiere migrar a AWS. ¿Qué servicio de AWS pueden utilizar que les proporcione una base de datos Apache Cassandra totalmente gestionada, altamente disponible y escalable?  
> "Amazon Keyspaces" porque es un servicio de base de datos completamente gestionado que es compatible con Apache Cassandra, lo que facilita la migración de tu base de datos existente sin necesidad de ajustar el código. Además, proporciona alta disponibilidad y escalabilidad, lo que es ideal para tus necesidades comerciales. 

**Pregunta 9:** Una empresa de pagos online utiliza AWS para alojar su infraestructura. Debido a la naturaleza de la aplicación, tienen un requisito estricto de almacenar un registro preciso de las transacciones financieras, como las de crédito y débito. Dichas transacciones deben almacenarse en un almacenamiento seguro, inmutable y cifrado que pueda ser verificado criptográficamente. ¿Qué servicio de AWS es el más adecuado para este caso de uso?  
>  "Amazon QLDB" porque es un servicio diseñado específicamente para almacenar un registro inmutable y verificable de transacciones, con capacidades de cifrado y seguridad que satisfacen tus requisitos para datos financieros. Es ideal para aplicaciones que necesitan un historial preciso y seguro de transacciones, lo que lo convierte en la mejor opción para tu caso de uso.

**Pregunta 10:** Una startup está trabajando en el desarrollo de un nuevo proyecto para reducir los incendios forestales debidos al cambio climático. La startup está desarrollando sensores que se extenderán por todo el bosque para realizar algunas lecturas como la temperatura, la humedad y las presiones que ayudarán a detectar los incendios forestales antes de que se produzcan. Van a tener miles de sensores que van a almacenar muchas lecturas cada segundo. Es necesario almacenar esas lecturas y hacer un análisis rápido para poder predecir si hay un incendio. ¿Qué servicio de AWS pueden utilizar para almacenar esas lecturas?  
> "Amazon Timestream" porque es un servicio de base de datos diseñado específicamente para trabajar con datos de series de tiempo, como las lecturas de temperatura y humedad de tus sensores. Su capacidad para manejar grandes volúmenes de datos y realizar análisis rápidos lo convierte en la opción ideal para predecir incendios forestales.

