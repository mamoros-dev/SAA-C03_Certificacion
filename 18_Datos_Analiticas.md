# Datos y Analítica en AWS

## Introducción
AWS ofrece una amplia gama de servicios para gestionar, almacenar y analizar datos a escala empresarial.

## Servicios Principales

### 1. Amazon S3 (Simple Storage Service)
- Almacenamiento de objetos escalable
- Ideal para data lakes
- Control de acceso y encriptación

### 2. Amazon Redshift
- Data warehouse para análisis
- Procesamiento de grandes volúmenes
- Escalable horizontalmente
- Redshift se basa en PostgreSQL, pero no se utiliza para OLTP
- Es OLAP - procesamiento analítico en línea (análisis y almacenamiento de datos)
- 10 veces mejor rendimiento que otros almacenes de datos, escala a PBs de datos
- Almacenamiento de datos en columnas (en lugar de filas) y motor de consulta paralelo
- Pago por uso en función de las instancias aprovisionadas
- Dispone de una interfaz SQL para realizar consultas
- Se integra con herramientas de BI como Amazon Quicksight o Tableau
- vs Athena: consultas / uniones / agregaciones más rápidas gracias a los índices

+ Clúster Redshift
    - Nodo líder: planifica las consultas y agrega los resultados.
    - Nodo de cálculo: realiza las consultas y envía los resultados al líder.
    - Se aprovisiona el tamaño del nodo por adelantado
    - Puedes utilizar instancias reservadas para ahorrar costes

+ Espectro Redshift: Consultar datos que ya están en S3 sin cargarlos.Debe haber un clúster Redshift disponible para iniciar la consulta.

### 3. Amazon Athena
- Consultas SQL directamente en S3
- Sin necesidad de infraestructura
- Pago por consulta
+ Servicio de consulta sin servidor para analizar datos almacenados en Amazon S3.
+ Utiliza el lenguaje SQL estándar para consultar los archivos
+ Admite CSV, JSON, ORC, Avro y Parquet
+ Precio: 5 dólares por TB de datos analizados
+ Se utiliza habitualmente con Amazon Quicksight para la elaboración de informes y Dashboards
+ Casos de uso: Inteligencia empresarial / análisis / informes, analizar y consultar registros de flujo de VPC, registros de ELB, CloudTrail trails, etc....
+ Sugerencia de examen: analizar datos en S3 utilizando SQL sin servidor, utilizar Athena

+ Mejora del rendimiento
+ Utiliza datos en columnas para ahorrar costes
    + Se recomienda Apache Parquet u ORC
    + Enorme mejora del rendimiento
    + Utiliza Glue para convertir tus datos en Parquet u ORC
+ Comprime los datos para recuperaciones más pequeñas (bzip2, gzip, lz4, snappy, zlip, zstd...)
+ Particionar conjuntos de datos en S3 para facilitar la consulta en columnas virtuales
+ Utiliza archivos de mayor tamaño (> 128 MB) para minimizar la sobrecarga

+ Consulta federada:
    + Permite ejecutar consultas SQL en datos almacenados en fuentes de datos relacionales, no relacionales, de objetos y personalizadas (AWS o en las instalaciones).
    + Utiliza conectores de fuentes de datos que se ejecutan en AWS Lambda para ejecutar consultas federadas (por ejemplo, CloudWatch Logs, DynamoDB, RDS, ...)
    + Almacena los resultados de nuevo en Amazon S3

### 4. AWS Glue
- ETL (Extract, Transform, Load)
- Catálogo de datos
- Integración con múltiples fuentes
- Servicio gestionado de extracción, transformación y carga (ETL)
- Útil para preparar y transformar datos para análisis
- Servicio totalmente sin servidor

+ Glue Job Bookmarks: evitar el reprocesamiento de datos antiguos
+ Vistas elásticas Glue (Glue Elastic Views):
    + Combina y replica datos a través de múltiples almacenes de datos utilizando SQL
    + Sin código personalizado, Glue supervisa los cambios en los datos de origen, sin servidor
    + Aprovecha una "tabla virtual" (vista materializada)
