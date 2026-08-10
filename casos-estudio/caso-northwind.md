# CASO DE ESTUDIO SC-100 → Northwind Traders

**Formato de examen. Tiempo objetivo: 35 minutos para 12 preguntas.**
Léelo entero una vez, localiza en cada pregunta la restricción que la decide, y no mires el solucionario hasta terminar.

> Recuerda el método: la respuesta casi siempre la fija un adjetivo o cláusula del enunciado ("single tenancy", "se redepliegan", "sin salida a internet", "de terceros"). Búscalo antes de leer las opciones.

---

# ESCENARIO

## Visión general

Northwind Traders es una cadena de distribución mayorista con sede en Londres y centros logísticos en Dublín y Ámsterdam. Tiene 3.100 empleados y un área de desarrollo que publica APIs para sus distribuidores.

## Entorno actual

### Identidad y colaboración

- Un tenant de Microsoft Entra ID: `northwind.onmicrosoft.com`. Licencias **Microsoft 365 E5** y **Entra ID P2**.
- Un bosque de AD DS on-premises sincronizado con **PHS**.
- La empresa usa una aplicación SaaS de facturación de un proveedor, **Invoicing365**, que es **multi-tenant** y está registrada en el tenant del fabricante, `invoicing365.com`.
- Los empleados colaboran intensamente en **Microsoft Teams**, tanto en mensajes de canal como en ficheros compartidos en los canales.

### Infraestructura

- Una suscripción de Azure con una topología hub-and-spoke.
- Un **Azure API Management** que publica 12 APIs a tres grupos distintos de distribuidores, cada grupo con acceso a un subconjunto de APIs y patrones de consumo muy variables.
- Un conjunto de **máquinas virtuales que alojan la app `OrderProc`**; estas VMs se **destruyen y recrean varias veces al día** según la demanda. `OrderProc` lee secretos de un Key Vault.
- 20 **App Service web apps** que exponen servicios a clientes externos; **cada una tiene su propio certificado y clave**.
- Un servidor on-premises, **Srv-Vault**, que aloja una base de datos crítica y está tras un firewall que **le impide cualquier conexión saliente a internet**.
- Tres servidores de ficheros on-premises con **carpetas compartidas** que contienen 40.000 documentos.
- Cinco cuentas de **Azure Blob Storage** con datos de las web apps.

### Seguridad y datos

- Microsoft Purview desplegado, protegiendo actualmente **solo Exchange Online y SharePoint Online**. Hay **SITs personalizados** creados para identificar datos de clientes.
- Backups mediante Azure Backup en Recovery Services vaults, sin protecciones avanzadas.
- Los dispositivos son Windows 11, unidos a Entra y gestionados con Intune. **Aún no están onboarded en Purview.**

## Incidentes y observaciones recientes

1. Un desarrollador accedió directamente a una base de datos usando la cuenta de servicio de una aplicación, porque esa cuenta tenía credenciales reutilizables.
2. Se ha detectado que numerosos empleados pegan fragmentos de pedidos y datos de clientes en **webs de IA generativa de terceros** (ChatGPT, Gemini) desde sus portátiles.
3. Durante una prueba de recuperación, se comprobó que un administrador de backups podía, por sí solo, reducir la retención de un vault.

## Requisitos planificados

### Identidad y acceso

- **R1.** Permitir que los empleados de Northwind se autentiquen en Invoicing365 con sus credenciales de Entra.
- **R2.** Dar a `OrderProc` acceso a los secretos del Key Vault minimizando los security principals con acceso al vault, sin almacenar secretos en las VMs y minimizando el esfuerzo administrativo, teniendo en cuenta que las VMs se recrean continuamente.
- **R3.** Sustituir la cuenta de servicio con credenciales reutilizables por un método que impida su uso fuera de la aplicación, minimizando el esfuerzo administrativo.

### APIs

- **R4.** Cada grupo de distribuidores debe poder acceder solo a su subconjunto de APIs, cada subconjunto debe poder tener sus propios rate limits, y hay que minimizar esfuerzo y coste.
- **R5.** En el modelo de gestión de APIM, usar RBAC para gestionar las APIs y keys para controlar el consumo, asociando cada mecanismo al scope correcto.

### Protección de datos

- **R6.** Gestionar los certificados y claves de las 20 web apps en una solución de **tenancy propia**, con FIPS 140-2 Level 3, siguiendo least privilege y minimizando coste y esfuerzo.
- **R7.** Escanear los blobs de Azure y las carpetas compartidas on-prem usando los SITs personalizados para localizar datos de clientes.
- **R8.** Bloquear que los empleados envíen prompts con datos confidenciales a webs de IA de terceros, y escanear los prompts que se envían.

