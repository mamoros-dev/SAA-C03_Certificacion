# AWS Snow Family: introducción y casos de uso real

AWS Snow Family es una colección de dispositivos y servicios de AWS diseñados para mover grandes volúmenes de datos y ejecutar cargas de trabajo en entornos con conectividad limitada, alta latencia o donde la transferencia por red no es práctica.

## ¿Qué es AWS Snow?

AWS Snow permite transportar datos de forma física a los centros de datos de AWS cuando:

- El volumen de datos es demasiado grande para enviarlo por internet.
- La red es lenta o inestable.
- Se necesita una migración masiva con tiempos de transferencia reducidos.
- Se requiere procesamiento en edge locations o sitios remotos.

## Componentes principales de AWS Snow Family

### Snowcone
- Dispositivo pequeño y portátil.
- Ideal para migraciones de datos pequeñas o medianas.
- Útil en ubicaciones con poco espacio o conectividad limitada.
- Dispositivo utilizado para computación de borde, almacenamiento y transferencia de datos
- Snowcone - 8 TB de almacenamiento HDD
- Snowcone SSD - 14 TB de almacenamiento SSD
- Utiliza Snowcone donde no quepa Snowball (entorno con limitaciones de espacio)
- Debes proporcionar tu propia batería / cables
- Se puede enviar a AWS sin conexión, o conectarlo a internet y utilizar AWS DataSync para enviar los datos

### Snowball Edge
- Dispositivo más potente para transferencias de datos y procesamiento local.
- Se usa para migraciones masivas, recopilación de datos y computación en edge.
- Alternativa a mover datos a través de la red (y pagar tarifas de red)
- Disponible en varias capacidades según el caso de uso.
    + Almacenamiento optimizado Snowball Edge: 80 TB de capacidad de HDD
    + Computación optimizada de Snowball Edge: 42 TB de capacidad HDD o 28 TB de capacidad NVMe

### Snowmobile
- Plataforma de transporte física de grandes volúmenes de datos.
- Adecuada para exabytes de información.
- Muy útil para organizaciones con infraestructura extremadamente grande.
- Alta seguridad: temperatura controlada, GPS, videovigilancia 24/7
- Mejor que la Snowball si transfieres más de 10 PB

## Casos de uso reales

### 1. Migración masiva de datos
Una empresa con cientos de terabytes o petabytes de datos en sus servidores locales puede usar Snowball Edge o Snowmobile para mover esa información a AWS sin depender de una conexión de red muy rápida.

### 2. Procesamiento en edge
En industrias como petróleo y gas, manufactura o minería, las operaciones ocurren en sitios remotos. Snow permite capturar, procesar y analizar datos localmente, incluso cuando no hay conectividad estable.

### 3. Recuperación ante desastres y backup
Algunas organizaciones usan Snow para transferir copias de seguridad o grandes conjuntos de datos a AWS en escenarios donde la red no es suficiente o el tiempo de recuperación debe ser rápido.

### 4. Medios y entretenimiento
Productoras de video, estudios de cine y empresas de broadcasting pueden usar Snow para mover archivos multimedia pesados como videos 4K, contenido de alta resolución y bibliotecas completas de producción.

### 5. Gobierno y sectores regulados
En entornos donde la seguridad, la privacidad o la infraestructura física son críticas, Snow ofrece una opción de transferencia controlada y auditable.

## Ventajas de AWS Snow

- Reduce la dependencia de redes lentas o saturadas.
- Permite mover grandes volúmenes de datos de forma práctica.
- Soporta procesamiento local en lugares remotos.
- Ayuda a cumplir requisitos de cumplimiento y seguridad.

## Cuándo usar AWS Snow

AWS Snow es una buena opción cuando:

- Necesitas migrar datos en grandes cantidades.
- Los tiempos de transferencia por red son ineficientes.
- Tienes infraestructura en ubicaciones remotas.
- Requieres una solución física y controlada para mover información.

