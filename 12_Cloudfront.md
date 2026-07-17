# CloudFront

## Introducción

Amazon CloudFront es un servicio de distribución de contenido (CDN) de AWS que entrega datos, vídeos, aplicaciones y APIs a los usuarios con baja latencia y altas velocidades de transferencia.

CloudFront utiliza una red global de ubicaciones de borde para almacenar en caché contenido estático y dinámico, reduciendo el tiempo de carga y mejorando la experiencia del usuario.

## Casos de uso

- Aceleración de sitios web estáticos y dinámicos.
- Distribución de contenido multimedia, como vídeo bajo demanda y transmisiones en vivo.
- Protección de aplicaciones web mediante la integración con AWS WAF y AWS Shield.
- Entrega de APIs y recursos móviles con mejor rendimiento y menor latencia.
- Caching de activos de aplicaciones para reducir la carga en los servidores de origen.
- Distribución segura de contenido privado con firmas y políticas de acceso.

## Orígenes
+ Bucket S3:
    + Para distribuir archivos y almacenarlos en caché en el borde
    + Seguridad mejorada con CloudFront Origin Access Control (OAC)
    + OAC sustituye a Origin Access Identity (OAI)
    + CloudFront puede utilizarse como entrada (para subir archivos a S3)
+ Origen personalizado (HTTP):
    + Application Load Balancer
    + Instancia EC2
    + Sitio web S3 (primero debes habilitar el bucket como sitio web S3 estático)
    + Cualquier backend HTTP que desees

## CloudFront vs S3 Cross Region Replication (CRR)
+ CloudFront:
    + Red Global Edge
    + Los archivos se almacenan en caché durante un TTL (quizás un día)
    + Es ideal para contenidos estáticos que deben estar disponibles en todas partes
+ S3 Cross Region Replication (CRR):
    + Debe configurarse para cada región en la que quieras que se produzca la replicación
    + Los archivos se actualizan casi en tiempo real
    + Sólo lectura
    + Ideal para contenidos dinámicos que deben estar disponibles con baja latencia en pocas regiones

## CloudFront - Clases de precios
+ Puedes reducir el número de Edge Locations para reducir los costes
+ Tres clases de precios:
    1. Clase de precio Todos: todas las regiones - mejor rendimiento
    2. Clase de precio 200: la mayoría de las regiones, pero excluye las regiones más caras
    3. Clase de precio 100: sólo las regiones menos caras

## IP unicast vs IP anycast
+ IP unicast: un servidor tiene una dirección IP
+ IP Anycast: todos los servidores tienen la misma dirección IP y el cliente es dirigido al más cercano

## AWS Global Accelerator
+ Aprovecha la red interna de AWS para dirigirte a tu aplicación
+ Se crean 2 IP Anycast para tu aplicación
+ Las IP Anycast envían el tráfico directamente a las Edge Locations
+ Las Edge Locations envían el tráfico a tu aplicación
+ Funciona con Elastic IP, instancias EC2, ALB, NLB
+ Rendimiento consistente
    + Enrutamiento inteligente para lograr la menor latencia y una rápida conmutación por error
    regional
    + No hay problemas con la caché del cliente (porque la IP no cambia)
    + Red interna de AWS
+ Comprobaciones de salud
    + Global Accelerator realiza una comprobación de la salud de tus aplicaciones
    + Ayuda a que tu aplicación sea global (conmutación por error en menos de 1 minuto en caso
    de no ser saludable)
    + Genial para la recuperación de desastres (gracias a las comprobaciones de salud)
+ Seguridad
    + Sólo hay que poner en la lista blanca 2 IP externas
    + Protección DDoS gracias a AWS Shield

## AWS Global Accelerator vs CloudFront
+ Ambos utilizan la red global de AWS y sus Edge Locations en todo el mundo
+ Ambos servicios se integran con AWS Shield para la protección DDoS.
+ CloudFront:
    + Mejora el rendimiento tanto del contenido almacenable en caché (como imágenes y vídeos)
    + Contenido dinámico (como la aceleración de la API y la entrega de sitios dinámicos)
    + El contenido se sirve en el borde
+ Global Accelerator:
    + Mejora el rendimiento de una amplia gama de aplicaciones sobre TCP o UDP
    + Proxy de paquetes en el borde a las aplicaciones que se ejecutan en una o más regiones de AWS.
    + Es adecuado para casos de uso no HTTP, como juegos (UDP), IoT (MQTT) o voz sobre IP
    + Bueno para casos de uso HTTP que requieren direcciones IP estáticas
    + Bueno para casos de uso de HTTP que requieran una conmutación por error regional determinista y rápida

## RESUMEN

