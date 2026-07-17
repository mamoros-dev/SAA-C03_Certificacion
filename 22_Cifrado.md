# CIFRADO Y SEGURIDAD

+ En cualquier empresa que uses AWS, la seguridad no es opcional. Los datos de clientes, contraseñas, certificados SSL, claves de API — todo eso necesita protección. Estos servicios son los que un Cloud Engineer configura y mantiene para que nada quede expuesto.

## Cifrado de datos: KMS
+ KMS (Key Management Service) es el servicio que gestiona las claves de cifrado en AWS. Cuando cifras un bucket S3, un volumen EBS o una base de datos RDS, por debajo AWS usa KMS para gestionar las claves.
+ Caso real: una empresa de salud almacena historiales médicos en S3. Por ley deben estar cifrados. Activan cifrado con KMS y AWS gestiona automáticamente las claves — la empresa no tiene que preocuparse de guardarlas manualmente.
+ Puntos clave del examen:
    - CMK (Customer Managed Key) → tú controlas la clave, la rotas, la eliminas
    - AWS Managed Key → AWS la gestiona automáticamente, menos control
    - KMS Multi-Region → misma clave en múltiples regiones. Útil para replicación de datos cifrados entre regiones
> Palabra clave: "cifrar datos", "gestionar claves", "cumplimiento normativo" → KMS.

## Guardar secretos y configuración: SSM Parameter Store vs Secrets Manager
+ Ambos guardan información sensible como contraseñas, claves de API o cadenas de conexión. La diferencia:

+ SSM Parameter Store:
    - Para qué: Configuración y secretos simples
    - Rotación automática: No
    - Coste: Gratis (tier estándar)
    - Integración: Lambda, EC2, ECS
> Caso real SSM: una Lambda necesita la URL de una API externa. En vez de hardcodearla en el código, la guarda en SSM Parameter Store y la Lambda la lee al ejecutarse. Si la URL cambia, solo cambias el parámetro sin tocar el código.  

+ Secrets Manager:
    - Para qué: Secretos con rotación automática
    - Rotación automática: Sí
    - Coste: De pago
    - Integración: RDS, Redshift, Lambda
> Caso real Secrets Manager: una aplicación conecta con RDS. Secrets Manager guarda la contraseña y la rota automáticamente cada 30 días sin que nadie tenga que hacerlo manualmente.  

+ Palabra clave: "rotar contraseñas automáticamente" → Secrets Manager. "guardar configuración o secretos simples" → SSM Parameter Store.

## Certificados SSL: ACM
+ ACM (AWS Certificate Manager) gestiona los certificados SSL/TLS que hacen que tu web sea HTTPS. Los crea, renueva automáticamente y los despliega en ALB, CloudFront o API Gateway.
+ Caso real: una empresa tiene una web en ALB. Con ACM crean un certificado SSL gratuito para su dominio y lo asocian al ALB. ACM lo renueva automáticamente antes de que expire — sin intervención manual.
> Palabra clave: "certificado SSL", "HTTPS", "renovación automática" → ACM.

## Protección contra ataques: WAF, Shield, Firewall Manager

+ WAF:
    - Protege Contra: Ataques capa 7 — SQL injection, XSS, bots
    - Caso Real: Bloquear peticiones maliciosas a tu web
+ Shield Standard:
    - Protege Contra: DDoS básico
    - Caso Real: Gratis, activo automáticamente en todos los recursos
+ Shield Advanced:
    - Protege Contra: DDoS sofisticado + soporte 24/7
    - Caso Real: Empresas críticas que no pueden permitirse caídas
+ Firewall Manager:
    - Protege Contra: Gestionar WAF y Shield en múltiples cuentas
    - Caso Real: Empresa con 20 cuentas AWS que quiere reglas WAF centralizadas

+ Caso real WAF: una web de e-commerce recibe ataques de bots que intentan hacer scraping de precios. WAF detecta el patrón y bloquea esas IPs automáticamente.

+ Palabras clave:
    - "SQL injection", "XSS", "bloquear IPs", "capa 7" → WAF
    - "DDoS" → Shield
    - "gestionar reglas de seguridad en múltiples cuentas" → Firewall Manager


## Detección de amenazas: GuardDuty, Inspector, Macie

+ GuardDuty:
    - Qué detecta: Amenazas en tu cuenta AWS — accesos sospechosos, cryptomining, IPs maliciosas
    - Caso real: Alguien intenta acceder a tu cuenta desde un país inusual a las 3am