+ AWS Snow Family es ideal para escenarios donde la transferencia de datos por red no es viable. Su uso real se observa en migraciones masivas, edge computing, backup, medios digitales y entornos regulados.

+ Proceso de uso: 
    1. Solicita la entrega de dispositivos Snowball desde la consola de AWS
    2. Instala el cliente Snowball / AWS OpsHub en tus servidores
    3. Conecta el Snowball a tus servidores y copia los archivos utilizando el cliente
    4. Devuelve el dispositivo cuando hayas terminado (va a la instalación de AWS adecuada)
    5. Los datos se cargarán en un bucket de S3
    6. La Snowball se borrará por completo

## AMAZON FSX
AWS FSx es un servicio totalmente gestionado para ejecutar sistemas de archivos de alto rendimiento en AWS. Permite lanzar soluciones compatibles con aplicaciones empresariales y cargas de trabajo que necesitan almacenamiento de archivos compartido.

### Uso
- Proveer almacenamiento de archivos compartido para aplicaciones que requieren acceso concurrido.
- Modernizar aplicaciones existentes sin reescribir el acceso a archivos.
- Ejecutar cargas de trabajo de alto rendimiento, análisis de datos y aplicaciones Windows.
- Integrarse con Amazon EC2, contenedores y servicios de AWS que usan sistemas de archivos POSIX o SMB.

### Tipos y características
- FSx para Lustre
    - Sistema de archivos de alto rendimiento optimizado para cargas de trabajo HPC, análisis y procesamiento de big data.
    - Escala IOPS y ancho de banda con la capacidad de almacenamiento.
    - Integración nativa con S3 para cargas de trabajo de datos temporales y procesamientos rápidos.
    - Ideal para entornos de procesamiento por lotes y pipelines de datos.
- FSx para Windows File Server
    - Sistema de archivos administrado compatible con SMB y Active Directory.
    - Soporta permisos de Windows, instantáneas y caché local para rendimiento de baja latencia.
    - Usado por aplicaciones .NET, sistemas ERP, entornos de escritorio remoto y compartición de archivos corporativos.
- FSx para NetApp ONTAP
    - Sistema de archivos empresarial con compatibilidad NFS, SMB, iSCSI y capacidades de snapshot, replicación y deduplicación.
    - Permite movilidad de datos entre on-premise y AWS y uso híbrido con NetApp.
    - Adecuado para bases de datos, desarrollo de aplicaciones y entornos de virtualización.
- FSx para OpenZFS
    - Sistema de archivos basado en ZFS con soporte para compresión, instantáneas y clones.
    - Optimizado para cargas de trabajo que necesitan almacenamiento de archivos compartido con protección de datos avanzada.
    - Bueno para desarrollo de software, análisis de datos y aplicaciones que usan NFS.

### Casos reales de uso
- Entorno de renderizado y medios: usar FSx para Lustre para procesar y renderizar video 4K/8K rápidamente y guardar resultados en S3.
- Aplicaciones empresariales Windows: usar FSx para Windows File Server para compartir documentos, perfiles de usuarios y datos de aplicaciones con integración AD.
- Modernización de aplicaciones SAP o Oracle: usar FSx para NetApp ONTAP para ofrecer almacenamiento compartido corporativo con snapshot y replicación.
- Desarrollo de software y CI/CD: usar FSx para OpenZFS como almacenamiento de builds y pruebas con snapshots rápidas y clones de entornos.
- Entornos híbridos: usar FSx para NetApp ONTAP para replicar datos entre el centro de datos local y AWS y mantener continuidad operativa.

## AWS Storage Gateway
+ Puente entre los datos locales y los del Cloud
+ Casos de uso:
    + recuperación de desastres
    + copia de seguridad y restauración
    + almacenamiento por niveles
    + caché local y acceso a archivos de baja latencia
+ Tipos de Gateway de almacenamiento:
    + S3 File Gateway
    + Gateway de archivos FSx
    + Gateway de volumen
    + Tape Gateway