### Continuidad y backup

- **R9.** Mitigar ataques de ransomware sobre los **mensajes** de canales de Teams y sobre los **ficheros** de canales de Teams.
- **R10.** Diseñar una estrategia de resiliencia con backup a Azure para **Srv-Vault**, teniendo en cuenta que no puede salir a internet.
- **R11.** Impedir que un administrador de backups comprometido pueda, por sí solo, reducir la retención o eliminar backups. Debe exigirse la aprobación de un segundo actor, con la máxima seguridad posible.

### Red

- **R12.** Desplegar Microsoft Entra Internet Access desde los centros logísticos hacia Microsoft 365, de forma que el acceso a M365 quede condicionado a que el tráfico pase por Global Secure Access. En Dublín se asignará una **remote network** a la política; en Ámsterdam se desplegará el **cliente de Global Secure Access** en todos los dispositivos.

### Restricciones

- **C1.** Preferir servicios gestionados y minimizar el esfuerzo administrativo.
- **C2.** Minimizar costes donde sea posible, aceptando el gasto que un requisito exija explícitamente.

---
---

# PREGUNTAS

---

**1.** Para cumplir **R1**, ¿qué debes crear en el tenant de Northwind?

- A. Un application object (registro de app)
- B. Una enterprise application (service principal)
- C. Una user-assigned managed identity
- D. Un relying party trust en AD FS

---

**2.** Para cumplir **R2**, ¿qué endpoint y qué tipo de identidad debe usar `OrderProc`?

- A. Endpoint público del Key Vault + system-assigned managed identity
- B. IMDS + system-assigned managed identity
- C. IMDS + user-assigned managed identity
- D. Private endpoint + service principal con secreto

---

**3.** Para cumplir **R3**, ¿qué método de autenticación deberías recomendar?

- A. Un group managed service account (gMSA)
- B. Una managed identity
- C. Un delegated managed service account (dMSA)
- D. Una cuenta de usuario de Entra con rotación de contraseña

---

**4.** Para cumplir **R4**, ¿qué tier de APIM y a qué scope debes aplicar los rate limits?

- A. Consumption + rate limit por API
- B. Developer + rate limit por API
- C. Standard v2 + rate limit por Product
- D. Premium + rate limit por Subscription

---

**5.** Para cumplir **R5**, ¿a qué scope asocias RBAC y a qué scope las keys?

- A. RBAC → Products; Keys → Subscriptions
- B. RBAC → Subscriptions; Keys → Products
- C. RBAC → APIs; Keys → Products
- D. RBAC → Subscriptions; Keys → APIs

---

**6.** Para cumplir **R6**, ¿qué debes incluir?

- A. Azure Key Vault Premium con access policies
- B. Azure Key Vault Premium con RBAC
- C. Azure Key Vault Managed HSM con un único vault y RBAC
- D. Un Managed HSM por cada web app con RBAC

---

**7.** Para cumplir **R7**, empareja cada origen con la herramienta de Purview:

| Origen | Herramienta |
|---|---|
| Blobs de Azure Storage | ? |
| Carpetas compartidas on-prem | ? |

Opciones: *Data Map · Information Protection scanner · Content Explorer · Data Loss Prevention*

---

**8.** Para cumplir **R8**, ¿qué dos componentes necesitas y qué debe hacerse primero?

- A. Sensitivity labels + Insider Risk Management; primero habilitar auto-labeling
- B. Endpoint DLP + DSPM for AI; primero onboarding de los dispositivos en Purview
- C. Endpoint DLP + DSPM for AI; primero licenciar a los usuarios con Microsoft 365 Copilot
- D. Conditional Access + Defender for Cloud Apps; primero crear una session policy

---

**9.** Para cumplir **R9**, ¿qué debes usar para cada tipo de contenido de Teams?

- A. Mensajes → Files Restore; Ficheros → Single Item Recovery
- B. Mensajes → Single Item Recovery; Ficheros → Files Restore
- C. Mensajes → Retention labels; Ficheros → Immutable storage
- D. Ambos → Azure Backup del tenant de M365

---

**10.** Para cumplir **R10**, ¿qué debes incluir?

- A. El agente MARS instalado en Srv-Vault + un Recovery Services vault
- B. Microsoft Azure Backup Server (MABS) + un Recovery Services vault
- C. Azure Site Recovery + una cuenta de Storage
- D. VNet Integration + un Recovery Services vault