+ Inspector:
    - Qué detecta: Vulnerabilidades en EC2, Lambda y contenedores
    - Caso real: Una instancia EC2 tiene una versión de OpenSSL con vulnerabilidad conocida
+ Macie:
    - Qué detecta: Datos sensibles expuestos en S3 — tarjetas de crédito, DNIs, emails
    - Caso real: Un bucket S3 contiene datos personales de clientes sin cifrar

+ Palabras clave:
    - "amenazas en la cuenta", "actividad sospechosa", "cryptomining" → GuardDuty
    - "vulnerabilidades en instancias", "CVEs", "seguridad de contenedores" → Inspector
    - "datos sensibles en S3", "PII", "GDPR", "tarjetas de crédito expuestas" → Macie

## USO EN EMPRESAS
+ En cualquier empresa que maneje datos de clientes:
    - KMS está activo en prácticamente todos los recursos
    - Secrets Manager gestiona todas las contraseñas de bases de datos
    - ACM gestiona todos los certificados SSL
    - GuardDuty está activo en todas las cuentas como primera línea de defensa
    - WAF protege todas las webs públicas
> No es algo avanzado y puntual — es infraestructura de seguridad básica que todo Cloud Engineer configura y mantiene.  

## PREGUNTAS TIPO EXAMEN
**Pregunta 1:** Una aplicación en EC2 necesita conectarse a RDS. La contraseña de la base de datos debe rotarse automáticamente cada 30 días sin modificar el código de la aplicación. ¿Qué servicio usan?  
A) SSM Parameter Store  
B) KMS  
**C) Secrets Manager**  
D) ACM  
> C) Secrets Manager: "Rotación automática" es la palabra que diferencia Secrets Manager de SSM Parameter Store. SSM es más barato pero no rota. Secrets Manager es de pago precisamente por esa función de rotación automática integrada con RDS.  

**Pregunta 2:** Una web de banca online necesita protegerse contra ataques de SQL injection y cross-site scripting en su API pública. ¿Qué servicio configura?  
A) Shield Advanced  
B) GuardDuty  
**C) WAF**  
D) Inspector  
> C) WAF: "SQL injection" y "XSS" son ataques de capa 7 — capa de aplicación HTTP. WAF opera exactamente en esa capa. Shield protege contra DDoS volumétrico (capa 3/4), no contra ataques de aplicación.

**Pregunta 3:** El equipo de seguridad quiere detectar automáticamente si hay datos de tarjetas de crédito de clientes almacenados sin cifrar en buckets S3. ¿Qué servicio usan?  
A) GuardDuty  
B) Inspector  
C) CloudTrail  
**D) Macie**  
> D)  Macie es especialista en encontrar datos sensibles en S3 — PII, tarjetas de crédito, pasaportes, DNIs. GuardDuty detecta comportamiento sospechoso en la cuenta pero no analiza el contenido de los archivos en S3.

**Pregunta 4:** Una empresa detecta que sus instancias EC2 están siendo usadas para minar criptomonedas sin autorización y que hay conexiones desde IPs maliciosas conocidas. ¿Qué servicio habría detectado esto?  
A) Inspector  
B) Macie  
**C) GuardDuty**  
D) WAF  
> C) GUardDuty:  Cryptomining + IPs maliciosas = GuardDuty siempre. GuardDuty analiza los logs de VPC Flow Logs, CloudTrail y DNS para detectar patrones de comportamiento anómalo. Inspector busca vulnerabilidades de software, no comportamiento malicioso en tiempo real.

**Pregunta 5:** Una empresa tiene un ALB con una web HTTPS. El certificado SSL actual expira en 30 días y quieren uno que se renueve automáticamente sin intervención manual. ¿Qué servicio usan?  
A) KMS  
**B) ACM**  
C) Secrets Manager  
D) WAF  
> B) ACM + renovación automática es una de las ventajas más importantes del servicio. Los certificados de ACM para recursos AWS (ALB, CloudFront, API Gateway) son además gratuitos y se renuevan solos antes de expirar.

**Pregunta 6:** Una empresa con 15 cuentas AWS quiere aplicar las mismas reglas WAF a todas las cuentas desde un punto centralizado. ¿Qué servicio usan?  
A) WAF directamente en cada cuenta  
B) Shield Advanced  
C) AWS Organizations  
**D) Firewall Manager**  
> D) Firewall Manager es el servicio de gestión centralizada de seguridad para múltiples cuentas. Trabaja encima de Organizations para aplicar WAF, Shield y Security Groups de forma consistente en toda la organización.