## Familia de transferencia AWS
+ Un servicio totalmente gestionado para la transferencia de archivos hacia y desde Amazon
S3 o Amazon EFS mediante el protocolo FTP
+ Protocolos soportados
    + AWS Transfer para FTP (Protocolo de Transferencia de Archivos (FTP))
    + AWS Transfer para FTPS (Protocolo de Transferencia de Archivos sobre SSL (FTPS))
    + AWS Transfer para SFTP (Protocolo de Transferencia de Archivos Seguro (SFTP))
+ Infraestructura administrada, escalable, fiable, altamente disponible (multi-AZ)
+ Paga por endpoint aprovisionado por hora + transferencias de datos en GB
+ Almacena y gestiona las credenciales de los usuarios dentro del servicio
+ Se integra con los sistemas de autenticación existentes (Microsoft Active Directory, LDAP,
Okta, Amazon Cognito, personalizado)
+ Uso: compartir archivos, conjuntos de datos públicos, CRM, ERP, ...

## AWS DataSync
+ Mover grandes cantidades de datos hacia y desde
    + En las instalaciones / otra nube a AWS (NFS, SMB, HDFS, API S3...) - necesita agente
    + De AWS a AWS (diferentes servicios de almacenamiento) - no necesita agente
+ Puedes sincronizar a:
    + Amazon S3 (cualquier clase de almacenamiento - incluido Glacier)
    + Amazon EFS
    + Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
+ Las tareas de replicación se pueden programar cada hora, cada día, cada semana
+ Se conservan los permisos y metadatos de los archivos (NFS POSIX, SMB...)
+ Una tarea de agente puede utilizar 10 Gbps, se puede configurar un límite de ancho de banda

## Comparación de almacenamiento
+ S3: Almacenamiento de objetos
+ S3 Glacier: Archivo de objetos
+ Volúmenes EBS: Almacenamiento en red para una instancia EC2 a la vez
+ Almacenamiento de instancia: Almacenamiento físico para tu instancia EC2 (altas IOPS)
+ EFS: Sistema de archivos de red para instancias Linux, sistema de archivos POSIX
+ FSx para Windows: Sistema de archivos de red para servidores Windows
+ FSx para Lustre: Sistema de archivos Linux de computación de alto rendimiento
+ FSx para NetApp ONTAP: Alta compatibilidad con sistemas operativos
+ FSx para OpenZFS: Sistema de ficheros ZFS gestionado
+ Storage Gateway: S3 & FSx File Gateway, Volume Gateway (caché y almacenado), Tape Gateway
+ Familia de transferencia: Interfaz FTP, FTPS, SFTP sobre Amazon S3 o Amazon EFS
+ Sincronización de datos: Programa la sincronización de datos desde las instalaciones a AWS, o de AWS a AWS
+ Snowcone / Snowball / Snowmobile: para mover grandes cantidades de datos a el Cloud, físicamente
+ Base de datos: para cargas de trabajo específicas, normalmente con indexación y consulta


## RESUMEN

+ Si tiene muchos TB o PB de datos y la conexión es lenta o inexistente → Snow Family.

+ Regla simple: "migrar PB de datos" o "sin conexión a internet" o "ubicación remota" → Snow Family.

+ Regla: si ves "Windows" + compartido → FSx for Windows. Si ves "HPC" o "ML" + alto rendimiento (HPC) → FSx for Lustre. Si ves "Linux" + compartido sin requisitos especiales → EFS.

+ Storage Gateway — cuándo usarlo: Conecta tu infraestructura on-premise con AWS. La empresa tiene servidores físicos propios y quiere usar AWS como extensión.

+ DataSync: Mover datos grandes entre on-premise y AWS o entre servicios AWS. "migración de datos", "sincronización", "automatizado".

+ Transfer Family: Transferir archivos via protocolos estándar (SFTP, FTP). "SFTP", "FTP", "socios comerciales", "transferencia de archivos"

## CUESTIONARIO