---

**11.** Para cumplir **R11**, ¿qué debes recomendar y con qué configuración de máxima seguridad?

- A. Immutable vault bloqueado en la misma suscripción
- B. MUA con Resource Guard en la misma suscripción, rol Contributor para el equipo de backup
- C. MUA con Resource Guard en una suscripción/tenant separada, rol Reader para el equipo de backup
- D. Un Security PIN para operaciones críticas

---

**12.** Respecto a **R12**: una vez desplegado, ¿desde qué centros logísticos cumplirán los usuarios el compliant network check?

- A. Solo Dublín
- B. Solo Ámsterdam
- C. Tanto Dublín como Ámsterdam
- D. Ninguno de los dos

---
---

# SOLUCIONARIO

---

**1 → B.** Enterprise application (service principal).

Invoicing365 es multi-tenant y su **application object ya existe en `invoicing365.com`**. Lo que le falta a Northwind es la instancia local de esa app: el **service principal**, que en el portal se llama enterprise application y se crea al consentir la app.
❌ A: el application object no se crea en el tenant consumidor. ❌ C: las managed identities autentican recursos de Azure, no federan apps entre tenants. ❌ D: AD FS es complejidad on-prem innecesaria para un SaaS moderno.
🔑 *App de otro tenant, ¿qué creo en el mío? → service principal / enterprise application.*

---

**2 → C.** IMDS + user-assigned managed identity.

La cláusula que decide: **las VMs se recrean varias veces al día.** Una identidad *system-assigned* moriría con cada VM y habría que reconfigurar el acceso al vault sin parar. La **user-assigned sobrevive**: la creas una vez, le das acceso al vault una vez, y la reasignas a cada VM nueva → lo que además **minimiza los principals** con acceso (una sola identidad para todas). **IMDS** es el endpoint local desde el que se obtiene el token sin almacenar credenciales.
❌ B: system-assigned choca con la recreación continua. ❌ D: un secreto es exactamente lo que se quiere evitar.

---

**3 → B.** Una managed identity.

El problema del incidente 1 es que la cuenta de servicio tenía **credenciales reutilizables** que un humano pudo usar fuera de la app. Una managed identity **no tiene credenciales que un desarrollador pueda copiar**: el token se emite a la carga de trabajo y no es reutilizable fuera de ella. Y elimina la gestión de contraseñas (mínimo esfuerzo).
❌ A y C: gMSA/dMSA son de AD on-prem, no aplican a una app en Azure que va contra Azure SQL. ❌ D: seguiría teniendo una contraseña reutilizable.

---

**4 → C.** Standard v2 + rate limit por Product.

Cada grupo de distribuidores → un **Product** distinto que agrupa su subconjunto de APIs; el rate limit se aplica a nivel de producto, así cada subconjunto tiene el suyo con esfuerzo mínimo. **Standard v2** es el tier de producción que cumple sin el sobrecoste de Premium (satisface **C2**).
❌ A/B: Consumption y Developer no son adecuados para producción con SLA. ❌ D: Premium encarece innecesariamente.

---

**5 → B.** RBAC → Subscriptions; Keys → Products.

**RBAC gobierna la gestión** (quién administra APIs y quién crea/revoca suscripciones) → scope Subscriptions. **Las keys gobiernan el consumo** y se generan al suscribirse a un **Product** → scope Products.
🔑 *Gestión→Subscriptions, consumo→Products.*

---

**6 → C.** Managed HSM con un único vault y RBAC.

El discriminador es **"tenancy propia"**, no el FIPS: Key Vault Premium también es FIPS 140-2 Level 3, pero en HSM **compartido**. Tenancy propia obliga a **Managed HSM** (single-tenant dedicado). Managed HSM **solo admite RBAC** (least privilege), y **un único vault** para las 20 apps minimiza coste y esfuerzo.
❌ A: Premium no es single-tenant, y además Managed HSM no usa access policies. ❌ D: un HSM por app dispara el coste.

---

**7 →**

| Origen | Herramienta |
|---|---|
| Blobs de Azure Storage | **Data Map** |
| Carpetas compartidas on-prem | **Information Protection scanner** |

Blob es fuente cloud-native integrada con Purview → **Data Map** la escanea y cataloga. Las carpetas on-prem las escanea el **Information Protection scanner**, que clasifica y etiqueta ficheros en file servers.
🔑 *Cloud → Data Map; file share on-prem → IP scanner.*

---

**8 → B.** Endpoint DLP + DSPM for AI; primero onboarding de dispositivos en Purview.