+ Glue DataBrew: limpia y normaliza los datos mediante transformaciones predefinidas
+ Glue Studio: nueva interfaz gráfica de usuario para crear, ejecutar y supervisar trabajos ETL en Glue
+ Glue Streaming ETL (basado en Apache Spark Structured Streaming): compatible con Kinesis Data Streaming, Kafka, MSK (managed Kafka)

### 5. Amazon EMR (Elastic MapReduce)
- Procesamiento de big data
- Hadoop y Spark
- Escalable bajo demanda
- EMR son las siglas de "Elastic MapReduce".
- EMR ayuda a crear clusters Hadoop (Big Data) para analizar y procesar grandes cantidades de datos
- Los clusters pueden estar formados por cientos de instancias EC2
- EMR viene con Apache Spark, HBase, Presto, Flink...
- EMR se encarga de todo el aprovisionamiento y configuración
- Auto-escalado e integrado con instancias Spot
- Casos de uso: procesamiento de datos, aprendizaje automático, indexación web, big data…

### 6. Amazon Kinesis
- Procesamiento de datos en tiempo real
- Kinesis Data Streams
- Kinesis Data Firehose
- Análisis en tiempo real en Kinesis Data Streams & Firehose mediante SQL
- Añadir datos de referencia de Amazon S3 para enriquecer los datos de streaming
- Totalmente administrado, sin servidores que aprovisionar
- Escalado automático
- Paga por la tasa de consumo real
- Salida:
    - Kinesis Data Streams: crea flujos a partir de las consultas analíticas en tiempo real
    - Kinesis Data Firehose: envío de resultados de consultas analíticas a destinos
- Casos de uso:
    - Análisis de series temporales
    - Dashboards en tiempo real
    - Métricas en tiempo real

+ Amazon Managed Streaming para Apache Kafka (Amazon MSK)
+ Alternativa a Amazon Kinesis
+ Apache Kafka totalmente administrado en AWS
    + Permite crear, actualizar y eliminar clústeres
    + MSK crea y administra nodos brokers de Kafka y nodos Zookeeper por ti
    + Implementa el clúster MSK en tu VPC, multi-AZ (hasta 3 para Alta Disponibilidad)
    + Recuperación automática de fallos comunes de Apache Kafka
    + Los datos se almacenan en volúmenes EBS durante todo el tiempo que desees
+ MSK sin servidor
    + Ejecuta Apache Kafka en MSK sin gestionar la capacidad
    + MSK aprovisiona automáticamente los recursos y escala la computación y el almacenamiento

### 7. Amazon QuickSight
- Herramienta de BI y visualización
- Dashboards interactivos
- ML insights
- Servicio de inteligencia empresarial basado en aprendizaje automático sin servidor
para crear Dashboards interactivos
- Rápido, escalable automáticamente, integrable y con precios por sesión
- Casos de uso:
    - Análisis empresarial
    - Creación de visualizaciones
    - Realizar análisis ad hoc
    - Obtener perspectivas de negocio utilizando datos
- Integrado con RDS, Aurora, Athena, Redshift, S3...
- Computación en memoria utilizando el motor SPICE si los datos se importan en QuickSight
- Edición Enterprise: Posibilidad de configurar la seguridad a nivel de columna (CLS)

### 8. AWS Lake Formation
- Crear data lakes seguros
- Gestión centralizada
- Control de acceso granular
- Lake Formation / Lago de datos = lugar central para tener todos los datos con fines analíticos
- Servicio totalmente gestionado que facilita la configuración de un lago de datos en cuestión de días
- Descubre, limpia, transforma e ingiere datos en el lago de datos
- Automatiza muchos pasos manuales complejos (recopilar, limpiar, mover, catalogar datos, ...) y de-duplicar (utilizando ML Transforms)
- Combina datos estructurados y no estructurados en el lago de datos
- Planos de origen listos para usar: S3, RDS, BD relacionales y NoSQL...
- Control de acceso detallado para las aplicaciones (a nivel de fila y columna)
- Construido sobre AWS Glue

