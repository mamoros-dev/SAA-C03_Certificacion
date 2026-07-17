# Amazon EC2

## Facturación
+ Para que los usuarios IAM tengan "permisos" para ver los costes, en la cuenta root tenemos que ir a CUENTA y luego buscar la opción abajo de:
```
Acceso de usuario de IAM y rol a información de facturación:
- Active el acceso de IAM
```

+ Si queremos crear Presupuestos/Budgets en Planificacion y presupuestos/Cost Management podemos hacer plantillas de costo 0, umbrales mensuales, alertas por email cuando se supere, etc.

## Conceptos básicos 
+ EC2 es una de las ofertas más populares de AWS
+ EC2 = Elastic Compute Cloud = Infraestructura como servicio (IaaS)
+ Consiste principalmente en la capacidad de :
    + Alquilar máquinas virtuales (EC2)
    + Almacenar datos en unidades virtuales (EBS)
    + Distribuir la carga entre las máquinas (ELB)
    + Escalar los servicios mediante un Auto Scaling Group (ASG) o también conocido en español como Grupo de Autoescalado
+ Conocer EC2 es fundamental para entender el funcionamiento del Cloud

## Opciones de tamaño y configuración de EC2
+ Sistema operativo (OS): Linux, Windows o Mac OS
+ Cuánta potencia de cálculo y núcleos (CPU)
+ Cuánta memoria de acceso aleatorio (RAM)
+ Cuánto espacio de almacenamiento:
    + Conectado a la red (EBS y EFS)
    + hardware (EC2 Instance Store)
+ Tarjeta de red: velocidad de la tarjeta, dirección IP pública
+ Reglas de firewall: grupo de seguridad
+ Script de arranque (configurar en el primer lanzamiento): Datos de usuario de EC2

## Datos del usuario de EC2
+ Es posible arrancar nuestras instancias utilizando un script de datos de usuario de EC2.
+ bootstrapping significa lanzar comandos cuando una máquina se inicia
+ Ese script sólo se ejecuta una vez en el primer arranque de la instancia
+ Los datos de usuario de EC2 se utilizan para automatizar tareas de arranque como:
    + Instalar actualizaciones
    + Instalación de software
    + Descarga de archivos comunes de Internet
    + Cualquier cosa que se te ocurra
+ El script de datos de usuario de EC2 se ejecuta con el usuario root

## Crear primera instancia
+ Vamos a lanzar nuestro primer servidor virtual utilizando la consola de AWS
+ Tendremos una primera aproximación de alto nivel a los distintos
parámetros
+ Veremos que nuestro servidor web se lanza utilizando los datos de
usuario de EC2
+ Aprenderemos a iniciar / parar / terminar nuestra instancia.
+ Prepárese para iniciar sesión de forma segura en su instancia con un par de claves. Un par de claves es un conjunto de credenciales de seguridad que utiliza para demostrar su identidad cuando se conecta a la instancia. La clave pública está en su instancia y la clave privada está en su equipo.
+ Configure la red para permitir el acceso a Internet. Lanzará la instancia en una subred de su propia nube privada virtual (VPC) dentro de la nube de Amazon. Una subred es un rango de direcciones IP dentro de su VPC. La subred predeterminada es una subred pública, lo que significa que asignará una dirección IP pública y proporcionará acceso a Internet a la instancia desde fuera de la red de Amazon.  
> Consejo: En las instancias de prueba, puede usar la configuración de subred predeterminada que proporciona acceso a Internet. Sin embargo, para las instancias de producción, se recomienda asignar solo una IP pública y usar una subred con acceso a Internet cuando sea absolutamente necesario.  
+ Configure el firewall. Un grupo de seguridad es un conjunto de reglas de firewall que controlan el tráfico hacia su instancia. Para conectarse mediante el SSH desde su equipo local a su instancia, necesita una regla que permita el tráfico SSH desde su equipo local.  
> Consejo: La dirección IP de su equipo local puede cambiar con el tiempo si su proveedor de servicios de Internet usa una asignación de IP dinámica. En las instancias de prueba, está bien permitir el tráfico desde cualquier dirección IP (0.0.0.0/0) para que siempre pueda conectarse aunque su dirección IP cambie. Sin embargo, en el caso de las instancias de producción, especialmente aquellas con datos confidenciales, se recomienda permitir el tráfico únicamente desde direcciones IP conocidas.  
> Hemos añadido tráfico de entrada HTTP en esta pequeña práctica para que podamos entrar por web.  
+ Configure el almacenamiento. Un volumen de Amazon EBS es un dispositivo de almacenamiento que funciona como un disco duro físico. El volumen raíz es un volumen especial de EBS que almacena la AMI, que incluye el sistema operativo y el software necesarios para arrancar la instancia.
+ En este caso, en Detalles Avanzados, hemos puesto en datos de usuario, este pequeño script:
```
#!/bin/bash
# Utiliza esto para tus datos de usuario
# Instala httpd (Version: Linux 2)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hola Mundo desde $(hostname -f)</h1>" > /var/www/html/index.html
```
> Esto nos permite que cuando se carga la instancia, ya tenga esto definido desde el principio.  
+ Cuando se para o reinicia una instancia, la ip pública cambia, muy a tener en cuenta por si queremos entrar. Para tener ip fija se utilitza otro servicio.  