+ CloudFront explicado con un escenario real:
    - Imagina que tienes una web en un servidor en Irlanda. Un usuario de Tokio la visita — su petición viaja miles de kilómetros hasta Irlanda y vuelve. Lento.
    - Con CloudFront, la primera vez sí va a Irlanda, pero CloudFront guarda una copia en el edge más cercano a Tokio. La segunda petición ya no viaja a Irlanda — se sirve desde Tokio directamente. Mucho más rápido.
    -  Eso es CloudFront en esencia: una red de caché global que acerca tu contenido al usuario.

+ Ejemplos reales en España:
    - Una tienda online con imágenes de producto → CloudFront las sirve desde el edge más cercano al usuario
    - Una plataforma de formación con vídeos → CloudFront distribuye el contenido sin saturar el origen
    - Una app con usuarios en España, Latinoamérica y Europa → CloudFront reduce la latencia para todos

+ Los conceptos clave para el examen:
    - Origin → de dónde saca CloudFront el contenido original. Puede ser S3, ALB, EC2 o cualquier servidor HTTP.
    - Edge Location → los puntos de presencia de CloudFront repartidos por el mundo donde se cachea el contenido. Hay más de 400.
    - TTL → cuánto tiempo se guarda el contenido en caché. Cuando expira, CloudFront vuelve al origen a buscar la versión nueva.
    - Invalidación de caché → si publicas una versión nueva de tu web y quieres que los usuarios la vean inmediatamente sin esperar al TTL, invalidas la caché manualmente.
    - Restricción geográfica → bloquear o permitir acceso solo desde ciertos países. Ejemplo: contenido con licencia solo para España.

+ Palabra clave examen: "caché", "contenido estático", "latencia web" → **CloudFront**. "IP fija global", "tiempo real", "TCP/UDP" → **Global Accelerator**.


## CUESTIONARIO

+ **Pregunta 1:** Tienes una distribución de CloudFront que sirve a tu sitio web alojado en una flota de instancias EC2 detrás de un Load Balancer de aplicaciones. Todos tus clientes son de Estados Unidos, pero has descubierto que algunas peticiones maliciosas proceden de otros países. ¿Qué deberías hacer para permitir sólo a los usuarios de Estados Unidos y bloquear a los de otros países?
> "Utiliza la restricción geográfica de CloudFront" porque esta funcionalidad permite limitar el acceso a tu contenido exclusivo para usuarios de ciertos países, en este caso, Estados Unidos, bloqueando así las peticiones maliciosas provenientes de otras regiones. Esto es crucial para proteger tu infraestructura y optimizar la seguridad de tu sitio web.

+ **Pregunta 2:** Tienes un sitio web estático alojado en un bucket de S3. Has creado una distribución de CloudFront que apunta a tu bucket de S3 para atender mejor las peticiones y mejorar el rendimiento. Después de un tiempo, te has dado cuenta de que los usuarios pueden seguir accediendo a tu sitio web directamente desde el bucket de S3. Quieres obligar a los usuarios a acceder al sitio web sólo a través de CloudFront. ¿Cómo lo conseguirías?
> configurar una Identidad de Acceso al Origen (OAI) en tu distribución de CloudFront y actualizar la política del bucket S3 garantiza que solo las solicitudes autorizadas a través de CloudFront puedan acceder a los recursos, evitando el acceso directo al bucket y mejorando la seguridad de tu sitio web. Esto se alinea con el objetivo de aprender a proteger recursos en AWS mediante configuraciones adecuadas de acceso.

+ **Pregunta 3:** ¿Qué hace esta política de bucket S3?
```
{
     "Version": "2012-10-17",
     "Id": "Mystery policy",
     "Statement": [{
        "Sid": "What could it be?",
        "Effect": "Allow",
        "Principal": {
           "Service": "cloudfront.amazonaws.com"
        },
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::examplebucket/*",
         "Condition": {
              "StringEquals": {
                 "AWS:SourceArn":  "arn:aws:cloudfront::123456789012:distribution/EDFDVBD6EXAMPLE"
           }
      }
   }]
}
```
> permite que solo las solicitudes de CloudFront, mediante la Identidad de Acceso al Origen, accedan al contenido del bucket de S3. Esto asegura que el acceso a los objetos sea controlado y seguro, impidiendo que otras entidades o usuarios accedan directamente al bucket.

+ **Pregunta 4:** Un sitio web de WordPress está alojado en un conjunto de instancias de EC2 en un Grupo de Auto Scaling de EC2 y está encabezado por una Distribución de CloudFront que está configurada para almacenar en caché el contenido durante 3 días. Has lanzado una nueva versión del sitio web y quieres lanzarla inmediatamente a producción sin esperar 3 días a que caduque el contenido almacenado en caché. ¿Cuál es la forma más fácil y eficaz de resolver esto?
> "Invalidación de la caché de CloudFront" porque al invalidar la caché, puedes forzar a CloudFront a eliminar el contenido almacenado y servir la nueva versión del sitio web inmediatamente, asegurando que los usuarios vean los cambios sin esperar el tiempo de expiración de 3 días. Esto es una práctica eficaz para mantener tu contenido actualizado.