### 9. Amazon OpenSearch
- Amazon OpenSearch es el sucesor de Amazon ElasticSearch
- En DynamoDB, las consultas sólo existen por clave primaria o índices...
- Con OpenSearch, puedes buscar en cualquier campo, incluso coincidencias parciales
- Es habitual utilizar OpenSearch como complemento de otra base de datos
- OpenSearch requiere un Cluster de instancias (no sin servidor)
- No soporta SQL (tiene su propio lenguaje de consulta)
- Ingestión desde Kinesis Data Firehose, AWS IoT y CloudWatch Logs
- Seguridad mediante Cognito & IAM, cifrado KMS, TLS
- Viene con OpenSearch Dashboards (visualización)

## Arquitectura Típica
```
Fuentes → Ingestión → Almacenamiento → Procesamiento → Análisis/BI
         (Kinesis)    (S3/Redshift)   (Glue/EMR)    (QuickSight)
```

## Mejores Prácticas
- Usar S3 como capa de almacenamiento central
- Implementar catalogación con Glue
- Aplicar seguridad en capas
- Optimizar costos con particionamiento
- Monitorear con CloudWatch

## PIPELINE INGESTIÓN DE BIG DATA

+ Queremos que el proceso de ingesta sea totalmente sin servidor.
+ Queremos recopilar datos en tiempo real
+ Queremos transformar los datos
+ Queremos consultar los datos transformados utilizando SQL
+ Los informes creados utilizando las consultas deben estar en S3
+ Queremos cargar esos datos en un almacén y crear Dashboards

+ Debate sobre el Pipeline de ingestión de Big Data
    + IoT Core permite recopilar datos de dispositivos IoT
    + Kinesis es ideal para la recopilación de datos en tiempo real
    + Firehose ayuda con la entrega de datos a S3 en tiempo casi real (1 minuto)
    + Lambda puede ayudar a Firehose con las transformaciones de datos
    + Amazon S3 puede activar notificaciones a SQS
    + Lambda puede suscribirse a SQS (podríamos tener un conector S3 a     Lambda)
    + Athena es un servicio SQL sin servidor y los resultados se almacenan en S3
    + El bucket de reportes contiene datos analizados y puede ser utilizado por herramientas de informes como AWS QuickSight, Redshift, etc...


## RESUMEN

+ Athena — consultas SQL sobre S3 sin mover datos
    - Una empresa tiene millones de logs en S3. En vez de cargarlos en una base de datos, Athena les permite hacer consultas SQL directamente sobre S3. Sin servidor, sin ETL, pagas por consulta.
    - Caso real: analizar logs de CloudFront, logs de ALB, datos de coste de AWS — todo guardado en S3 y consultado con SQL puntualmente.
> Palabra clave: "consultas SQL sobre S3", "análisis de logs", "sin mover datos", "serverless" → Athena.

+ Redshift — data warehouse para análisis masivo
    - Base de datos columnar para análisis de grandes volúmenes de datos históricos. No es para transacciones del día a día — es para analizar millones de registros y generar informes de negocio.
    - Caso real: una empresa carga todos sus datos de ventas de los últimos 5 años y los analistas hacen queries complejas para encontrar tendencias.
    - Diferencia clave con RDS: RDS es para transacciones operacionales (OLTP). Redshift es para análisis histórico masivo (OLAP).
> Palabra clave: "data warehouse", "análisis histórico", "OLAP", "Business Intelligence" → Redshift.

+ OpenSearch — búsqueda y análisis de logs en tiempo real
    -   Permite buscar dentro de grandes volúmenes de texto y logs. Es el motor detrás de muchos sistemas de búsqueda y monitorización.
    - Caso real: una empresa indexa todos sus logs de aplicación y puede buscar "todos los errores 500 de las últimas 2 horas" en tiempo real.
> Palabra clave: "búsqueda de texto", "análisis de logs en tiempo real", "Elasticsearch" → OpenSearch.