![](./imagenes/ec2_practica_inicial.png)  

## Tipos de instancias EC2  

+ Puedes utilizar diferentes tipos de instancias EC2 optimizadas para
diferentes casos de uso (https://aws.amazon.com/ec2/instance-types/)
+ AWS tiene la siguiente convención de nombres:
m5.2xlarge  
    + m: clase de instancia
    + 5: generación (AWS los mejora con el tiempo)
    + 2xlarge: tamaño dentro de la clase de instancia

### Propósito general

+ Excelente para una diversidad de cargas de trabajo, como servidores web o repositorios de código.
+ Equilibrio entre:
    + Computación
    + Memoria
    + Red
+ En el curso, utilizaremos la instancia t2.micro que es una instancia EC2 de propósito general

### Computación optimizada
+ Ideal para tareas de cálculo intensivo que requieren procesadores de alto rendimiento:
    + Cargas de trabajo de procesamiento por lotes
    + Transcodificación de medios
    + Servidores web de alto rendimiento
    + Computación de alto rendimiento (HPC)
    + Modelado científico y aprendizaje automático
    + Servidores dedicados a juegos

### Memoria optimizada
+ Rápido rendimiento para cargas de trabajo que procesan grandes conjuntos de datos en memoria
+ Casos de uso:
    + Alto rendimiento, bases de datos relacionales/no relacionales
    + Almacenes de caché distribuidos a escala web
    + Bases de datos en memoria optimizadas para BI (business intelligence)
    + Aplicaciones que realizan el procesamiento en tiempo real de grandes datos no estructurados

### Almacenamiento optimizado
+ Ideal para tareas de almacenamiento intensivo que requieran un acceso alto y secuencial de lectura y escritura a grandes conjuntos de datos en el almacenamiento local
+ Casos de uso:
    + Sistemas de procesamiento de transacciones en línea (OLTP) de alta frecuencia
    + Bases de datos relacionales y NoSQL
    + Caché para bases de datos en memoria (por ejemplo, Redis)
    + Aplicaciones de almacenamiento de datos
    + Sistemas de archivos distribuidos

## Grupos de seguridad

+ Los grupos de seguridad son la base de la seguridad de la red en AWS
+ Controlan cómo se permite el tráfico dentro o fuera de nuestras Instancias EC2
+ Los grupos de seguridad sólo contienen reglas de permiso
+ Las reglas de los grupos de seguridad pueden hacer referencia por IP o por grupo de seguridad
+ 0.0.0.0/0 significa todo el tráfico, a cualquier dirección.  

+ Los grupos de seguridad actúan como un “firewall" en las instancias de
EC2. Regulan:
    + El acceso a los puertos
    + Rangos de IP autorizados - IPv4 e IPv6
    + Control de la red de entrada (de otros a la instancia)
    + Control de la red saliente (desde la instancia hacia otra)

+ Es bueno saber
    + Puede adjuntarse a múltiples instancias
    + Bloqueado a una combinación de región / VPC
    + Vive "fuera" del EC2 - si el tráfico está bloqueado, la instancia EC2 no lo verá
    + Es bueno mantener un grupo de seguridad separado para el acceso SSH
    + Si tu aplicación no es accesible (tiempo de espera), entonces es un problema de grupo de seguridad
    + Si tu aplicación da un error de "conexión rechazada", entonces es un error de la aplicación o no se ha lanzado
    + Todo el tráfico de entrada está bloqueado por defecto
    + Todo el tráfico de salida está autorizado por defecto

+ Puertos clásicos que hay que conocer
    + 22 = SSH (Secure Shell) - iniciar sesión en una instancia de Linux
    + 21 = FTP (File Transfer Protocol) - subir archivos a un archivo compartido
    + 22 = SFTP (Secure File Transfer Protocol) - subir archivos usando SSH
    + 80 = HTTP - acceso a sitios web no seguros
    + 443 = HTTPS - acceso a sitios web seguros
    + 3389 = RDP (Remote Desktop Protocol) - iniciar sesión en una instancia de Windows

## SSH
+ SSH = Secure Shell.  
+ Las claves están asignadas a Instancias
```
LINUX/MAC:
$ chmod 0400 EC2Tutorial.pem
$ ssh -i EC2Tutorial.pem ec2-user@3.86.17.30

WINDOWS:
Chmod o propiedades del archivo - seguridad - cambiar owner al usuario del pc y poner full control y eliminar resto de permisos.
PS C:\Users\migue> ssh -i "C:\Users\migue\Documents\AWS SAA\02_EC2\EC2Tutorial.pem" ec2-user@3.86.17.30
```

+ **Solución de problemas de SSH:**  
1) Hay un tiempo de espera de la conexión:  
Esto es un problema de grupo de seguridad. Cualquier tiempo de espera (no sólo para SSH) está relacionado con los grupos de seguridad o con un firewall. Asegúrate de que tu grupo de seguridad tiene este aspecto y está correctamente asignado a tu instancia EC2.