+ **Pregunta 5:** Una empresa está desplegando un sitio web para compartir medios en AWS. Van a utilizar CloudFront para entregar el contexto con baja latencia a sus clientes, que se encuentran en EE.UU. y en Europa solamente. Después de un tiempo hay un gran coste para CloudFront. ¿Qué característica de CloudFront permite disminuir los costes al dirigirse sólo a EE.UU. y Europa?
> "Clases de precios de CloudFront" porque esta característica permite optimizar los costos al seleccionar las tarifas adecuadas según la región de entrega, lo que es beneficioso para dirigir el tráfico solo a EE. UU. y Europa y así reducir gastos innecesarios. Esta respuesta refleja un entendimiento clave de cómo gestionar costos en la entrega de contenido en la nube.

+ **Pregunta 6:** Una empresa está migrando una aplicación web a AWS Cloud y va a utilizar un conjunto de instancias EC2 en un EC2 Auto Scaling Groups. La aplicación web está formada por múltiples componentes, por lo que necesitarán una función de enrutamiento basada en el host para dirigirla a componentes específicos de la aplicación web. Esta aplicación web es utilizada por muchos clientes y, por tanto, la aplicación web debe tener una dirección IP estática para que pueda ser incluida en la lista blanca de los firewalls de los clientes. Como los clientes están distribuidos por todo el mundo, la aplicación web también debe proporcionar una baja latencia a todos los clientes. ¿Qué servicio de AWS puede ayudarte a asignar una dirección IP estática y proporcionar una baja latencia en todo el mundo?
>  "AWS Global Accelerator + Application Load Balancer" porque esta combinación proporciona una dirección IP estática, lo que permite inclusión en listas blancas, y mejora la latencia a nivel global al enrutar el tráfico a las instancias más cercanas. Esto asegura un rendimiento óptimo y confiable para los usuarios de la aplicación web en todo el mundo.


## PREGUTNAS TIPO EXAMEN

**Pregunta 1**  
Una empresa tiene su web con imágenes y vídeos en S3 en eu-west-1. Tienen usuarios en Europa, Asia y América que se quejan de lentitud. ¿Qué servicio añaden para mejorar la experiencia global con mínimo coste?  
A) Global Accelerator  
B) Route 53 Latency  
**C) CloudFront**  
D) ALB multi-región  
> C) CloudFront. S3 como origen + usuarios globales + latencia = CloudFront siempre. Route 53 Latency solo enruta al servidor más cercano pero no cachea nada — el contenido sigue viajando desde el origen cada vez.  

**Pregunta 2**  
Una plataforma de streaming acaba de actualizar sus vídeos promocionales en S3. Los usuarios siguen viendo los vídeos antiguos aunque ya se subieron los nuevos. ¿Qué hace el equipo para que los usuarios vean el contenido actualizado inmediatamente?  
A) Eliminar y volver a crear la distribución de CloudFront  
B) Cambiar el TTL a 0 permanentemente  
**C) Crear una invalidación de caché en CloudFront**  
D) Subir los vídeos a otro bucket S3  
> C) Crear una invalidación de caché en CloudFront. La invalidación de caché es la solución estándar cuando publicas contenido nuevo y no puedes esperar a que expire el TTL. En el mundo real se usa mucho en deploys de webs — cada vez que subes una nueva versión, invalidas la caché para que los usuarios vean los cambios inmediatamente.

**Pregunta 3**  
Una empresa tiene una aplicación de videojuegos online que necesita la menor latencia posible para conexiones TCP en tiempo real desde cualquier parte del mundo. No necesitan caché. ¿Qué servicio usan?  
A) CloudFront  
B) Route 53 Simple  
**C) Global Accelerator**  
D) ELB  
> C) Global Accelerator. "TCP en tiempo real" + "sin caché" + "latencia mínima global" = Global Accelerator. El detalle técnico que suma: Global Accelerator usa la red privada de AWS en vez de internet público, que es más estable y rápida.

**Pregunta 4**  
Una empresa tiene contenido con derechos de autor que solo puede distribuirse legalmente en España y Portugal. ¿Qué función de CloudFront usan?  
A) Invalidación de caché  
**B) Restricción geográfica**  
C) Price Class  
D) TTL personalizado  
> B, Restricción geográfica es exactamente para cumplimiento legal de derechos por país. Price Class en cambio sirve para reducir costes eligiendo en qué regiones del mundo se despliegan los edge locations — no tiene nada que ver con bloquear acceso.