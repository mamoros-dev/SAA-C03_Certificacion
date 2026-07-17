# Desacoplamiento en AWS

## Introducción
El desacoplamiento permite que los componentes de una arquitectura de software funcionen de manera independiente, mejorando la escalabilidad, confiabilidad y mantenibilidad de las aplicaciones.

---

## SQS (Simple Queue Service)

**Descripción:** Servicio de colas de mensajes completamente gestionado que permite desacoplar y escalar microservicios, sistemas distribuidos y aplicaciones sin servidor.

### Características principales:
- **Colas estándar:** Entrega de mensajes de máximo esfuerzo con latencia muy baja
- **FIFO (First-In-First-Out):** Garantiza el orden exacto de los mensajes
- **Escalabilidad automática:** Maneja millones de mensajes por segundo
- **Reintento automático:** Manejo de fallos con reintentos configurables
- **Visibilidad de mensajes:** Tiempo de espera antes de que un mensaje esté disponible nuevamente

### Casos de uso:
- Desacoplamiento de productores y consumidores
- Procesamiento asincrónico de tareas
- Regulación de carga entre componentes

---

## SNS (Simple Notification Service)

**Descripción:** Servicio de notificación completamente gestionado que utiliza un modelo de publicador-suscriptor para enviar mensajes a múltiples suscriptores.

### Características principales:
- **Modelo Pub/Sub:** Un publicador envía un mensaje a muchos suscriptores
- **Múltiples protocolos:** Email, SMS, HTTP, SQS, Lambda, etc.
- **Filtrado de mensajes:** Los suscriptores pueden filtrar mensajes según atributos
- **Entrega confiable:** Reintentos automáticos y manejo de fallos

### Casos de uso:
- Notificaciones en tiempo real
- Alertas y monitoreo
- Distribución de eventos a múltiples servicios
- Integración con SQS para procesamiento asincrónico

---

## Kinesis

**Descripción:** Plataforma de transmisión de datos completamente gestionada que permite capturar, procesar y almacenar flujos de datos en tiempo real.

### Componentes:

#### Kinesis Data Streams:
- Transmisión de datos en tiempo real
- Procesamiento de datos con latencia baja
- Particiones (shards) para escalabilidad
- Retención configurable (24 horas - 365 días)

#### Kinesis Data Firehose:
- Entrega automática de datos a destinos (S3, Redshift, Elasticsearch)
- Buffering y compresión de datos
- Sin gestión de shards

#### Kinesis Data Analytics:
- Procesamiento y análisis de datos en tiempo real usando SQL

### Características principales:
- **Escalabilidad:** Procesamiento de millones de registros por segundo
- **Baja latencia:** Procesamiento en tiempo real
- **Durabilidad:** Replicación automática entre zonas de disponibilidad

### Casos de uso:
- Análisis de logs en tiempo real
- Monitoreo de métricas de aplicaciones
- Procesamiento de eventos de IoT
- Análisis de comportamiento de usuarios

---

## Amazon MQ

**Descripción:** Servicio de mensajería completamente gestionado para Apache ActiveMQ y RabbitMQ que facilita la migración de aplicaciones on-premises.

### Características principales:
- **Compatibilidad:** Soporte nativo para ActiveMQ y RabbitMQ
- **Colas y temas:** Modelo de mensajería flexible
- **Alta disponibilidad:** Implementación de múltiples zonas de disponibilidad
- **Seguridad:** Encriptación en tránsito y en reposo
- **Integración fácil:** Compatible con aplicaciones existentes

### Casos de uso:
- Migración de sistemas legacy con middleware de mensajes
- Aplicaciones que requieren protocolos específicos (OpenWire, AMQP, MQTT)
- Arquitecturas que necesitan colas y temas avanzados

---

## Comparación de servicios

| Característica | SQS | SNS | Kinesis | Amazon MQ |
|---|---|---|---|---|
| **Tipo** | Cola de mensajes | Pub/Sub | Stream de datos | Middleware tradicional |
| **Patrón** | Consumidor única cola | Un publicador, muchos suscriptores | Procesamiento en tiempo real | Colas y temas avanzados |
| **Latencia** | Baja-Media | Muy baja | Muy baja | Media |
| **Escalabilidad** | Automática | Automática | Manual (Shards) | Manual |
| **Retención** | Corta (14 días máx) | No almacena | Larga (24h - 365d) | Configurable |
| **Caso ideal** | Desacoplamiento simple | Notificaciones | Big Data, IoT | Sistemas legacy |

---

## Arquitectura típica de desacoplamiento

```
Aplicación 1 → SQS → Aplicación 2 (procesamiento)
Aplicación 1 → SNS → {Email, SMS, Lambda, SQS, HTTP}
IoT Devices → Kinesis → Análisis y visualización
Legacy App → Amazon MQ → Nuevos microservicios
```

## RESUMEN

+ ¿Qué significa "desacoplar" en la vida real?
    - Imagina una tienda online. El usuario hace un pedido → el sistema tiene que: guardar el pedido, cobrar la tarjeta, enviar email de confirmación, avisar al almacén, actualizar el stock y generar la factura.  
    - Sin desacoplamiento: todo ocurre en cadena. Si el sistema de email falla, el pedido entero falla. Si hay 10.000 pedidos a la vez, el sistema colapsa.  
    - Con desacoplamiento: el usuario hace el pedido → se mete en una cola → cada sistema (cobro, email, almacén) lo procesa a su ritmo de forma independiente. Si el email falla, el cobro no se ve afectado. Si hay picos, la cola absorbe la carga.  
    - Eso es exactamente lo que hacen SQS, SNS y Kinesis.

