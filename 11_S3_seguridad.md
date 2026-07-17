# Seguridad de Amazon S3

## Amazon S3 - Cifrado de objetos
+ Puedes cifrar objetos en buckets de S3 utilizando uno de los 4 métodos siguientes
+ Cifrado del lado del servidor (SSE):
+ Cifrado del lado del servidor con claves gestionadas por Amazon S3 (SSE-S3) -
    + Activado por defecto
    + Cifra los objetos de S3 utilizando claves manejadas, gestionadas y propiedad de AWS
+ Cifrado del lado del servidor con claves KMS almacenadas en AWS KMS (SSE-KMS)
    + Aprovecha el servicio de administración de claves de AWS (AWS KMS) para gestionar las claves de cifrado
+ Cifrado del lado del servidor con claves proporcionadas por el cliente (SSE-C)
    + Cuando quieras gestionar tus propias claves de cifrado
+ Cifrado del lado del cliente
    - Utiliza bibliotecas de clientes como la biblioteca de cifrado del lado del cliente de Amazon S3
    - Los clientes deben cifrar los datos ellos mismos antes de enviarlos a Amazon S3
    - Los clientes deben descifrar los datos ellos mismos al recuperarlos de Amazon S3
    - El cliente gestiona completamente las claves y el ciclo de cifrado

## CORS
+ Compartir recursos entre orígenes (CORS)
+ Origen = esquema (protocolo) + host (dominio) + puerto
    + Ejemplo: https://www.example.com (el puerto implícito es 443 para HTTPS, 80 para
HTTP)
+ Mecanismo basado en el navegador web para permitir peticiones a otros
orígenes mientras se visita el origen principal
+ El mismo origen: http://example.com/app1 y http://example.com/app2
+ Diferentes orígenes: http://www.example.com y http://other.example.com
+ Las peticiones no se cumplirán a menos que el otro origen permita las peticiones,
utilizando cabeceras CORS (ejemplo: Access-Control-Allow-Origin)

## Eliminación de MFA
+ MFA (Autenticación de Factores Múltiples): obliga a los usuarios a generar un código en un dispositivo (normalmente un teléfono móvil o un hardware) antes de realizar operaciones importantes en el S3
+ MFA será necesario para:
    + Eliminar permanentemente una versión de un objeto
    + Suspender el control de versiones en el bucket
+ MFA no será necesario para:
    + Habilitar el control de versiones
    + Listar las versiones eliminadas
+ Para utilizar MFA Delete, el control de versiones debe estar activado en el bucket
+ Sólo el propietario del bucket (cuenta root) puede activar/desactivar MFA Delete

+ S3 Access Logs: Para fines de auditoría, es posible que quieras registrar todos los
accesos a los buckets de S3

+ Objeto S3 Lambda: Utiliza las Funciones Lambda de AWS para modificar el objeto antes de que lo recupere la aplicación que lo llama. Sólo se necesita un bucket de S3, sobre el que creamos puntos de acceso de S3 y puntos de acceso de S3 Object Lambda.

## PREGUNTAS TIPO EXAMEN

**Pregunta 1:**  
Tu cliente quiere asegurarse de que el cifrado de los archivos se realiza en S3, pero quiere gestionar completamente las claves de cifrado y no almacenarlas nunca en AWS. Le recomiendas que utilice
> "SSE-C" porque te permite cifrar archivos en S3 manteniendo un control total sobre las claves de cifrado, lo que significa que nunca se almacenarán en AWS. Esto es fundamental para cumplir con requisitos de seguridad y gestión de claves que tu cliente demanda.

**Pregunta 2:**  
Una empresa para la que trabajas quiere que sus datos almacenados en S3 estén cifrados. No les importa que las claves de cifrado sean almacenadas y gestionadas por AWS, pero quieren mantener el control sobre la política de rotación de las claves de cifrado. Les recomiendas que utilicen
> "SSE-KMS" porque esta opción te permite cifrar los datos almacenados en S3 mientras AWS se encarga de la gestión de las claves de cifrado, dándote a ti el control sobre la política de rotación de dichas claves. Esto asegura que tus datos estén protegidos y que cumplan con tus requisitos de gestión de seguridad.

**Pregunta 3:**  
Tu empresa no confía en AWS para el proceso de cifrado y quiere que éste se realice en la aplicación. Les recomiendas que utilicen
> "Encriptación del lado del cliente" porque este método te permite cifrar tus datos antes de enviarlos a AWS, asegurando que mantienes el control total sobre las claves de cifrado. De esta manera, AWS no tiene acceso a tus claves, lo que incrementa la seguridad y cumple con las expectativas de tu empresa en cuanto al manejo de datos sensibles.

**Pregunta 4:**  
Tienes un sitio web que carga archivos desde un bucket de S3. Cuando pruebas la URL de los archivos directamente en tu navegador Chrome funciona, pero cuando el sitio web que visitas intenta cargar estos archivos no lo hace. ¿Cuál es el problema?
> "CORS es incorrecto" porque el problema radica en que el uso compartido de recursos entre orígenes (CORS) permite que las aplicaciones web de un dominio accedan a recursos en otro dominio. Si CORS no está configurado adecuadamente en tu bucket de S3, los archivos no se cargarán cuando se soliciten desde un sitio web diferente.