**Endpoint DLP** bloquea el envío de contenido con SITs a webs de IA desde el dispositivo Windows; **DSPM for AI** da la visibilidad de los prompts. Ambos exigen que los **dispositivos estén onboarded en Purview** → y en el escenario aún **no lo están**, así que ese es el primer paso.
❌ C: **la trampa.** La visibilidad de apps de IA de **terceros** no requiere licencia de Copilot; esa licencia es solo para ver *Microsoft 365 Copilot*.

---

**9 → B.** Mensajes → Single Item Recovery; Ficheros → Files Restore.

Teams no tiene almacenamiento propio: los **mensajes** viven en buzones de **Exchange Online** (→ Single Item Recovery restaura mensajes borrados/alterados) y los **ficheros** en **SharePoint/OneDrive** (→ Files Restore revierte la biblioteca a un punto anterior).
🔑 *La protección de Teams es la de su carga subyacente: EXO para mensajes, SPO para ficheros.*

---

**10 → B.** MABS + Recovery Services vault.

**Srv-Vault no puede salir a internet**, así que el agente **MARS** (que respalda directo a Azure) queda descartado. **MABS** actúa como servidor de backup **local**: Srv-Vault respalda contra MABS dentro de la red, y MABS orquesta el envío a Azure. Además MABS soporta backup app-aware de la BD crítica. El **Recovery Services vault** almacena datos, políticas y puntos de recuperación.
🔑 *Servidor sin salida directa a Azure o con app-aware → MABS + Recovery Services vault. MARS solo para ficheros directos a Azure.*

---

**11 → C.** MUA con Resource Guard en suscripción/tenant separada, rol Reader para el equipo de backup.

**MUA con Resource Guard** impone la regla de dos personas sobre operaciones críticas. Para **máxima seguridad**: el Resource Guard vive **fuera del alcance** de los administradores de backup (otra suscripción, idealmente otro tenant) y al equipo de backup se le asigna **solo Reader** sobre él, de modo que no puedan auto-aprobar la reducción de retención ni el borrado.
❌ A: la inmutabilidad protege los datos, pero el requisito pide aprobación de un segundo actor sobre operaciones → eso es MUA. ❌ B: en la misma suscripción y con Contributor, el propio equipo podría desactivarlo. ❌ D: el PIN es anti-error, no anti-ataque.

---

**12 → C.** Tanto Dublín como Ámsterdam.

**Ojo, esta es la versión bien redactada del concepto que se te resistía.** Ambos centros tienen un camino válido hacia GSA contemplado por el despliegue:
- **Dublín** → **remote network asignada** a la política: su tráfico entra por GSA aunque los dispositivos no tengan cliente.
- **Ámsterdam** → **cliente GSA** en todos los dispositivos: entra por GSA a nivel de dispositivo.

Los dos caminos son independientes y ambos satisfacen el compliant network check, así que los dos cumplen.
🔑 Compara con la pregunta 11 del simulacro #3, donde **solo** cumplía una oficina porque el cliente estaba desplegado en una oficina cuyo camino **no** estaba contemplado por la asignación de la política. **La regla no es "¿está instalado?", sino "¿está ese camino asignado a la política?"** Aquí ambos lo están; allí no. Mismo concepto, resultado distinto según la asignación → que es justamente el matiz que tienes que dominar.

---

# LO QUE ENTRENA ESTE CASO

| Pregunta | Restricción que la decide |
|---|---|
| 1 | "multi-tenant, registrada en otro tenant" → service principal |
| 2 | "se recrean varias veces al día" → user-assigned |
| 3 | "credenciales reutilizables fuera de la app" → managed identity |
| 4 | "grupos con APIs y ritmos distintos" → Products |
| 5 | gestión vs consumo → Subscriptions vs Products |
| 6 | "tenancy propia" → Managed HSM (no el FIPS) |
| 7 | cloud vs on-prem → Data Map vs IP scanner |
| 8 | "de terceros" + "no onboarded" → Endpoint DLP + DSPM, onboarding primero |
| 9 | mensajes=EXO, ficheros=SPO → SIR + Files Restore |
| 10 | "no puede salir a internet" → MABS |
| 11 | "un segundo actor" + "máxima seguridad" → MUA, otro tenant, Reader |
| 12 | "¿está el camino asignado?" no "¿está instalado?" → ambos cumplen |

Si sacas 11 o 12, esta tanda de temas está cerrada. Repite en voz alta la restricción que decide cada una: es el músculo que de verdad se examina en los casos de estudio.