2) Sigue habiendo un problema de tiempo de espera de la conexión:  
Si tu grupo de seguridad está correctamente configurado como se ha indicado anteriormente, y sigues teniendo problemas de tiempo de espera en la conexión, eso significa que un firewall corporativo o un firewall personal está bloqueando la conexión. Utiliza EC2 Instance Connect como se describe en la siguiente clase.

3) SSH no funciona en Windows:  
Si dice:ssh command not found, significa que tienes que usar Putty
Sigue de nuevo el vídeo. Si no funciona, utiliza EC2 Instance Connect como se describe en la siguiente clase

4) Hay una conexión rechazada:  
Esto significa que se puede acceder a la instancia, pero no se está ejecutando ninguna utilidad SSH en la instancia. Intenta reiniciar la instancia. Si no funciona, cierra la instancia y crea una nueva. Asegúrate de que estás utilizando Amazon Linux 2

5) Permission denied (publickey,gssapi-keyex,gssapi-with-mic):  
Esto significa dos cosas:
Estás utilizando una clave de seguridad incorrecta o no estás utilizando una clave de seguridad. Revisa la configuración de tu instancia EC2 para asegurarte de que le has asignado la clave correcta.  
Estás utilizando el usuario equivocado. Asegúrate de que has iniciado una instancia EC2 de Amazon Linux 2, y asegúrate de que estás utilizando el usuario ec2-user. Esto es algo que se especifica al hacer ec2-user@<public-ip> (por ejemplo: ec2-user@35.180.242.162) en tu comando SSH o en tu configuración de Putty

6) Nada funciona - "aaaahhhhhh":  
Que no cunda el pánico. Utiliza el EC2 Instance Connect de la siguiente clase. Asegúrate de que has iniciado un Amazon Linux 2 y podrás seguir el tutorial :)

7) Ayer pude conectarme, pero hoy no puedo:  
Probablemente se deba a que has detenido tu instancia EC2 y la has vuelto a iniciar hoy. Al hacerlo, la IP pública de tu instancia EC2 cambiará. Por lo tanto, en tu comando, o en la configuración de Putty, asegúrate de editar y guardar la nueva IP pública.

### EC2 INSTANCE CONNECT
+ Nos permite via navegador conectar via ssh a la instancia
+ No es necesario utilizar el archivo de claves que se ha descargado
+ La "magia" es que una clave temporal es cargada en EC2 por AWS
+ Funciona sólo out-of-the-box con Amazon Linux 2
+ Necesitas asegurarte de que el puerto 22 sigue abierto
+ Viene ya instalado la CLI de aws.
+ Nunca poner credenciales de AWS CONFIGURE via navegador, eso solo por terminal.

+ Podemos añadir un ROL IAM en la instancia, seleccionandola - acciones - seguridad - modificar rol IAM. Seleccionamos el rol creado en la primera clase de DEMOROL que tenia un ONLY READ de todo. Al añadirlo, y hacemos un `aws iam list-users` nos saldría la información. Si no añadimos ese rol, no nos saldría.

