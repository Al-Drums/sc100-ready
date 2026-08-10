# SIMULACRO SC-100 #2 — 25 preguntas

**Tiempo objetivo: 50 minutos.**
No mires el solucionario hasta terminar. Apunta también las que dudes aunque aciertes.

**Ponderación de este simulacro** (distinta a la del #1, según tus fallos recientes):
- Conditional Access, session control y las tres detecciones de identidad → **6** (los fallos previos)
- Ransomware: secuencias DART y backup → **3** (los fallos previos)
- Red: Firewall, WAF, Global Secure Access, conectividad híbrida → **6** (gaps declarados)
- Purview: Information Barriers, DLP, etiquetas → **3** (pedido)
- Intune y endpoint → **2** (pedido)
- Cifrado → **3**
- Gobernanza y SecOps → **2**

---

## PREGUNTAS

---

**1.** Tu empresa publica varias aplicaciones como enterprise applications en Microsoft Entra ID. Debes impedir que los usuarios se conecten a esas aplicaciones desde una lista concreta de países.

¿Qué deberías recomendar?

- A. Activity policies en Microsoft Defender for Cloud Apps
- B. Sign-in risk policies en Entra ID Protection
- C. Una política de Conditional Access con condición de ubicación
- D. User risk policies en Entra ID Protection
- E. Device compliance policies en Microsoft Intune

---

**2.** Usuarios autenticados acceden a SharePoint Online desde dispositivos **no gestionados**. Debes impedir que descarguen archivos, pero permitirles seguir viéndolos en el navegador.

¿Qué **dos** servicios debes incluir?

- A. Microsoft Entra Conditional Access
- B. Microsoft Purview Information Protection
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Entra Application Proxy
- E. Microsoft Intune app protection policies

---

**3.** Se detecta que una cuenta de usuario ha iniciado sesión desde Madrid y, 20 minutos después, desde Yakarta.

¿Qué producto genera esta detección?

- A. Microsoft Defender for Identity
- B. Microsoft Entra ID Protection
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Sentinel UEBA

---

**4.** Un usuario ya autenticado en SharePoint Online descarga 4.000 documentos en diez minutos.

¿Qué producto genera esta detección?

- A. Microsoft Entra ID Protection
- B. Microsoft Defender for Identity
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Defender for Endpoint

---

**5.** Debes exigir que los administradores usen exclusivamente métodos de autenticación resistentes al phishing (FIDO2 o Windows Hello for Business) para acceder al portal de Azure.

¿Qué deberías configurar?

- A. Security defaults en el tenant
- B. Una política de Conditional Access con Authentication Strengths
- C. Una política de Conditional Access que exija MFA
- D. Una política de riesgo de inicio de sesión en Entra ID Protection

---

**6.** Litware quiere que las sesiones activas de Exchange Online, SharePoint Online y Teams se reevalúen **casi en tiempo real** cuando se deshabilita una cuenta o se restablece una contraseña, sin esperar a que caduque el token de acceso.

¿Qué deberías recomendar?

- A. Reducir la vida del token en la configuración de la aplicación
- B. Continuous Access Evaluation con Conditional Access
- C. Sign-in frequency de 1 hora en Conditional Access
- D. Privileged Identity Management con activación de 1 hora

---

**7.** Tu empresa quiere **optimizar la investigación** de incidentes de ransomware siguiendo el enfoque del equipo DART de Microsoft.

¿En qué orden deben realizarse estas tres acciones?

1. Identificar el proceso de recuperación del compromiso
2. Evaluar la situación actual e identificar el alcance
3. Identificar qué aplicaciones de negocio (LOB) están no disponibles

- A. 2 → 3 → 1
- B. 1 → 2 → 3
- C. 3 → 2 → 1
- D. 2 → 1 → 3

---

**8.** Tienes una suscripción de Microsoft 365 sincronizada con AD DS. Debes definir los pasos de **recuperación** tras un ataque de ransomware que ha cifrado datos en la suscripción.

¿Cuál es el **primer** paso del plan de recuperación?

- A. Ejecutar un análisis de seguridad desde Defender for Endpoint
- B. Recuperar los archivos en un equipo limpio
- C. Contactar con las fuerzas del orden
- D. Deshabilitar la sincronización de OneDrive y Exchange ActiveSync

---

**9.** Debes proteger los backups de 300 servidores de forma que un atacante que haya comprometido credenciales privilegiadas no pueda **reducir la retención** ni **eliminar** los puntos de recuperación, ni siquiera con permisos de Global Administrator.

¿Qué deberías recomendar?

- A. Soft delete mejorado con retención de 180 días
- B. MUA con Resource Guard en un tenant distinto
- C. Cifrado de los backups con CMK en Managed HSM
- D. Un Security PIN para operaciones críticas

---

**10.** Una App Service debe conectarse a un SQL Server on-premises que escucha en un puerto TCP estático. La empresa **no tiene** VPN Site-to-Site ni ExpressRoute, y quiere minimizar los endpoints accesibles desde internet hacia la red on-premises.

¿Qué deberías recomendar?

- A. VNet Integration
- B. Hybrid Connections
- C. Un private endpoint en la App Service
- D. Integración con NAT Gateway

---

**11.** Debes dar acceso a usuarios remotos a un servidor RDP y a un recurso compartido SMB alojados on-premises, sin desplegar VPN y sin abrir puertos entrantes en el firewall perimetral.

¿Qué deberías recomendar?

- A. Microsoft Entra Application Proxy
- B. Microsoft Entra Private Access
- C. Microsoft Entra Internet Access
- D. Azure Bastion

---

**12.** La empresa quiere bloquear el acceso de sus dispositivos corporativos a categorías web concretas (redes sociales, juego online) y controlar el tráfico saliente hacia SaaS no aprobado.

¿Qué componente de Global Secure Access deberías recomendar?

- A. Microsoft Entra Private Access
- B. Microsoft Entra Internet Access
- C. Microsoft Entra Application Proxy
- D. Azure Firewall Premium

---

**13.** Debes detectar y bloquear conexiones salientes hacia dominios de command-and-control cuando el tráfico viaja **cifrado por HTTPS** desde las spokes de una topología hub-and-spoke.

¿Qué deberías recomendar?

- A. Azure Firewall Basic con reglas de red
- B. Azure Firewall Standard con reglas de aplicación por FQDN
- C. Azure Firewall Premium con TLS inspection e IDPS
- D. Un NSG con reglas de denegación por Service Tag

---

**14.** Una aplicación web pública recibe ataques de inyección SQL y cross-site scripting. La aplicación se sirve desde tres regiones de Azure y necesitas protección en el edge, lo más cerca posible del usuario.

¿Qué deberías recomendar?

- A. Azure Front Door con WAF y OWASP Core Rule Set
- B. Application Gateway con WAF en cada región
- C. Azure Firewall Premium con IDPS
- D. NSG con Application Security Groups

---

**15.** Debes impedir que las VMs de la subnet de aplicación se comuniquen entre sí, permitiendo solo el tráfico desde la subnet de frontend, **sin gestionar direcciones IP individuales** que cambian con el autoescalado.

¿Qué deberías recomendar?

- A. NSGs con Service Tags
- B. NSGs con Application Security Groups
- C. Azure Firewall con reglas de red
- D. Private endpoints en cada VM

---

**16.** Tu empresa tiene un departamento de banca de inversión, un departamento de análisis y un equipo legal. Los requisitos son:
- Los empleados de banca y de análisis no pueden comunicarse entre sí.
- El equipo legal puede comunicarse con ambos departamentos.

¿Cuál es el número **mínimo** de segmentos y de políticas de Information Barriers?

- A. 2 segmentos, 2 políticas
- B. 3 segmentos, 2 políticas
- C. 3 segmentos, 3 políticas
- D. 4 segmentos, 4 políticas

---

**17.** Debes impedir que los usuarios peguen contenido con números de tarjeta de crédito en sitios de IA generativa de terceros (ChatGPT, Gemini) desde sus portátiles corporativos Windows.

¿Qué deberías recomendar?

- A. Sensitivity labels con cifrado
- B. Endpoint DLP con onboarding de dispositivos en Purview
- C. Insider Risk Management
- D. Conditional Access con Authentication Strengths

---

**18.** Una organización necesita aplicar automáticamente etiquetas de confidencialidad a documentos que contengan datos de clientes, sin intervención del usuario, en SharePoint Online y OneDrive.

¿Qué licencia es el requisito mínimo?

- A. Microsoft 365 E3
- B. Microsoft 365 E5
- C. Entra ID P1
- D. Entra ID P2

---

**19.** Gestionas 400 dispositivos Windows 11 y 150 dispositivos macOS con Microsoft Intune. Debes desplegar **security baselines** y ser capaz de ejecutar un **wipe remoto**.

¿Qué es cierto?

- A. Ambas funciones están disponibles en Windows y macOS
- B. Security baselines solo en Windows; wipe remoto en ambos
- C. Security baselines en ambos; wipe remoto solo en Windows
- D. Ninguna de las dos está disponible en macOS

---

**20.** Debes garantizar que solo se ejecuten aplicaciones autorizadas en 50 VMs con Windows Server 2019, bloqueando automáticamente cualquier aplicación no aprobada hasta que un administrador la autorice.

¿Qué deberías recomendar?

- A. App registrations en Microsoft Entra ID
- B. Application control policies en Microsoft Defender for Endpoint
- C. App discovery anomaly detection policies en Defender for Cloud Apps
- D. Conditional Access App Control

---

**21.** Una base de datos de Azure SQL contiene la columna `TarjetaCredito`. Requisitos:
- Los DBAs, que tienen acceso completo al servidor, **no** deben poder ver el valor real con herramientas de gestión SQL.
- Los operadores de atención al cliente deben ver **solo los cuatro últimos dígitos** desde la aplicación web.

¿Qué combinación deberías incluir?

- A. TDE con CMK + Dynamic Data Masking
- B. Always Encrypted + Dynamic Data Masking
- C. Always Encrypted + Row-Level Security
- D. Dynamic Data Masking + Row-Level Security

---

**22.** Debes compartir blobs concretos con proveedores externos durante 7 días, sin exponerlos públicamente y sin crear identidades para los proveedores.

¿Qué deberías recomendar?

- A. Acceso anónimo de lectura a nivel de container
- B. Shared Access Signatures con caducidad, preferiblemente user delegation SAS
- C. Una cuenta de invitado B2B con rol Storage Blob Data Reader
- D. Un private endpoint compartido con el proveedor

---

**23.** Una entidad exige que la clave de cifrado **nunca** exista dentro de Azure, ni siquiera en Managed HSM, para datos almacenados en Blob Storage.

¿Qué deberías recomendar?

- A. CMK en Managed HSM con BYOK
- B. Encryption scopes por container
- C. Client-side encryption con claves gestionadas fuera de Azure
- D. Doble cifrado con infrastructure encryption

---

**24.** Necesitas ingerir en Microsoft Sentinel los logs de CloudTrail de varias cuentas de AWS y correlacionarlos con señales de Microsoft Defender XDR.

¿Qué deberías recomendar?

- A. Azure Arc en las instancias EC2
- B. El conector nativo de AWS S3 en Sentinel
- C. Defender for Cloud con el conector de AWS
- D. Una Logic App que consulte la API de CloudTrail

---

**25.** Debes recomendar el proceso para incorporar aplicaciones nuevas a Azure siguiendo el Microsoft Security Development Lifecycle. La pregunta es qué incluir en la **fase de diseño**.

- A. SAST con SonarQube
- B. DAST con Veracode
- C. Threat modeling con Microsoft Threat Modeling Tool
- D. Descomposición de software con Visual Studio Enterprise

---
---

# SOLUCIONARIO

---

**1 — C.** Conditional Access con condición de ubicación.
Se crea una **named location** por país y se bloquea en la política. Es control de acceso **determinista**: la regla se cumple o no.
❌ B y D: ID Protection reacciona a **riesgo calculado** (viaje imposible, IP anónima, credenciales filtradas). "Esta lista de países" no es riesgo, es política. ❌ A: las activity policies de MDA actúan **después** del login, dentro de la sesión. ❌ E: el estado del dispositivo no es la ubicación.
🔑 **Regla que resuelve todo este bloque:** *¿el requisito es una condición conocida de antemano (país, dispositivo, app, IP) o una señal de riesgo calculada por Microsoft?* Lo primero → Conditional Access. Lo segundo → ID Protection.

**2 — A + C.** Conditional Access + Defender for Cloud Apps.
Requiere **las dos** piezas: la política de CA enruta la sesión hacia el proxy inverso (Conditional Access App Control) usando el estado del dispositivo como condición, y la **session policy** de MDA es la que bloquea efectivamente la descarga permitiendo la visualización.
❌ B: las etiquetas protegen el archivo, no la acción de descarga en dispositivos no gestionados. ❌ D: App Proxy publica apps on-prem. ❌ E: las MAM protegen apps móviles, no la sesión de navegador en un equipo ajeno.
🔑 **"Bloquear descarga / copiar / imprimir en sesión" + "unmanaged devices" → siempre CA + MDA session policy.** Una sola de las dos nunca es la respuesta completa.

**3 — B.** Entra ID Protection.
Viaje imposible es una detección de **riesgo de inicio de sesión** en el momento del login cloud.
❌ C: MDA detecta también viaje imposible, pero en el contexto de sesiones SaaS; cuando la pregunta describe el **sign-in** a Entra, la respuesta canónica del examen es ID Protection.

**4 — C.** Defender for Cloud Apps.
🔑 **Post-login, dentro de la app SaaS, comportamiento masivo (mass download, mass delete) → MDA.** Contrasta con la 3: ID Protection es *en el login*, MDA es *después del login*.

**5 — B.** Authentication Strengths.
🔑 **"phishing-resistant"** o el nombre de un método concreto (FIDO2, Windows Hello, certificado) → Authentication Strengths.
❌ C: "requerir MFA" a secas admite SMS o llamada, que no son resistentes al phishing. Es el distractor diseñado. ❌ A: security defaults es todo-o-nada, sin granularidad.

**6 — B.** Continuous Access Evaluation.
CAE hace que EXO, SPO y Teams reevalúen la sesión ante eventos críticos (cuenta deshabilitada, contraseña restablecida, revocación de token, cambio de ubicación) sin esperar al vencimiento del token.
❌ C: sign-in frequency fuerza reautenticación periódica, pero sigue siendo *esperar*, no *casi en tiempo real*.

**7 — A.** 2 → 3 → 1.
Evaluar la situación y el alcance → identificar qué aplicaciones de negocio están caídas → identificar el proceso de recuperación del compromiso.
🔑 **La lógica es siempre: entender antes de priorizar, priorizar antes de actuar.** No puedes saber qué LOB apps recuperar sin conocer el alcance, ni diseñar la recuperación sin saber qué está caído.
⚠️ **Ojo a la trampa de este par de preguntas:** DART tiene dos secuencias distintas y el examen las mezcla. Ver la 8.

**8 — D.** Deshabilitar la sincronización de OneDrive y Exchange ActiveSync.
🔑 **Contención primero.** El primer paso del plan de *recuperación* es cortar la propagación al cloud.

> **Cuadro de las dos secuencias DART** (memorízalo así, es donde fallaste):
>
> | | **Investigación** (¿qué ha pasado?) | **Recuperación** (¿qué hago?) |
> |---|---|---|
> | 1º | Evaluar situación y alcance | **Deshabilitar OneDrive sync + ActiveSync** |
> | 2º | Identificar LOB apps no disponibles | Investigar el alcance |
> | 3º | Identificar proceso de recuperación | Erradicar preservando evidencia |
> | 4º | — | Recuperar a dispositivos limpios |
>
> Si el enunciado dice **"investigate" / "optimize investigations"** → columna izquierda. Si dice **"recovery plan" / "first step"** → columna derecha. Nunca escanear, restaurar ni llamar a la policía como primer paso.

**9 — B.** MUA con Resource Guard en un tenant distinto.
🔑 **"ni siquiera un Global Administrator"** → otro tenant, no otra suscripción.
❌ A: soft delete da margen temporal pero no impide la operación. ❌ C: el cifrado protege confidencialidad, no impide el borrado. ❌ D: el PIN es anti-error humano; el atacante con la credencial lo genera igual.

**10 — B.** Hybrid Connections.
Sin VPN ni ExpressRoute, la VNet no tiene camino a on-premises. El Hybrid Connection Manager se instala en la red on-prem y abre un túnel **saliente** por TCP 443 → cero endpoints entrantes expuestos.
❌ A: VNet Integration es **outbound hacia la VNet**, y la VNet no llega a on-prem. ❌ C: private endpoint es **inbound hacia la app**, dirección contraria. ❌ D: NAT Gateway es salida a internet.
🔑 **App Service + on-prem + sin VPN/ER + puerto TCP estático → Hybrid Connections.**

**11 — B.** Entra Private Access.
RDP y SMB no son HTTP → Application Proxy queda descartado de inmediato.
❌ D: Azure Bastion da acceso a **VMs de Azure** desde el portal, no publica recursos on-premises.

**12 — B.** Entra Internet Access.
🔑 **Filtrado web, categorías de URL, SWG, tráfico saliente hacia internet/SaaS → Internet Access.**
Contraste con la 11: **Private Access trae al usuario hacia recursos privados de la organización; Internet Access controla lo que sale hacia internet.**
❌ D: Azure Firewall Premium filtra tráfico de **recursos en la VNet**, no de dispositivos de usuario en cualquier red.

**13 — C.** Azure Firewall Premium.
🔑 **Inspeccionar contenido dentro de HTTPS → Premium obligatorio.** Standard filtra por FQDN (ve el nombre vía SNI) pero no descifra, así que no ve el contenido ni aplica IDPS.

**14 — A.** Front Door con WAF.
🔑 **Multi-región + edge + protección L7 → Front Door WAF.** Application Gateway WAF es **regional y dentro de tu VNet**: válido, pero obliga a una instancia por región y no protege en el edge.
❌ C: el Firewall es L3/L4 + inspección de egress, no protege la app de SQLi/XSS entrante.

**15 — B.** NSGs con ASGs.
🔑 **"agrupar por rol sin gestionar IPs"** → ASG. Con autoescalado las IPs cambian; el ASG es la agrupación lógica que se mantiene.
❌ A: los Service Tags son etiquetas **de Microsoft** para servicios de Azure (Storage, SQL, KeyVault), no para tus VMs.

**16 — B.** 3 segmentos, 2 políticas.
Segmentos: Banca, Análisis, Legal. Políticas: solo donde hay **restricción** → Banca bloquea Análisis, y Análisis bloquea Banca. Legal no lleva política porque el comportamiento por defecto es permitir, y eso ya cumple "puede hablar con ambos".
🔑 **Segmentos = grupos con comportamiento de comunicación distinto. Políticas = segmentos que necesitan restricción.** Un segmento sin restricciones no consume política.
⚠️ La trampa es responder 3 políticas "por simetría" con Legal, o 4 segmentos contando cada grupo organizativo.

**17 — B.** Endpoint DLP con onboarding de dispositivos.
🔑 **Apps de IA de terceros + control en el endpoint → Endpoint DLP, y requiere onboarding del dispositivo a Purview, no licencia de Copilot.** Esta es la actualización de 2026 que conviene tener fresca.
❌ A: la etiqueta protege el documento, no impide pegar texto en un navegador. ❌ C: IRM detecta patrones de comportamiento, no bloquea la acción.

**18 — B.** Microsoft 365 E5.
🔑 **Auto-labeling siempre E5.** E3 solo da etiquetado **manual**. Mismo patrón para Endpoint DLP, Insider Risk, eDiscovery Premium y Audit Premium.

**19 — B.** Security baselines solo Windows; wipe remoto en ambos.
🔑 **Regla general de Intune:** lo que es "baseline", "privilege management" o específico del stack Windows (BitLocker, Autopilot, Defender) es **Windows-only**. Las acciones remotas genéricas sobre el dispositivo (lock, wipe, retire, sync) son **multiplataforma**.

**20 — B.** Application control policies en Defender for Endpoint.
Allow-list explícita de software autorizado, con bloqueo por defecto de lo no aprobado.
❌ C: MDA app discovery detecta shadow IT SaaS, no ejecución de binarios. ❌ D: CA App Control es control de sesión en apps cloud.

**21 — B.** Always Encrypted + Dynamic Data Masking.
Dos requisitos, dos controles distintos:
- *DBA no ve el valor real* → **Always Encrypted**: la clave nunca reside en el servidor SQL, así que ni sysadmin ve el claro.
- *Operadores ven los 4 últimos dígitos* → **Dynamic Data Masking**: ofuscación en la presentación.
❌ A: TDE cifra en reposo pero el DBA sí ve el dato en claro al consultar — es la confusión clásica.
⚠️ Recuerda que DDM **solo oculta visualmente**: el dato real sigue siendo accesible con los permisos adecuados. Por eso no basta por sí solo para el primer requisito.

**22 — B.** SAS con caducidad, preferiblemente **user delegation SAS**.
La user delegation SAS se firma con credenciales de Entra en lugar de con la clave de cuenta, así que es revocable y auditable.
❌ A: expone públicamente. ❌ C: funciona pero exige crear identidades, que el enunciado descarta.

**23 — C.** Client-side encryption.
🔑 **"La clave nunca existe en Azure"** = HYOK, y **HYOK no está soportado en el cifrado nativo de PaaS**. La única alternativa real es cifrar antes de subir.
❌ A: BYOK importa la clave a Azure, así que **la clave acaba existiendo en Azure** — es exactamente lo que se prohíbe.

**24 — B.** El conector nativo de AWS S3 en Sentinel.
🔑 **Logs de terceros / CloudTrail / ingesta centralizada → Sentinel.** Azure Arc extiende el control plane para políticas y agentes, pero no es un mecanismo de ingesta de logs.
❌ D: Logic Apps es automatización y playbooks, **nunca** ingesta.

**25 — C.** Threat modeling con Microsoft Threat Modeling Tool.
🔑 **Fase de diseño → threat modeling (STRIDE).** SAST es build/commit, DAST es operate.
Y el complemento que cae mucho: el diagrama que se crea en la Threat Modeling Tool es un **data flow diagram**.

---

## AUTOEVALUACIÓN

| Aciertos | Lectura |
|---|---|
| 23-25 | Listo para el 29. Solo repaso ligero. |
| 20-22 | Aprobado con margen. Mira si los fallos se agrupan por dominio. |
| 17-19 | Aprobarías justo. Repasa el dominio con más fallos antes de seguir. |
| <17 | Vuelve al diccionario en ese bloque antes de más preguntas. |

**Sigue sin cubrirse** (los tres gaps que más cuestan): granularidad de Azure Files a nivel de share, AzureBastionSubnet como objetivo de control NSG, y el tier de GitHub Enterprise Cloud para Dependabot en repos privados. Esos tres piden un bloque propio.

**Repite estas si fallaste:** 1, 2, 7 y 8 son las versiones directas de tus los fallos previos. Si vuelves a fallar alguna, el problema no es memoria sino el criterio de discriminación — vuelve a la regla en negrita de cada una.