+ **Pregunta 1:**  
Necesitas mover cientos de Terabytes a Amazon S3, y luego procesar los datos utilizando una flota de instancias EC2. Tienes una banda ancha de 1 Gbit/s. Te gustaría mover los datos más rápido y posiblemente procesarlos mientras están en tránsito. ¿Qué recomiendas?
> "Utilizar Snowball Edge" porque este servicio no solo facilita la transferencia de grandes volúmenes de datos a Amazon S3, sino que también permite realizar un preprocesamiento en el dispositivo durante la migración, maximizando así la eficiencia del movimiento y el procesamiento de datos.

+ **Pregunta 2:**  
Quieres exponer un almacenamiento prácticamente infinito para tus copias de seguridad en cinta. Quieres mantener el mismo software que estás utilizando y quieres una interfaz compatible con iSCSI. ¿Qué utilizas?
>  "AWS Storage Gateway - Tape Gateway" porque este servicio permite mantener el software existente para copias de seguridad en cinta, proporcionando una interfaz compatible con iSCSI y acceso a almacenamiento prácticamente infinito en la nube, ideal para tus necesidades de respaldo.

+ **Pregunta 3:**  
Tus servidores EC2 Windows necesitan compartir algunos datos teniendo montado un Sistema de Archivos de Red que respete los mecanismos de seguridad de Windows y tenga integración con Microsoft Active Directory. ¿Qué recomiendas?
> "Amazon FSx para Windows (Servidor de archivos)" porque este servicio permite crear un sistema de archivos que es totalmente compatible con los mecanismos de seguridad de Windows y se integra perfectamente con Microsoft Active Directory, cumpliendo con tus necesidades de compartir datos en servidores EC2 Windows. Esto asegura que puedas manejar tu entorno de manera eficiente y segura.

+ **Pregunta 4:**  
Tienes cientos de Terabytes que quieres migrar a AWS S3 lo antes posible. Has intentado utilizar el ancho de banda de tu red y tardarás unas 3 semanas en completar el proceso de carga. ¿Cuál es el enfoque recomendado en esta situación?
> "AWS Snowball Edge" porque es ideal para transferir grandes volúmenes de datos rápidamente, ya que permite la migración física de datos utilizando dispositivos de almacenamiento a través de envío, evitando las limitaciones de ancho de banda de la red y acelerando el proceso de carga en S3.

+ **Pregunta 5:**  
Tienes un gran conjunto de datos almacenado en S3 al que quieres acceder desde los servidores locales utilizando el protocolo NFS o SMB. Además, quieres autenticar el acceso a estos archivos a través de Microsoft AD local. ¿Qué utilizarías?
> AWS Storage Gateway - Gateway de archivos S3" porque este servicio permite acceder a los datos almacenados en S3 a través de los protocolos NFS o SMB y autenticar el acceso mediante Microsoft Active Directory, cumpliendo así con tus necesidades específicas de conectividad y seguridad.

+ **Pregunta 6:**  
Estás planeando migrar la infraestructura de tu empresa de las instalaciones a la Cloud de AWS. Tienes un Microsoft Windows File Server on-premise que quieres migrar. ¿Cuál es el servicio de AWS más adecuado que puedes utilizar?
> "Amazon FSx para Windows (Servidor de archivos)" porque este servicio está diseñado específicamente para migrar y gestionar aplicaciones basadas en Windows, ofreciendo una solución de sistema de archivos completamente compatible con el entorno de Microsoft, lo que facilita la transición desde tu servidor de archivos local.

+ **Pregunta 7:**  
Te gustaría tener un sistema de archivos distribuido compatible con POSIX que te permita maximizar las IOPS para realizar algunas investigaciones informáticas de alto rendimiento (HPC) y genómicas. Este sistema de archivos tiene que escalar fácilmente a millones de IOPS. ¿Qué recomiendas?
> "Amazon FSx para Lustre" porque este servicio ofrece un sistema de archivos distribuido compatible con POSIX, diseñado específicamente para soportar cargas de trabajo de alto rendimiento como HPC y genómica, maximizando las IOPS y escalando fácilmente a millones de ellas.