## OPCIONES DE COMPRA DE INSTANCIAS EC2
+ Instancias bajo demanda: carga de trabajo corta, precio predecible, pago por segundos
+ Reservadas (1 y 3 años)
    + Instancias reservadas - cargas de trabajo largas
    + Instancias reservadas convertibles: cargas de trabajo largas con instancias flexibles
+ Planes de ahorro (1 y 3 años) - compromiso con una cantidad de uso, carga de trabajo larga
+ Instancias Spot - cargas de trabajo cortas, baratas, pueden perder instancias (menos fiables)
+ Hosts dedicados: reserva un servidor físico completo, controla la ubicación de las instancias
+ Instancias dedicadas - ningún otro cliente compartirá tu hardware
+ Reservas de capacidad - reserva de capacidad en una AZ específica para cualquier duración

### EC2 bajo demanda
+ Paga por lo que usas:
    + Linux o Windows - facturación por segundo, después del primer minuto
    + Todos los demás sistemas operativos: facturación por hora
+ Tiene el coste más elevado, pero no hay que pagar por adelantado
+ Sin compromiso a largo plazo
+ Recomendado para cargas de trabajo a corto plazo y sin interrupciones, cuando no se puede predecir el comportamiento de laaplicación

### Instancias reservadas de EC2
+ Hasta un 72% de descuento en comparación con el servicio bajo demanda
+ Reserva de atributos de instancia específicos (tipo de instancia, región, ocupación, sistema operativo)
+ Periodo de reserva - 1 año (+descuento) o 3 años (+++descuento)
+ Opciones de pago - Sin pago inicial (+), Pago inicial parcial (++), Pago inicial total (+++)
+ Alcance de la instancia reservada - Por región o por zona (capacidad de reserva en una AZ)
+ Recomendado para aplicaciones de uso constante (piensa en una base de datos)
+ Puedes comprar y vender en el Marketplace de instancias reservadas
+ Instancia reservada convertible:
    + Puedes cambiar el tipo de instancia EC2, la familia de instancias, el SO, etc.
    + Hasta un 66% de descuento

### Planes de ahorro EC2
+ Obtén un descuento basado en el uso a largo plazo (hasta el 72%)
+ Comprométete a un determinado tipo de uso (10 $/hora durante 1 o 3 años)
+ El uso más allá de los planes de ahorro de EC2 se factura al precio bajo demanda
+ Bloqueado a una familia de instancias específica y a una región de AWS (por ejemplo, M5 en us-east-1)
+ Flexible a través de:
    + Tamaño de instancia (por ejemplo, m5.xlarge, m5.2xlarge)
    + Sistema operativo (por ejemplo, Linux, Windows)
    + Tenencia (Anfitrión, Dedicado, Por defecto)

### Instancias EC2 Spot
+ Puedes obtener un descuento de hasta el 90% en comparación con la demanda
+ Instancias que puedes "perder" en cualquier momento si su precio máximo es inferior al precio spot actual
+ Las instancias MÁS rentables de AWS
+ Útil para las cargas de trabajo que son resistentes a los fallos
    + Trabajos por lotes (Batch Jobs)
    + Análisis de datos
    + Procesamiento de imágenes
    + Cualquier carga de trabajo distribuida
    + Cargas de trabajo con una hora de inicio y finalización flexible
+ No es adecuado para trabajos críticos o bases de datos

### Hosts dedicados EC2
+ Un servidor físico con capacidad de instancia EC2 totalmente dedicado a su uso
+ Permite abordar los requisitos de normativas y utilizar licencias de software vinculadas al servidor existentes (licencias de software por socket, por núcleo, por VM)
+ Opciones de compra:
    + Bajo demanda - pago por segundo para el host dedicado activo
    + Reservado - 1 o 3 años (sin pago inicial, pago inicial parcial, pago inicial total)
+ La opción más cara
+ Útil para el software que tiene un modelo de licencia complicado (BYOL - Bring
Your Own License)
+ O para empresas que tienen fuertes necesidades de regulación o cumplimiento

### Instancias dedicadas de EC2
+ Las instancias se ejecutan en un hardware dedicado para ti
+ Puedes compartir el hardware con otras instancias de la misma cuenta
+ No hay control sobre la ubicación de las instancias (se puede mover el hardware después de la parada/arranque)