**Pregunta 5:**  
Una empresa de comercio electrónico tiene los datos de sus clientes y pedidos almacenados en un bucket de S3. El director general de la empresa quiere generar un informe que muestre la lista de clientes y los ingresos de cada uno de ellos. Los datos de los clientes almacenados en los archivos del bucket de S3 tienen información sensible que no queremos exponer en el informe. ¿Cómo recomiendas que se cree el informe sin exponer la información sensible?
> "Utiliza S3 Object Lambda para modificar los objetos antes de que sean recuperados por la aplicación generadora de informes" porque esta solución permite procesar los datos sensibles en el momento de la recuperación, eliminando la información que no deseas exponer en el informe sin necesidad de crear múltiples buckets o funciones adicionales. Esto asegura que la información confidencial se mantenga protegida mientras se generan los informes necesarios.

**Pregunta 6:**  
Sospechas que algunos de tus empleados intentan acceder a archivos en un bucket de S3 al que no tienen acceso. ¿Cómo puedes verificar que esto es así sin que se den cuenta?
> "Habilita los S3 Access Logs y analízalos con Athena" porque esta opción te permite monitorear de forma discreta las solicitudes realizadas a tu bucket de S3 sin alertar a los empleados. Los registros de acceso son útiles para identificar intentos de acceso no autorizados, y Athena facilita el análisis de estos registros sin necesidad de infraestructura adicional.

**Pregunta 7:**  
Quieres proporcionar URLs temporales a una lista creciente de usuarios federados para permitirles realizar una carga de archivos en tu bucket de S3 a una ubicación específica. ¿Qué deberías utilizar?
> "URL pre-firmada de S3" porque estas URLs permiten proporcionar acceso temporal y controlado a tu bucket de S3, lo que es ideal para permitir a los usuarios federados cargar archivos sin necesidad de compartir tus credenciales o configurar permisos a largo plazo. Esto garantiza la seguridad y flexibilidad en la gestión de accesos.

**Pregunta 8:**  
Por razones de normativa, tu empresa tiene el mandato de que las copias de seguridad de las bases de datos deben conservarse durante 4 años. No debería ser posible borrarlas. ¿Qué recomiendas?
> "Bóvedas de Glacier (Glacier Vaults) con políticas de bloqueo de bóvedas" porque estas bóvedas te permiten almacenar datos a largo plazo y aplicar políticas que impiden su eliminación, cumpliendo así con los requisitos normativos de retención de datos durante 4 años. Esto asegura que tus copias de seguridad estén protegidas y no puedan ser borradas accidentalmente o intencionadamente.

**Pregunta 9:**  
Te gustaría que todos tus archivos en un bucket de S3 estuvieran encriptados por defecto. ¿Cuál es la forma óptima de conseguirlo?
> "Habilitar el cifrado por defecto" porque esta opción garantiza que todos los archivos nuevos que se suban a tu bucket de S3 se encripten automáticamente en reposo, asegurando así la protección de tus datos sin necesidad de intervención manual. Esto es fundamental para cumplir con normas de seguridad y privacidad que exigen el cifrado de datos críticos.

**Pregunta 10:**  
Has habilitado el control de versiones y quieres ser muy cuidadoso a la hora de borrar archivos en un bucket de S3. ¿Qué deberías habilitar para evitar los borrados permanentes accidentales?
> "Habilitar el borrado MFA" porque esta opción añade una capa de seguridad necesaria: requiere que los usuarios ingresen un código de autenticación multifactor antes de poder eliminar objetos de S3, lo que ayuda a prevenir eliminaciones accidentales y protege tus datos. 

**Pregunta 11:**  
Una empresa tiene sus datos y archivos almacenados en algunos buckets de S3. Algunos de estos archivos deben conservarse durante un periodo de tiempo predefinido y estar protegidos para que no se sobrescriban ni se eliminen según la normativa de la empresa. ¿Qué función de S3 te ayuda a hacer esto?
>  "S3 Object Lock - Modo de cumplimiento de la normativa de retención" porque esta función te permite establecer políticas que protegen los objetos en S3 contra la eliminación o sobrescritura durante un período definido, asegurando que cumples con las regulaciones de retención de datos de tu empresa de manera efectiva.

**Pregunta 12:**  
¿Cuál de las siguientes configuraciones de Bloqueo de Objetos de S3 (S3 Object Lock) te permite impedir que un objeto o sus versiones se sobrescriban o eliminen indefinidamente y te da la posibilidad de eliminarlo manualmente?
> "Retención legal" porque esta configuración de S3 Object Lock permite que un objeto y sus versiones sean protegidos contra la eliminación o sobrescritura indefinidamente, asegurando el cumplimiento de regulaciones de retención de datos. Esto es esencial para organizaciones que necesitan mantener datos críticos por un período prolongado.

