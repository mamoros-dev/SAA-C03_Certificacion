# Serverless en AWS

+ El modelo serverless permite ejecutar aplicaciones y procesos sin administrar servidores físicos ni VMs. En AWS, el proveedor se encarga de la infraestructura, el escalado y la disponibilidad, mientras el equipo de desarrollo se enfoca en el código.

+ No significa que no haya servidores — significa que tú no los gestionas. AWS los crea, los escala y los elimina automáticamente. Solo pagas cuando tu código se ejecuta, no por tener un servidor encendido 24h.

## Conceptos clave

- Serverless no significa "sin servidores", sino "sin administrar servidores".
- Se paga solo por el tiempo de ejecución real y por los recursos consumidos.
- Escala automáticamente según la carga.

### Servicios principales de AWS para serverless

- AWS Lambda: ejecuta funciones en respuesta a eventos.
- Amazon API Gateway: expone APIs REST o WebSocket.
- Amazon S3: almacenamiento de objetos y eventos de cambios.
- Amazon DynamoDB: base de datos NoSQL con escalado automático.
- Amazon EventBridge: orquestación y integración basada en eventos.
- AWS Step Functions: coordina flujos de trabajo distribuidos.
- AWS SAM y Serverless Framework: herramientas para definir y desplegar aplicaciones serverless.

### Ejemplo típico

Una aplicación web puede tener:

1. Un frontend estático en S3.
2. Un API Gateway que recibe peticiones.
3. Funciones Lambda que procesan la lógica de negocio.
4. DynamoDB para almacenar datos.
5. EventBridge o Step Functions para integrar procesos adicionales.

### Ventajas

- Menor complejidad operativa.
- Escalado automático.
- Pago por uso.
- Mayor rapidez para desarrollar y desplegar.

### Limitaciones y consideraciones

- Latencia inicial o cold start en Lambda.
- Tiempo de ejecución y límites de recursos.
- Diseño orientado a eventos y funciones pequeñas.
- Monitoreo, logging y seguridad deben configurarse adecuadamente.

## Resumen

+ AWS ofrece una gran cantidad de servicios serverless que permiten construir aplicaciones modernas, escalables y de bajo mantenimiento, ideales para APIs, procesamiento de eventos, automatización y microservicios.

+ ¿Cuándo mete una empresa serverless en su arquitectura?
    - Caso real 1 — Procesamiento de eventos puntuales: una tienda online sube una foto de producto → Lambda la redimensiona automáticamente a varios tamaños → las guarda en S3. No tiene sentido tener un servidor EC2 encendido 24h solo para redimensionar fotos que llegan de vez en cuando.
    - Caso real 2 — API backend de una app móvil: la app del móvil hace peticiones → API Gateway las recibe → llama a Lambda → Lambda consulta DynamoDB → devuelve el resultado. Sin servidores que gestionar, escala automáticamente con los usuarios.
    - Caso real 3 — Automatización interna: cada noche a las 2am Lambda ejecuta un script que genera informes, limpia datos antiguos o envía emails. Sin serverless necesitarías un servidor encendido solo para eso.
    - Caso real 4 — Webhooks y integraciones: cuando un cliente paga en Stripe, Stripe llama a tu API Gateway → Lambda procesa el pago y actualiza la base de datos. Escala automáticamente aunque lleguen 10.000 pagos a la vez.

+ Los servicios serverless principales y cuándo usarlos:
    - Lambda — la función que se ejecuta sin servidor
        - Piénsalo como una función que duerme hasta que algo la despierta. Ese algo puede ser: una petición HTTP, un archivo subido a S3, un mensaje en SQS, un evento de DynamoDB, un cron job programado.
        - Límites importantes que el examen pregunta:
            - Máximo 15 minutos de ejecución. Si necesitas más → usa ECS/EC2
            - Máximo 10GB de RAM
            - Máximo 50MB de código (250MB con capas)
    > Palabra clave examen: "sin servidor", "evento puntual", "escala automática", "pago por uso" → Lambda.

    - Lambda@Edge — Lambda pero en los edge locations de CloudFront
        - Ejecuta código más cerca del usuario geográficamente. Casos reales:
            - Personalizar respuestas HTTP según el país del usuario
            - Autenticar usuarios antes de que lleguen al origen
            - Redireccionar URLs dinámicamente
    > Palabra clave: "ejecutar código en el edge", "personalizar respuesta CloudFront" → Lambda@Edge.

    - DynamoDB — base de datos NoSQL serverless
        - Ya lo viste brevemente antes. Lo importante para serverless es que DynamoDB escala automáticamente, no tiene servidor que gestionar y responde en milisegundos.
        - Cuándo usarlo en empresa:
            - App móvil con millones de usuarios — escala sin límite
            - Carrito de compra — estructura flexible, acceso rapidísimo
            - Sesiones de usuario — clave-valor simple y rápido
            - Gaming — rankings, estado del juego en tiempo real
        - Características avanzadas que pregunta el examen:
            - DynamoDB Streams → captura cambios en la tabla en tiempo real. Útil para triggear Lambda cuando se modifica un dato
            - DAX → caché en memoria para DynamoDB. Reduce latencia de milisegundos a microsegundos
            - Global Tables → replica la tabla en múltiples regiones automáticamente para acceso global
    > Palabra clave: "NoSQL", "serverless", "escala automática", "milisegundos", "clave-valor" → DynamoDB. Si además dice "caché" → DAX.

    - API Gateway — la puerta de entrada a tu backend serverless
        - Es el servicio que recibe las peticiones HTTP/REST/WebSocket de los clientes y las manda a Lambda, EC2 o cualquier backend. En arquitecturas serverless siempre aparece delante de Lambda.
        - Casos reales:
            - App móvil hace GET /productos → API Gateway → Lambda → DynamoDB → respuesta
            - WebSocket para chat en tiempo real → API Gateway gestiona las conexiones
    > Palabra clave: "API REST", "HTTP endpoint", "frontend de Lambda" → API Gateway.

    - Step Functions — orquestar varios Lambdas en secuencia
        - Cuando tienes un proceso que requiere varios pasos en orden — Lambda 1 valida, Lambda 2 procesa, Lambda 3 notifica — Step Functions los coordina y gestiona errores entre pasos.
        - Caso real: proceso de onboarding de usuario → verificar email → crear cuenta → enviar bienvenida → asignar plan. Cada paso es una Lambda coordinada por Step Functions.
    > Palabra clave: "orquestar", "flujo de trabajo", "varios pasos en secuencia" → Step Functions.