+ EMR — Hadoop y Spark gestionados
    - Para procesar cantidades enormes de datos con frameworks como Hadoop, Spark o Hive. AWS gestiona el cluster por ti.
    - Caso real: una empresa de medios procesa petabytes de datos de comportamiento de usuarios con Spark para entrenar modelos de recomendación.
> Palabra clave: "Hadoop", "Spark", "Big Data", "procesamiento distribuido", "ETL a gran escala" → EMR.

+ Glue — ETL serverless
    - Extrae datos de una fuente, los transforma y los carga en otro destino. Serverless y totalmente gestionado.
    - Caso real: datos de ventas en RDS + logs en S3 → Glue los combina, limpia y transforma → los carga en Redshift para análisis.
    - El Glue Data Catalog es importante — es un inventario centralizado de todos tus datos en AWS. Athena y EMR lo usan para saber qué datos existen y dónde.
> Palabra clave: "ETL", "transformar datos", "mover datos entre servicios", "serverless ETL" → Glue.

+ Lake Formation — crear un data lake rápido
    - Un data lake es un repositorio centralizado donde guardas todos tus datos en bruto. Lake Formation automatiza la creación y gestión de seguridad del data lake sobre S3.
    - Caso real: una empresa quiere centralizar datos de RDS, S3, DynamoDB y aplicaciones externas en un único lugar para análisis.
> Palabra clave: "data lake", "centralizar datos", "seguridad centralizada sobre S3" → Lake Formation.

+ QuickSight — dashboards y visualización BI
    - La herramienta de Business Intelligence de AWS. Conecta con Redshift, Athena, RDS y crea dashboards visuales para que los managers vean los datos.
    - Caso real: el equipo de negocio ve un dashboard con ventas por región, tendencias y KPIs actualizado en tiempo real.
> Palabra clave: "dashboards", "visualización", "Business Intelligence", "informes para negocio" → QuickSight.

+ MSK — Apache Kafka gestionado
    - Kafka es como Kinesis pero open source y más flexible. MSK es Kafka gestionado por AWS.
    - Cuándo usar MSK vs Kinesis:
        - Ya usas Kafka on-premise y quieres migrar → MSK
        - Empiezas desde cero en AWS → Kinesis
        - Necesitas protocolo Kafka estándar → MSK
> Palabra clave: "Apache Kafka", "migrar Kafka a AWS" → MSK.

+ La tabla mental para el examen (ESCENARIO -> SERVICO):
"SQL sobre S3 sin mover datos" -> Athena
"data warehouse, análisis histórico, OLAP" -> Redshift
"búsqueda de texto, logs en tiempo real" -> OpenSearch
"Hadoop, Spark, Big Data distribuido" -> EMR
"ETL serverless, transformar datos" -> Glue
"data lake, centralizar todos los datos" -> Lake Formation
"dashboards, BI, visualización" -> QuickSight
"Apache Kafka gestionado" -> MSK

+ El pipeline típico de Big Data en AWS — muy frecuente en el examen:
```
Fuentes de datos (RDS, apps, IoT)
         ↓
    Kinesis / MSK  (ingesta en tiempo real)
         ↓
      S3 (data lake raw)
         ↓
    Glue (ETL, transformación)
         ↓
  Redshift (data warehouse)
         ↓
   QuickSight (dashboards)
```
> Athena aparece como alternativa a Redshift cuando las consultas son puntuales y no necesitas un warehouse permanente.

## CUESTIONARIO

**Pregunta 1:**
Te gustaría disponer de una base de datos que sea eficiente a la hora de realizar consultas analíticas sobre grandes conjuntos de datos en columnas. Te gustaría conectarte a este almacén de datos mediante una herramienta de informes y dashboards como Amazon QuickSight. ¿Qué tecnología de AWS recomiendas?  
> "Amazon Redshift" porque es un almacén de datos optimizado para consultas analíticas de grandes conjuntos de datos en columnas, lo que permite un rendimiento eficiente en tus análisis. Además, se integra perfectamente con herramientas como Amazon QuickSight para la visualización de datos, cumpliendo con tus necesidades.

