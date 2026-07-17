# CERTIFICADO AWS SOLUTIONS ARCHITECT ASSOCIATE

+ AWS (Amazon Web Services).  
+ Te proporcionan servidores y servicios bajo demanda y escalar facilmente, lo más importante lo de escalar.  
+ Web tutor Joan Amengual: https://learn.blockstellart.com/  
+ Recursos: https://blockstellart.com/aws-solutions-architect-associate-asf/  
+ [DISCORD](https://discord.com/channels/874674646119764018/1242189257330786444)
+ Cuenta free 6 meses:
    - miguelito_5_@hotmail.com
    - miguel_aws

+ Videos interesantes:
    - [Qué hace un cloud engineer](https://www.youtube.com/watch?v=t3ncM_TCxyc)
    - [Webinar SAA-C03](https://www.youtube.com/watch?v=IQo6t98O-BI)
    - [Servicios AWS populares](https://www.youtube.com/watch?v=QIMNep-nFZc)
    - [Servicios Azure populares](https://www.youtube.com/watch?v=dQFIvM2x-nE)

## INFRAESTRUCTURA GLOBAL

+ AWS REGIONS / REGIONES
+ AWS AVAILIBILITY ZONES / ZONAS DE DISPONIBILIDAD
+ AWS DATA CENTERS / CENTRO DE DATOS
+ AWS EDGE LOCATIONS-POINTS OF PRESENCE / PUNTOS DE PRESENCIA
+ [ZONAS DE DISPONIBILIDAD Y REGIONES](https://aws.amazon.com/es/about-aws/global-infrastructure/regions_az/)

### REGIONES / REGIONS
+ Las regiones tienen nombre especificos como us-east-1, eu-west-3. Una región es un grupo de centros de datos. La mayoría de los servicios de AWS son de ámbito regional.
+ ¿Cómo elegir una regions de AWS?
    - Cumplimiento de los requisitos legales y de gobernanza de datos: los datos nunca salen de una región si tu permiso explícito.
    - Proximidad a los clientes: latencia reducida.
    - Servicios disponibles en una región: los nuevos servicios y las nuevas funciones no están en todas.
    - Precios: los precios cambias de una región a otra y hay una tabla de precios bien transparentes en la web aws

### ZONAS DE DISPONIBLIDAD / AVAILIBILITY ZONES (AZ)
+ Cada región tiene muchas zonas de disponibilidad, normalmente 3(mínimo) y el máximo es 6.
+ Cada AZ es uno o varios centro de datos discretos con alimentación, red y conectividad redundantes.
+ Están separadas unas de las otras de manera geográfica para que en casa de que en una zona pase una catástrofe, la otra no se vería afectada.
+ Están conectadas por redes de alto ancho de banda y latencia ultrabaja.
+ Ejemplo Sydney: 
    - ap-southeast-2 (Región)
        - ap-southeast-2a (AZ formada por centros de datos)
        - ap-southeast-2b (AZ formada por centros de datos)
        - ap-southeast-2c (AZ formada por centros de datos)

### PUNTOS DE PRESENCIA / EDGE LOCATIONS
+ Amazon tiene +450 puntos de presencia (+10 cachés regionales) y +90 ciudades de +40 paises.
+ El contenido se entrega a los usuarios finales con menor latencia.
+ Podemos ver esos puntos que van a ir en aumento cada día en este [MAPA EDGE LOCATIONS](https://aws.amazon.com/es/cloudfront/features/)

## CONSOLA AWS

+ Existen servicios de ámbito global y regional. Los podemos ver en este [LISTADO SERVICIOS SEGÚN REGIÓN](https://aws.amazon.com/es/about-aws/global-infrastructure/regional-product-services/).  

+ Globales, eso hace que no puedas elegir región para usarlo, ya que es servicio global de AWS:
    - IAM (Identity and Acces Management)
    - ROUTE 53 (Servicio DNS)
    - Cloudfront (Red de entrega de contenido más rápido a cliente final)
    - WAF ( Firewall de aplicaciones web)

+ La Mayoría de servicios son de ámbito regional, eso sí, cuando usen servicios siempre en la misma región:
    - AMAZON EC2 (Infraestructura como servicio)
    - Elastic BeanStalk (Plataforma como servicio)
    - Lambda (Función como servicio)
    - Rekognition (Software como servicio)