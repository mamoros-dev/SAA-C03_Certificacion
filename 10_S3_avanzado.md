# S3 avanzado

## Amazon S3 - Movimiento entre clases de almacenamiento
+ Puedes pasar los objetos entre las clases de almacenamiento
+ Para los objetos a los que se accede con poca frecuencia, muévelos a IA Estándar
+ Para los objetos de archivo a los que no necesitas acceder rápidamente, muévelos a Glacier o Glacier Deep Archive
+ El movimiento de los objetos puede automatizarse mediante las Reglas del Ciclo de Vida

## Amazon S3 - Reglas del ciclo de vida
+ Acciones de transición: configura los objetos para que pasen a otra clase de almacenamiento
    + Mover los objetos a la clase IA Estándar 60 días después de su creación
    + Mover a Glacier para archivar después de 6 meses
+ Acciones de expiración - configura los objetos para que caduquen (se eliminen) después de un
tiempo
    + Los archivos de logs de acceso pueden configurarse para que se eliminen después de 365 días
    + Se puede utilizar para eliminar versiones antiguas de archivos (si el versionado está
    activado)
    + Se puede utilizar para eliminar subidas incompletas de Multipartes
+ Se pueden crear reglas para un determinado prefijo (ejemplo: s3://mybucket/mp3/*)
+ Se pueden crear reglas para determinados objetos Etiquetas (ejemplo: Departamento: Finanzas)

## Amazon S3 Analytics: Análisis de la clase de almacenamiento
+ Te ayuda a decidir cuándo pasar los objetos a la clase de almacenamiento adecuada
+ Recomendaciones para IA estándar y estándar
    + NO sirve para IA de una Zona o Glacier
+ El informe se actualiza diariamente
+ De 24 a 48 horas para empezar a ver el análisis de los datos
+ Buen primer paso para elaborar las Reglas del Ciclo de Vida (o mejorarlas)

## Notificaciones de eventos S3
+ Posibilidad de filtrar el nombre del objeto (*.jpg)
+ Caso de uso: generar miniaturas de imágenes subidas a S3
+ Se pueden crear tantos "eventos S3" como se desee
+ Las notificaciones de eventos S3 suelen entregar los eventos en segundos, pero a veces pueden tardar un minuto o más

## S3 Select y Glacier Select
+ Recupera menos datos mediante SQL realizando un filtrado del lado del servidor
+ Puedes filtrar por filas y columnas (simples sentencias SQL)
+ Menos transferencia de red, menos coste de CPU en el lado del cliente

## Operaciones por lotes de S3 (Batch Operations)
+ Realiza operaciones masivas en objetos S3 existentes con una sola petición, por ejemplo:
    + Modificar los metadatos y las propiedades de los objetos
    + Copiar objetos entre buckets de S3
    + Cifrar objetos no cifrados
    + Modificar ACLs, etiquetas
    + Restaurar objetos desde S3 Glacier
    + Invocar una función Lambda para realizar una acción personalizada en cada objeto
+ Un trabajo consiste en una lista de objetos, la acción a realizar y parámetros opcionales
+ S3 Batch Operations gestiona los reintentos, sigue el progreso, envía notificaciones de finalización, genera informes ...
+ Puedes utilizar el Inventario de S3 para obtener la lista de objetos y utilizar S3 Select para filtrarlos

## PREGUNTAS TIPO EXAMEN

**Pregunta 1:**  
¿Cómo puedes recibir una notificación cuando se sube un objeto a tu bucket de S3?
> "Notificaciones de eventos del S3" porque es la opción diseñada específicamente para alertarte cuando se sube un objeto a tu bucket de S3. Esta funcionalidad permite a los usuarios implementar flujos de trabajo automáticos basados en eventos de almacenamiento, facilitando la gestión de objetos en sus buckets.

**Pregunta 2:**  
Tienes un bucket de S3 que tiene activado el control de versiones de S3. Este bucket de S3 tiene muchos objetos, y te gustaría eliminar las versiones antiguas de los objetos para reducir costes. ¿Cuál es el mejor enfoque para automatizar la eliminación de estas versiones antiguas de los objetos?
> "Reglas del ciclo de vida de S3 - Acciones de caducidad" porque esta opción te permite automatizar la eliminación de versiones antiguas de los objetos en tu bucket de S3, lo que ayuda a reducir costes y optimizar el almacenamiento.

**Pregunta 3:**  
¿Cómo puedes automatizar la transición de los objetos de S3 entre sus diferentes niveles?
> "Reglas del ciclo de vida de S3" porque esta opción permite automatizar la transición de objetos entre diferentes niveles de almacenamiento, mejorando la eficiencia y reduciendo costos en la gestión de datos.

**Pregunta 4:**  
Mientras subes archivos grandes a un bucket de S3 utilizando la Carga Multiparte, hay muchas partes inacabadas almacenadas en el bucket de S3 debido a problemas de red. No estás utilizando estas partes inacabadas y te cuestan dinero. ¿Cuál es el mejor enfoque para eliminar estas partes inacabadas?
> "Utiliza una política de ciclo de vida de S3 para automatizar la eliminación de piezas antiguas/no terminadas" porque esta opción permite gestionar automáticamente el almacenamiento de objetos inacabados, reduciendo costos y optimizando el uso de espacio sin necesidad de intervención manual.

**Pregunta 5:**  
Quieres obtener recomendaciones para las reglas del ciclo de vida de S3. ¿Cómo puedes analizar el número óptimo de días para mover objetos entre los distintos niveles de almacenamiento?
>  "S3 Analytics" porque esta herramienta permite analizar el acceso y uso de los objetos en tus buckets de S3, ayudándote a determinar el número óptimo de días para mover objetos entre diferentes niveles de almacenamiento, lo que mejora la gestión de costos y la eficiencia.

**Pregunta 6:**  
Estás buscando construir un índice de tus archivos en S3, utilizando Amazon RDS PostgreSQL. Para construir este índice, es necesario leer los primeros 250 bytes de cada objeto en S3, que contienen algunos metadatos sobre el contenido del propio archivo. Hay más de 100.000 archivos en tu bucket de S3, lo que supone 50 TB de datos. ¿Cómo puedes construir este índice de forma eficiente?
>  "Crea una aplicación que atraviese el bucket de S3, emita un Byte Range Fetch para los primeros 250 bytes y almacene esa información en RDS" porque usar Byte Range Fetch permite leer solo una porción específica de cada objeto, lo que optimiza el rendimiento y reduce el tiempo y el costo de la operación, siendo la opción más eficaz para construir el índice de los archivos.

**Pregunta 7:**  
Tienes un gran conjunto de datos almacenado en las instalaciones que quieres subir al bucket de S3. El conjunto de datos está dividido en archivos de 10 GB. Tienes un buen ancho de banda, pero tu conexión a Internet no es estable. ¿Cuál es la mejor manera de subir este conjunto de datos a S3 y garantizar que el proceso sea rápido y evitar cualquier problema con la conexión a Internet?
> "Utiliza la carga de varias partes en S3 y utiliza la aceleración de la transferencia en S3 (S3 Transfer Acceleration)" porque esta combinación permite subir grandes archivos de manera eficiente, ya que la carga de varias partes facilita la reanudación en caso de interrupciones y la aceleración de la transferencia optimiza la velocidad de carga, lo que es ideal para conexiones a Internet inestables.

**Pregunta 8:**  
Te gustaría recuperar un subconjunto de tu conjunto de datos almacenado en S3 con el formato .csv. Te gustaría recuperar un mes de datos y sólo 3 columnas de 10, para minimizar los costes de cálculo y de red. ¿Qué deberías utilizar?
> "S3 Select" porque esta funcionalidad te permite recuperar solo las columnas y filas de datos específicas que necesitas de un archivo CSV almacenado en S3, lo que minimiza los costos de cálculo y de transferencia de datos, optimizando así el rendimiento. Esto se alinea con el objetivo de gestionar eficientemente los recursos en S3.

**Pregunta 9:**  
Una empresa se está preparando para una revisión normativa en su infraestructura en AWS. Actualmente, tienen sus archivos almacenados en buckets de S3 que no están encriptados, y que deben ser encriptados como se requiere para la revisión normativa y de cumplimiento. ¿Qué función de S3 les permite cifrar todos los archivos de sus buckets de S3 de la forma más eficiente y rentable?
> "S3 Batch Operations (Operaciones por lotes)" porque esta función permite encriptar de manera eficiente y rentable todos los archivos en los buckets de S3, así cumpliendo con los requisitos normativos al procesar múltiples objetos en una sola acción. Es ideal para manejar grandes volúmenes de datos, simplificando así la tarea de garantizar la seguridad de la información.