**Pregunta 2:**
Tienes muchos archivos de logs almacenados en un bucket de S3 y quieres realizar un análisis rápido, a ser posible sin servidor, para filtrar los logs y encontrar a los usuarios que han intentado realizar una acción no autorizada. ¿Qué servicio de AWS te permite hacerlo?
> "Amazon Athena" porque es un servicio que permite consultas SQL directamente sobre datos almacenados en Amazon S3, lo que lo hace ideal para analizar archivos de logs sin necesidad de aprovisionar servidores. Esto se alinea con tu objetivo de filtrar logs rápidamente para identificar usuarios con acciones no autorizadas de manera eficiente.

**Pregunta 3:**
Como Arquitecto de Soluciones, te han indicado que prepares un plan de recuperación de desastres para un cluster de Redshift. ¿Qué debes hacer?
> "Activa las Instantáneas Automatizadas, y luego configura tu cluster de Redshift para que copie automáticamente las instantáneas a otra región de AWS" porque esta acción garantiza que tus datos estén respaldados y sean recuperables en caso de un desastre, mejorando la resiliencia de tu infraestructura. Esto es crítico para un plan de recuperación de desastres, ya que asegura que puedas restaurar rápidamente tu clúster en otra región si es necesario.

**Pregunta 4:**
¿Qué función de Redshift obliga a que todo el tráfico de COPIA y DESCARGA que se mueve entre tu clúster y los repositorios de datos pase por tus VPC?
> "Enrutamiento VPC mejorado" porque esta función asegura que todo el tráfico de copia y descarga entre tu clúster de Redshift y los repositorios de datos pase de forma segura a través de tu VPC, mejorando la seguridad y el rendimiento del tráfico de datos. Esto es fundamental para gestionar eficazmente las comunicaciones dentro de la infraestructura de AWS y alinearse con las mejores prácticas de seguridad.

**Pregunta 5:**
Diriges un sitio web de juegos que utiliza DynamoDB como almacén de datos. Los usuarios han pedido una función de búsqueda para encontrar a otros jugadores por su nombre, con coincidencias parciales si es posible. ¿Qué tecnología de AWS recomiendas para implementar esta función?
> "Amazon OpenSearch" porque es una tecnología diseñada específicamente para realizar búsquedas eficientes y rápidas sobre grandes volúmenes de datos, lo que permite a los usuarios encontrar a otros jugadores por su nombre, incluyendo coincidencias parciales. Esta elección se alinea perfectamente con tu objetivo de mejorar la funcionalidad de búsqueda en tu sitio web de juegos.

**Pregunta 6:**
Un servicio de AWS te permite crear, ejecutar y monitorizar trabajos ETL (extracción, transformación y carga) con unos pocos clics
>  "AWS Glue" porque es un servicio diseñado específicamente para crear, ejecutar y monitorizar trabajos ETL de manera sencilla, permitiéndote mover datos entre diferentes fuentes y formatos con unos pocos clics. Esto se alinea con el objetivo de facilitar la integración y preparación de datos en tus aplicaciones.

**Pregunta 7:**
Una empresa utiliza AWS para alojar sus sitios web públicos y sus aplicaciones internas. Esos diferentes sitios web y aplicaciones generan muchos logs y rastros. Es necesario almacenar esos logs de forma centralizada y buscar y analizar eficazmente esos logs en tiempo real para detectar cualquier error y si existe una amenaza. ¿Qué servicio de AWS puede ayudarles a almacenar y analizar eficientemente los logs?
>  "Servicio Amazon OpenSearch" porque es ideal para almacenar y analizar logs de manera centralizada, permitiendo la búsqueda y análisis en tiempo real, lo que facilita la detección de errores y amenazas en tus aplicaciones. Esta elección se alinea perfectamente con la necesidad de tu empresa de gestionar eficazmente sus registros generados.