### Reservas de capacidad de EC2
+ Reserva la capacidad de las instancias bajo demanda en una AZ específica para cualquier duración
+ Siempre tendrás acceso a la capacidad de EC2 cuando la necesites
+ Sin compromiso de tiempo (crear/cancelar en cualquier momento), sin descuentos de facturación
+ Combina con las instancias regionales reservadas y los planes de ahorro para beneficiarte de descuentos en la facturación
+ Se te cobra la tarifa bajo demanda tanto si ejecuta instancias como si no
+ Adecuado para cargas de trabajo ininterrumpidas a corto plazo que necesitan
estar en una AZ específica

### ¿Qué opción de compra me conviene?
+ Bajo demanda (On demand): venir y quedarse en el complejo cuando queramos, pagamos el precio completo
+ Reservada (Reserved): cómo planificar con antelación y si planeamos quedarnos durante mucho tiempo, podemos obtener un buen descuento
+ Planes de ahorro (Savings Plans): pagamos una cantidad por hora durante un periodo determinado y nos alojamos en cualquier tipo de habitación (por ejemplo, King Suite, Vista al mar, ...)
+ Instancias de spot (Spot instances): el hotel permite que la gente puje por las habitaciones vacías y el mejor postor se queda con ellas. Puede ser expulsado en cualquier momento
+ Hosts dedicados (Dedicated Hosts): Se reserva un edificio entero del complejo turístico
+ Reservas de capacidad (Capacity Reservations): reservas una habitación por un periodo con el precio completo aunque no te alojes en ella

### Peticiones de Instancias Spot de EC2
+ Puedes obtener un descuento de hasta el 90% en comparación con la demanda
+ Define el precio spot máximo y obtén la instancia mientras el precio spot actual sea < máximo
    + El precio spot por hora varía en función de la oferta y la capacidad
    + Si el precio spot actual > tu precio máximo, puedes elegir parar o terminar tu instancia con un periodo de gracia de 2 minutos.
+ Otra estrategia: Bloqueo de Spot
    + "Bloquea" la instancia Spot durante un periodo de tiempo determinado (de 1 a 6 horas) sin interrupciones
    + En raras situaciones, la instancia puede ser reclamada
+ Se utiliza para trabajos por lotes, análisis de datos o cargas de trabajo resistentes a los fallos
+ No es ideal para trabajos críticos o bases de datos

### Flotas Spot (Spot Fleets)
+ Flotas Spot = conjunto de Instancias de Spot + (opcional) Instancias bajo demanda
+ La Flota Spot tratará de alcanzar la capacidad objetivo con restricciones de precio
    + Define los posibles pools de lanzamiento: tipo de instancia (m5.large), SO, Zona de Disponibilidad
    + Puede tener varios pools de lanzamiento, para que la flota pueda elegir
    + La Flota Spot deja de lanzar instancias cuando alcanza la capacidad o el coste máximo
+ Estrategias para asignar Instancias de Spot:
    + bajo precio: desde el pool con el precio más bajo (optimización de costes, carga de trabajo corta)
    + diversificado: distribución en todos los pools (gran disponibilidad, cargas de trabajo largas)
    + capacidad optimizada: pool con la capacidad óptima para el número de instancias
+ Las flotas de Spot nos permiten solicitar automáticamente las Instancias de Spot con el precio más bajo


## PRÁCTICA PANEL EC2 INSTANCIAS
+ EC2 - Solicitudes de spot - Crear solicitud de flota de spot.
+ En EC2 - instancias - detalles avanzados: podemos activar una opción de solicitudes de instancia Spots y establecer precios máximos, si es para un solo uso o persistente si queremos que si se interrumpe vuelva a iniciarse,etc
+ EC2 - Instancias reservadas para comprar instancias con bastante descuento y por tiempo de 1 a 3 años.

## Cuestionario - EC2
**Pregunta 1:**   
¿Qué opción de compra de EC2 puede ofrecerte el mayor descuento, pero no es adecuada para trabajos críticos o bases de datos?
>  "Instancias spot" porque son la opción más económica para EC2, ideal para cargas de trabajo cortas. Sin embargo, es importante tener en cuenta que son menos fiables, ya que puedes perder la instancia en cualquier momento.