+ SQS — cola de tareas asíncronas. Casos reales:
    - Tienda online procesa pedidos — cada pedido entra en cola y se procesa uno a uno
    - App de edición de vídeo — el usuario sube un vídeo, entra en cola y se procesa cuando hay capacidad
    - Sistema de emails masivos — los emails se encolan y se envían sin saturar el servidor
> Palabra clave examen: "desacoplar", "procesar tareas asíncronas", "absorber picos de carga" → SQS.  

+ SQS FIFO vs Estándar:
    - Estándar → orden no garantizado, máximo rendimiento. Para la mayoría de casos.
    - FIFO → orden garantizado, más lento. Para cuando el orden importa — transacciones bancarias, comandas de restaurante.

+ SNS — notificaciones a múltiples destinos a la vez. Casos reales:
    - Sistema de alertas — si un servidor cae, SNS notifica simultáneamente a email, Slack y SMS del equipo
    - E-commerce — cuando llega un pedido, SNS avisa al mismo tiempo al sistema de cobro, almacén y email
    - App móvil — push notifications a miles de usuarios a la vez
> Palabra clave examen: "notificar a múltiples sistemas a la vez", "pub/sub", "fan-out" → SNS.  

+ El patrón SNS + SQS Fan-Out — muy frecuente en el examen:
    - SNS recibe un evento → lo manda simultáneamente a varias colas SQS → cada cola lo procesa independientemente. Ejemplo: pedido nuevo → SNS → cola de cobro + cola de email + cola de almacén. Cada uno procesa a su ritmo sin bloquearse entre sí.

+ Kinesis — streaming de datos en tiempo real. Casos reales:
    - App de IoT — miles de sensores mandando datos continuamente que hay que analizar en tiempo real
    - Análisis de logs — servidores generando millones de logs por segundo
    - Detección de fraude — analizar transacciones bancarias en tiempo real
    - Clickstream — analizar el comportamiento de usuarios en una web en tiempo real
> Palabra clave examen: "tiempo real", "streaming", "IoT", "logs en tiempo real", "análisis en tiempo real" → Kinesis.

+ Kinesis Data Streams vs Firehose:
    - Data Streams → tú controlas el procesamiento, tiempo real puro, más complejo
    - Firehose → totalmente gestionado, carga datos directamente en S3/Redshift/Elasticsearch, más sencillo

## PREGUNTAS TIPO EXAMEN
**Pregunta 1**: Una app de edición de fotos permite a usuarios subir imágenes que deben procesarse (redimensionar, añadir filtros). El procesamiento puede tardar minutos y no debe bloquear al usuario. En picos hay miles de imágenes simultáneas. ¿Qué servicio usan?  
A) SNS  
B) Kinesis  
**C) SQS**  
D) EventBridge  
> C) SQS: "No bloquear al usuario" + "picos de carga" + "procesamiento asíncrono" = SQS. Además este es un caso real muy común — cualquier app que procesa archivos (vídeos, fotos, documentos) usa este patrón. El usuario sube, recibe confirmación inmediata y SQS gestiona el procesamiento en segundo plano.

**Pregunta 2**: Un sistema de monitorización necesita que cuando una alarma se active, se envíe simultáneamente un email al equipo, un SMS al manager y se cree un ticket en el sistema de incidencias. ¿Qué servicio usan?  
A) SQS FIFO  
**B) SNS**  
C) Kinesis Data Streams  
D) SQS Estándar  
>  B) SNS: "Simultáneamente" + "múltiples destinos distintos" = SNS siempre. La clave es que con SQS solo hay un consumidor procesando — con SNS todos reciben el mensaje a la vez.

**Pregunta 3**: Una empresa de logística tiene 50.000 sensores GPS en camiones enviando su posición cada segundo. Necesitan procesar y analizar esa ubicación en tiempo real para optimizar rutas. ¿Qué servicio usan?  
A) SQS  
B) SNS  
C) SQS FIFO  
**D) Kinesis Data Streams**  
>  D) Kinesis Data Streams: "50.000 sensores" + "cada segundo" + "tiempo real" = Kinesis. SQS no está diseñado para streaming continuo de datos — está diseñado para tareas discretas. Kinesis es exactamente para flujos continuos de datos en tiempo real.

**Pregunta 4**: Un banco necesita procesar transferencias bancarias garantizando que se procesan exactamente en el orden en que se reciben. ¿Qué servicio usan?  
A) SQS Estándar  
B) SNS  
**C) SQS FIFO**  
D) Kinesis Firehose  
> C) SQS FIFO: "Orden garantizado" = FIFO siempre. El estándar no garantiza orden — un mensaje enviado primero puede llegar después. En transacciones bancarias eso sería un desastre.

**Pregunta 5**: Una empresa quiere que cuando llegue un pedido nuevo, el sistema de cobro, el almacén y el sistema de email lo procesen de forma independiente y a su propio ritmo. ¿Qué patrón usan?  
A) SQS Estándar con un consumidor  
**B) SNS + SQS Fan-Out**  
C) Kinesis Data Firehose  
D) SQS FIFO  
> B) SNS + SQS Fan-Out: Has identificado el patrón Fan-Out correctamente. Es uno de los patrones de arquitectura más usados en empresas reales y muy frecuente en el examen. SNS recibe uno, distribuye a muchos, cada SQS procesa independientemente.