**Pregunta 8:**
............................. hace que sea fácil y rentable para los ingenieros y analistas de datos ejecutar aplicaciones construidas con marcos de big data de código abierto, como Apache Spark, Hive o Presto, sin tener que operar o administrar clusters.
> "Amazon EMR" porque este servicio permite a los ingenieros y analistas de datos ejecutar aplicaciones de big data de forma sencilla y económica, sin la necesidad de gestionar clústeres, lo que se alinea perfectamente con el objetivo de facilitar el manejo de grandes volúmenes de datos.

**Pregunta 9:**
Una empresa de comercio electrónico tiene todos sus datos históricos, como pedidos, clientes, ingresos y ventas de los años anteriores, alojados en un clúster de Redshift. Se necesita generar algunos dashboards e informes que indiquen los ingresos de los años anteriores y las ventas totales, para que sea fácil definir los requisitos para el año siguiente. Al equipo de DevOps se le asigna la tarea de encontrar un servicio de AWS que pueda ayudar a definir esos dashboards y que tenga una integración nativa con Redshift. ¿Qué servicio de AWS es el más adecuado?
>  "Amazon QuickSight" porque es una herramienta de visualización de datos que se integra de manera nativa con Amazon Redshift, permitiéndote crear dashboards e informes fácilmente para analizar los ingresos y ventas históricas de tu empresa. Esta elección es acertada y te permitirá tomar decisiones informadas para el futuro.

**Pregunta 10:**
¿Qué función de AWS Glue permite guardar y rastrear los datos que ya se han procesado durante una ejecución anterior de un trabajo ETL de Glue?
> "Glue Job Bookmarks (Marcadores de trabajo Glue)" porque esta función permite a AWS Glue rastrear y guardar el estado de los datos procesados en ejecuciones anteriores de trabajos ETL, lo que evita la re-procesación de los mismos; esto es esencial para optimizar el rendimiento y la eficiencia en tus flujos de trabajo de datos.

**Pregunta 11:**
Eres un ingeniero de DevOps en una empresa de Machine Learning que tiene 3 TB de archivos JSON almacenados en un bucket de S3. Hay un requisito para hacer algunos análisis en esos archivos utilizando Amazon Athena y te han encargado que encuentres una manera de convertir el formato de esos archivos de JSON a Apache Parquet. ¿Qué servicio de AWS es el más adecuado?
> "AWS Glue" porque es el servicio ideal para transformar y mover datos entre diferentes formatos, como convertir archivos JSON a Apache Parquet, facilitando así el análisis posterior con Amazon Athena. 

**Pregunta 12:**
Tienes una aplicación on-premise que se utiliza junto con un Apache Kafka on-premise para recibir un flujo de eventos de clickstream de varios sitios web. Te han encargado que migres esta aplicación lo antes posible, sin que haya cambios en el código. Has decidido alojar la aplicación en una instancia EC2. ¿Cuál es la mejor opción que recomiendas para migrar Apache Kafka?
>  "Amazon MSK" porque es un servicio completamente administrado que facilita la migración de aplicaciones que utilizan Apache Kafka, permitiendo que tu aplicación on-premise se ejecute en la nube sin necesidad de cambios en el código. Esto garantiza una transición suave y eficiente para manejar flujos de eventos como el de clickstream. 

**Pregunta 13:**
Tienes datos almacenados en RDS, buckets de S3 y estás utilizando AWS Lake Formation como lago de datos para recopilar, mover y catalogar los datos para poder hacer algunos análisis. Tienes muchos ingenieros de big data y ML en la empresa y quieres controlar el acceso a parte de los datos, ya que podrían contener información sensible. ¿Qué puedes utilizar?
> "Control de acceso de grano fino de Lake Formation" porque esta funcionalidad permite gestionar quién puede acceder a datos específicos en tu lago de datos, garantizando que solo las personas autorizadas puedan ver información sensible, lo cual es fundamental en un entorno con muchos ingenieros de big data y ML.