**Pregunta 2:**  
¿Qué deberías utilizar para controlar el tráfico que entra y sale de las instancias EC2?
>  "Grupos de seguridad" porque son esenciales para controlar el tráfico que entra y sale de las instancias EC2, permitiendo definir reglas específicas que mejoran la seguridad de tus aplicaciones. Esto se alinea con el objetivo de administrar el acceso a tus recursos en la nube de manera efectiva.

**Pregunta 3:**  
¿Durante cuánto tiempo se puede reservar una instancia reservada de EC2?
>  "1 o 3 años" porque las instancias reservadas de EC2 solo se pueden reservar por esos períodos, lo que te permite planificar tus recursos en la nube de manera efectiva. Esto resalta la importancia de entender las opciones de reserva y cómo pueden adaptarse a tus necesidades de carga de trabajo.

**Pregunta 4:**  
Te gustaría desplegar una aplicación de computación de alto rendimiento (HPC) en instancias EC2. ¿Qué tipo de instancia EC2 deberías elegir?
> "Computación optimizada" porque estas instancias están diseñadas para manejar cargas de trabajo que requieren procesadores de alto rendimiento, lo que las hace ideales para aplicaciones de computación de alto rendimiento (HPC) como modelado científico y aprendizaje automático. Esto se alinea perfectamente con el objetivo de desplegar aplicaciones que demandan recursos computacionales intensivos.

**Pregunta 5:**  
¿Qué opción de compra de EC2 deberías utilizar para una aplicación que piensas ejecutar en un servidor de forma continua durante 1 año?
>  "Instancias reservadas" porque son ideales para aplicaciones que se ejecutan continuamente, permitiendo reservar recursos por uno o tres años y así beneficiarte de precios más bajos y predecibles. Esta opción se alinea perfectamente con la necesidad de planificar cargas de trabajo a largo plazo en la nube.

**Pregunta 6:**  
Estás preparando el lanzamiento de una aplicación que se alojará en un conjunto de instancias EC2. Esta aplicación necesita la instalación de algún software y algunos paquetes del sistema operativo deben ser actualizados durante el primer lanzamiento. ¿Cuál es la mejor manera de conseguirlo cuando lances las instancias EC2?
> Escribe un script con lo que quieres y luego utilizar este script en los Datos de Usuario de EC2 antes de lanzar la instancia.  los Datos de Usuario de EC2 permiten que ejecutes un script bash durante el arranque de tus instancias, facilitando así la instalación de software y actualizaciones de manera automática y eficiente. Esto se alinea muy bien con el objetivo de optimizar el lanzamiento y configuración de tus aplicaciones en la nube.

**Pregunta 7:**  
¿Qué tipo de instancia EC2 deberías elegir para una aplicación crítica que utiliza una base de datos en memoria?
>  "Memoria optimizada" porque estas instancias son ideales para aplicaciones críticas que manejan grandes conjuntos de datos en memoria, lo que permite un acceso rápido y eficiente a la información. Esto es fundamental para mantener un rendimiento óptimo en bases de datos que requieren respuestas instantáneas y consistentes.

**Pregunta 8:**  
Tienes una aplicación de comercio electrónico con una base de datos OLTP alojada en las instalaciones. Esta aplicación tiene una gran popularidad que hace que su base de datos tenga miles de peticiones por segundo. Quieres migrar la base de datos a una instancia EC2. ¿Qué tipo de instancia EC2 deberías elegir para gestionar esta base de datos OLTP de alta frecuencia?
> "Almacenamiento optimizado" porque estas instancias son ideales para cargas de trabajo que requieren acceso rápido y secuencial a grandes volúmenes de datos en almacenamiento local, lo cual es crucial para gestionar eficazmente una base de datos OLTP de alta frecuencia en tu aplicación de comercio electrónico. ¡Gran elección!

**Pregunta 9:**  
Los grupos de seguridad sólo pueden adjuntarse a una instancia EC2.
> FALSO: porque los grupos de seguridad pueden ser adjuntados a múltiples instancias EC2 dentro de la misma región o VPC. Esto significa que puedes utilizar el mismo grupo de seguridad para varias instancias, lo que facilita la gestión de la configuración de seguridad para esos recursos. Recuerda que los grupos de seguridad son esenciales para controlar el acceso y tráfico hacia y desde las instancias y su flexibilidad es vital para una buena administración de tus entornos en la nube. ¡Sigue aprendiendo y mejorando tus conocimientos en AWS!