+ La arquitectura serverless típica de empresa:
```
App móvil/web
      ↓
API Gateway  ←→  Cognito (autenticación)
      ↓
   Lambda
      ↓
DynamoDB / S3 / RDS
      ↓
SNS/SES (notificaciones)
```  

## PREGUNTAS TIPO EXAMEN

**Pregunta 1**: Una startup quiere construir una API REST para su app móvil sin gestionar servidores. La API tendrá picos de tráfico impredecibles. ¿Qué combinación de servicios eligen?  
A) EC2 + RDS + ELB  
**B) API Gateway + Lambda + DynamoDB**  
C) ECS + Aurora + ALB  
D) Elastic Beanstalk + MySQL  
> B) API Gateway + Lambda + DynamoDB:  Es la arquitectura serverless más clásica del examen. API Gateway + Lambda + DynamoDB aparece en múltiples preguntas con variaciones. Memoriza este combo porque es la respuesta correcta siempre que veas "API sin servidores + NoSQL + picos impredecibles".

**Pregunta 2**: Una función Lambda necesita procesar archivos de vídeo grandes que tardan 20 minutos en procesarse. ¿Qué problema tiene esta arquitectura?  
A) Lambda no puede acceder a S3  
**B) Lambda tiene un límite máximo de 15 minutos de ejecución**  
C) Lambda no soporta procesamiento de vídeo  
D) DynamoDB no puede almacenar vídeos  
> B) Lambda tiene un límite máximo de 15 minutos de ejecución: El límite de 15 minutos es uno de los datos más preguntados de Lambda. Cuando el examen ponga un proceso que tarde más de 15 minutos — transcodificación de vídeo, ML, ETL largo — Lambda no es la respuesta. La alternativa sería ECS con Fargate o AWS Batch.

**Pregunta 3**: Una empresa tiene una tabla DynamoDB con millones de registros. Quieren reducir la latencia de las consultas de milisegundos a microsegundos para su aplicación de trading en tiempo real. ¿Qué añaden?  
A) ElastiCache Redis  
B) RDS Read Replica  
**C) DynamoDB DAX**  
D) DynamoDB Streams  
> C) DynamoDB DAX: Bien diferenciado DAX de ElastiCache. Ambos son caché pero DAX es específico de DynamoDB — está integrado nativamente y no requiere cambios en el código de la aplicación. ElastiCache es para RDS y otras fuentes. Cuando veas DynamoDB + caché → DAX siempre.

**Pregunta 4**: Cuando un usuario sube una foto a S3, la empresa quiere automáticamente redimensionarla a 3 tamaños distintos y guardarlos en otro bucket. ¿Qué arquitectura usan?  
A) EC2 con cron job que revisa el bucket cada minuto  
**B) S3 trigger → Lambda → guardar en S3 destino**  
C) ECS con Fargate procesando el bucket  
D) Step Functions leyendo S3 directamente  
> B) S3 trigger → Lambda → guardar en S3 destino: Este patrón S3 trigger → Lambda es el más usado en procesamiento de archivos en empresas reales. Cada vez que algo llega a S3, Lambda reacciona automáticamente. Sin servidores, sin polling, sin coste cuando no hay actividad.

**Pregunta 5**: Una empresa necesita personalizar las respuestas de CloudFront según el tipo de dispositivo del usuario (móvil vs escritorio) para servir contenido optimizado. ¿Qué servicio usan?  
A) Lambda en una VPC privada  
B) API Gateway + Lambda  
**C) Lambda@Edge**  
D) CloudFront Functions con ECS  
> c) Lambda@Edge: es exactamente para ejecutar lógica en el edge de CloudFront antes de que la respuesta llegue al usuario. Detectar dispositivo, redirigir por idioma, autenticar — todo sin que la petición viaje hasta el origen.

**Pregunta 6**: Un proceso de pedido online requiere estos pasos en orden: validar stock → cobrar tarjeta → actualizar inventario → enviar confirmación. Si un paso falla, debe reintentarse o compensarse. ¿Qué servicio orquesta esto?  
A) SQS FIFO  
B) SNS Fan-Out  
**C) Step Functions**  
D) EventBridge  
> C) Step Functions: La clave era "pasos en orden" + "si falla reintentarse" — eso es orquestación y Step Functions es el servicio exacto. SQS FIFO garantiza orden pero no gestiona flujos con lógica condicional ni reintentos entre pasos.