**Pregunta 14:**
¿Qué servicio de AWS es el más adecuado cuando quieres realizar análisis en tiempo real sobre flujos de datos?
> Kinesis Data Analytics (Análisis de datos de Amazon Kinesis)" porque este servicio permite realizar análisis en tiempo real sobre flujos de datos utilizando Kinesis Data Streams como fuente, lo que lo convierte en la opción más adecuada para tus necesidades de análisis en tiempo real.

## PREGUNTAS TIPO EXAMEN

**Pregunta 1:** Un equipo de datos quiere analizar logs de acceso almacenados en S3 usando SQL estándar sin cargarlos en ninguna base de datos. Solo hacen consultas ocasionales. ¿Qué servicio usan?  
A) Redshift  
B) RDS  
**C) Athena**  
D) EMR  
> C) Athena: "Consultas ocasionales" + "sin cargar en base de datos" + "SQL sobre S3" = Athena. El detalle importante: Athena cobra por cantidad de datos escaneados en cada consulta — por eso es ideal para consultas puntuales, no para análisis continuos masivos donde Redshift sería más eficiente.

**Pregunta 2:** Una empresa retail quiere analizar 5 años de historial de ventas para encontrar tendencias estacionales y generar informes complejos de negocio. Los datos son estáticos y el análisis es intensivo. ¿Qué servicio usan?  
A) DynamoDB  
B) RDS MySQL  
C) Athena  
**D) Redshift**  
> D) Redshift: "5 años de historial" + "análisis intensivo" + "tendencias" = Redshift. La palabra OLAP es la que distingue Redshift de RDS en el examen. RDS es OLTP — muchas transacciones pequeñas en tiempo real. Redshift es OLAP — pocas consultas enormes sobre datos históricos

**Pregunta 3:** Una empresa quiere crear dashboards visuales con KPIs de ventas conectados a su Redshift para que el equipo directivo los consulte. ¿Qué servicio usan?  
A) OpenSearch  
**B) QuickSight**  
C) Glue  
D) Lake Formation  
> B) QuickSight es la capa de visualización que se conecta a Redshift, Athena o RDS y convierte los datos en dashboards visuales para perfiles no técnicos. Cuando el examen mencione "equipo directivo" o "negocio" queriendo ver datos → QuickSight.

**Pregunta 4:** Una empresa tiene datos en RDS, S3 y DynamoDB que quiere combinar, limpiar y cargar en Redshift para análisis. ¿Qué servicio gestiona este proceso ETL sin servidores?  
A) EMR  
B) Lake Formation  
**C) Glue**  
D) Kinesis Firehose  
> C) Glue:  ETL serverless = Glue siempre. El matiz con EMR es que EMR requiere gestionar un cluster — es más potente pero más complejo. Glue es completamente serverless y gestionado, ideal para pipelines ETL estándar sin querer gestionar infraestructura.

**Pregunta 5:** Una empresa ya tiene un cluster Apache Kafka on-premise para streaming de eventos y quiere migrarlo a AWS manteniendo compatibilidad total con Kafka. ¿Qué servicio usan?  
A) Kinesis Data Streams  
B) SQS  
**C) MSK**  
D) Kinesis Firehose  
> C) MSK: "Apache Kafka" + "migrar a AWS" + "compatibilidad total" = MSK. Si el enunciado no mencionara Kafka y empezaran desde cero, Kinesis sería la respuesta. La palabra Kafka es el trigger que te lleva a MSK directamente.

**Pregunta 6:** Una empresa quiere centralizar datos de múltiples fuentes en S3 con control de acceso granular por departamento, creando un repositorio único para todos sus datos en bruto. ¿Qué servicio usan?  
A) Glue  
B) Redshift  
C) S3 con políticas de bucket  
**D) Lake Formation**  
> D) Lake Formation:  "Repositorio único" + "múltiples fuentes" + "control de acceso por departamento" = Lake Formation. S3 solo con políticas de bucket también podría parecer válido, pero Lake Formation añade la capa de gobierno de datos y seguridad centralizada que S3 solo no tiene