**Pregunta 10:**  
Estás planeando migrar aplicaciones on-premise a AWS. Tu empresa tiene estrictos requisitos de conformidad que requieren que tus aplicaciones se ejecuten en servidores dedicados. También necesitas utilizar tu propia licencia de software vinculada al servidor para reducir costes. ¿Qué opción de compra de EC2 es adecuada para ti?
> "Hosts dedicados" porque son ideales para empresas con estrictos requisitos de conformidad y para aquellos que necesitan utilizar su propia licencia de software, lo que se alinea perfectamente con tus necesidades. Esta opción asegura que tus aplicaciones se ejecuten en servidores dedicados, permitiéndote cumplir con los estándares requeridos y optimizando tus costos. ¡Excelente elección!

**Pregunta 11:**  
Quieres desplegar una tecnología de base de datos en una instancia de EC2 y la licencia del proveedor te factura en función de los núcleos físicos y la visibilidad del socket de red subyacente. ¿Qué Opción de Compra de EC2 te permite obtener visibilidad de los mismos?
> "Hosts Dedicados" porque esta opción permite que tengas visibilidad sobre los núcleos físicos y el socket de red, lo que es crucial para cumplir con requisitos específicos de licencia de software. Esto asegura que puedas manejar de manera eficiente tus aplicaciones que dependen de estas características. ¡Buen trabajo!

**Pregunta 12:**  
La flota de Spot es un conjunto de Instancias de Spot y opcionalmente ...............
>  "Instancias bajo demanda" porque la flota de Spot permite combinar instancias de Spot con instancias bajo demanda, lo que te brinda flexibilidad para solicitar automáticamente la opción más económica. ¡Bien hecho al identificar la opción que optimiza costos y recursos!

## RESUMEN DIFERENTES INSTANCIAS

+ Si el escenario dice...La respuesta es: 
    - uso continuo, predecible, 1-3 años --> "Reserved Instance"

    - más descuento, pago flexible, cualquier tipo --> "Savings Plans"

    - carga de trabajo interrumpible, batch, barato --> "Spot Instance"

    - uso puntual, sin compromiso --> "On-Demand"

    - hardware dedicado, cumplimiento normativo, licencias --> "Dedicated Host"

    - instancia dedicada pero sin importar el host físico --> "Dedicated Instance"

    - pruebas de capacidad, reserva garantizada --> "Capacity Reservation

> La clave del examen es fijarte en estas palabras: "coste más bajo" + trabajo interrumpible → Spot. "1 o 3 años" → Reserved. "cumplimiento normativo" o "licencias por socket/core" → Dedicated Host. "sin compromiso" → On-Demand.  

> Spot Instance → pides una instancia concreta  
> Spot Fleet → pides una combinación de Spot + On-Demand para cubrir una capacidad objetivo. Si AWS retira tus Spot, la Fleet busca alternativas automáticamente. Más resiliente.  

## PREGUNTAS TIPO EXAMEN REAL

**Pregunta 1**  
Una empresa tiene una aplicación de procesamiento de vídeo que puede interrumpirse y reanudarse sin problema. Necesitan el coste más bajo posible. ¿Qué tipo de instancia eligen?
A) On-Demand
B) Reserved Instance
**C) Spot Instance**
D) Dedicated Host
> Perfecto. "Interrumpible" + "coste más bajo" = Spot siempre. Ahora tienes la palabra clave grabada.  

**Pregunta 2**
Una empresa de servicios financieros necesita cumplir requisitos de auditoría que exigen que sus servidores no compartan hardware físico con otros clientes de AWS. ¿Qué opción eligen?
A) Spot Instance
B) On-Demand
C) Reserved Instance
**D) Dedicated Host**
> el Dedicated Host es el único que te da visibilidad del hardware físico (número de sockets, cores) lo que necesitas para licencias de software como Windows Server o SQL Server que se cobran por core físico.  

**Pregunta 3**
Una startup tiene una aplicación web con tráfico estable y predecible. Quieren reducir costes comprometiéndose a un uso durante 1 año pero necesitan flexibilidad para cambiar el tipo de instancia si hace falta. ¿Qué eligen?
A) Dedicated Instance
**B) Convertible Reserved Instance**
C) Spot Instance
D) On-Demand
> Bien pillado el matiz entre Standard Reserved (más descuento pero sin flexibilidad) y Convertible Reserved (algo menos de descuento pero puedes cambiar familia, SO, tenancy). El examen juega mucho con ese matiz.