+ **Pregunta 8:**  
¿Qué opción de despliegue del sistema de archivos FSx te proporciona un almacenamiento a largo plazo que se replica dentro de AZ?
>  "Sistema de archivos persistente" porque este tipo de despliegue ofrece almacenamiento a largo plazo con replicación de datos dentro de la misma zona de disponibilidad (AZ), lo que garantiza la durabilidad y disponibilidad de tus datos, además de la rápida sustitución de archivos en caso de fallos. Esto es crucial para mantener la integridad y el acceso constante a la información almacenada.

+ **Pregunta 9:**  
¿Cuál de los siguientes protocolos NO está soportado por AWS Transfer Family?
> "Seguridad de la capa de transporte (TLS)" como la opción correcta porque, aunque TLS es un protocolo de seguridad importante, AWS Transfer Family no lo admite directamente; en cambio, utiliza FTP, que carece de la seguridad inherente de TLS.

+ **Pregunta 10:** 
Una empresa utiliza muchos archivos y datos que se almacenan en un almacenamiento FSx para Windows File Server en AWS. Esos archivos son utilizados actualmente por los recursos alojados en AWS. Es necesario que se pueda acceder a esos archivos en las instalaciones con una baja latencia. ¿Qué servicio de AWS puede ayudarte a conseguirlo?
> "FSx File Gateway" porque este servicio permite el acceso a los archivos de FSx para Windows desde las instalaciones locales con baja latencia, facilitando la integración entre el almacenamiento en la nube y los recursos locales. Es una gran elección para asegurar un rendimiento eficiente en la transferencia de datos.

+ **Pregunta 11:**  
Un arquitecto de soluciones está trabajando en la planificación de la migración de una empresa emergente de las instalaciones a AWS. Actualmente, su infraestructura consiste en muchos servidores y 30 TB de datos alojados en un almacenamiento NFS compartido. Ha decidido utilizar Amazon S3 para alojar los datos. ¿Qué servicio de AWS puede migrar eficazmente los datos de las instalaciones a S3?
> "AWS DataSync" porque es el servicio diseñado específicamente para facilitar la migración de grandes volúmenes de datos desde las instalaciones a Amazon S3, permitiendo una transferencia eficiente y rápida de tus 30 TB de datos de almacenamiento NFS. 

+ **Pregunta 12:**  
¿Qué servicio de AWS es el más adecuado para migrar una gran cantidad de datos de un bucket de S3 a un sistema de archivos EFS?
> "AWS DataSync" como la opción correcta porque este servicio está diseñado específicamente para mover grandes volúmenes de datos de manera eficiente entre Amazon S3 y sistemas de archivos, asegurando una migración rápida y segura de tus datos.

+ **Pregunta 13:**  
Una empresa de Machine Learning está trabajando en un conjunto de conjuntos de datos que están alojados en buckets S3. La empresa ha decidido hacer públicos esos conjuntos de datos para que sean útiles para otros en su investigación, pero no quiere configurar el bucket S3 para que sea público. Y esos conjuntos de datos deben ser expuestos a través del protocolo FTP. ¿Qué pueden hacer para cumplir el requisito de forma eficiente y con el menor esfuerzo?
> "Utilizar la familia de transferencia de AWS" porque este servicio permite transferir y exponer datos de Amazon S3 a través de protocolos como FTP sin necesidad de hacer el bucket público, cumpliendo así con los requisitos de seguridad y eficiencia de la empresa en la gestión de sus datos.

+ **Pregunta 14:**  
Amazon FSx para NetApp ONTAP es compatible con los siguientes protocolos, EXCEPTO
> "FTP" como la respuesta correcta porque Amazon FSx para NetApp ONTAP no es compatible con este protocolo. Esta pregunta te ayuda a comprender mejor las capacidades de los diferentes servicios de AWS y su interoperabilidad.

+ **Pregunta 15:**  
¿Qué servicio de AWS es el más adecuado cuando se migra de un sistema de archivos ZFS on-premise a AWS?
> "Amazon FSx para OpenZFS" porque este servicio está específicamente diseñado para facilitar la migración de sistemas de archivos ZFS locales a AWS, ofreciendo compatibilidad y optimización para este tipo de archivos.

+ **Pregunta 16:**  
Una empresa está ejecutando Amazon S3 File Gateway para alojar sus datos en buckets S3 y puede montarlos en sus instalaciones mediante SMB. Actualmente, los datos están alojados en la clase de almacenamiento S3 Standard, y tienen la necesidad de reducir los costes de S3. Por ello, han decidido migrar algunos de esos datos a S3 Glacier. ¿Cuál es la forma más eficiente que pueden utilizar para mover los datos a S3 Glacier automáticamente?
> "Utilizar la política del ciclo de vida de S3" porque esta opción permite automatizar la migración de datos de S3 a S3 Glacier según reglas definidas, lo que reduce costos de manera eficiente y sin intervención manual constante. Es la forma más adecuada de gestionar el ciclo de vida de los objetos en Amazon S3. 

## PREGUNTAS TIPO EXAMEN

+ **Pregunta 1**: Una empresa tiene 5 Petabytes de datos históricos en sus servidores físicos y quiere migrarlos a S3. Su conexión a internet es de 1Gbps y calcular la transferencia llevaría años. ¿Qué solución usan?  
A) DataSync  
B) Direct Connect  
**C) AWS Snowball Edge**  
D) Storage Gateway  
>  C) AWS Snowball Edge: "5 Petabytes" + "transferencia llevaría años" = Snow Family siempre. El cálculo que hace el examen es simple: si transferir por internet tarda más de una semana, Snow es más rápido y barato.

+ **Pregunta 2**: Una empresa tiene servidores Windows on-premise con Active Directory y necesita un sistema de archivos compartido compatible con SMB en AWS. ¿Qué servicio usan?  
A) EFS  
B) FSx for Lustre  
**C) FSx for Windows**  
D) S3  
> C) FSx for Windows: "Windows" + "Active Directory" + "SMB" son las tres palabras que gritan FSx for Windows. EFS no es compatible con SMB ni con Active Directory — es solo para Linux con NFS.

+ **Pregunta 3**: Un laboratorio de investigación necesita procesar datasets científicos enormes para simulaciones de alto rendimiento (HPC). Necesitan un sistema de archivos de altísima velocidad compatible con Linux. ¿Qué usan?  
A) EFS  
B) FSx for Windows  
C) S3  
**D) FSx for Lustre**  
> D) FSx for Lustre: FSx for Lustre está diseñado específicamente para cargas de trabajo de altísimo rendimiento como HPC, machine learning y simulaciones científicas. La velocidad de Lustre es órdenes de magnitud mayor que EFS.

+ **Pregunta 4**: Una empresa quiere seguir usando su software de backup con cintas físicas pero almacenarlas en AWS en vez de comprar hardware nuevo. ¿Qué servicio usan?  
A) DataSync  
B) Snowball  
**C) Storage Gateway Tape Gateway**  
D) S3 Glacier directamente  
> C) Storage Gateway Tape Gateway: "Cintas físicas" + "seguir usando el mismo software" = Tape Gateway. Es exactamente para empresas que tienen infraestructura de backup en cintas y quieren migrar a la nube sin cambiar sus procesos.

+ **Pregunta 5**: Una empresa necesita que sus socios comerciales externos les envíen archivos via SFTP directamente a S3. ¿Qué servicio configura esto?  
A) DataSync  
B) Storage Gateway  
**C) AWS Transfer Family**  
D) Snowcone  
> C) AWS Transfer Family: "Socios externos" + "SFTP" + "S3" = Transfer Family. DataSync es para migración automatizada interna, no para que terceros te envíen archivos via protocolos estándar.