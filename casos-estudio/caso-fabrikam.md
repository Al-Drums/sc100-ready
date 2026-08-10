# CASO DE ESTUDIO SC-100 → Fabrikam Industrial

**Formato real de examen.** En el SC-100 los casos de estudio aparecen al principio y las preguntas se responden **releyendo el escenario**, no de memoria. Léelo entero una vez, luego contesta. Tiempo objetivo: **30 minutos** para las 10 preguntas.

> **Consejo de examen:** en los casos de estudio, casi todas las respuestas están determinadas por una restricción concreta del enunciado ("no puede desplegar agentes", "sin coste adicional", "minimizar el esfuerzo administrativo"). Antes de mirar las opciones, localiza qué restricción aplica a esa pregunta.

---

# ESCENARIO

## Visión general

Fabrikam Industrial es un fabricante con sede en Barcelona y plantas en Alemania, México y Singapur. Cuenta con 4.200 empleados.

## Entorno actual

### Identidad

- Un bosque de Active Directory Domain Services llamado `fabrikam.local`, con cuatro controladores de dominio.
- Un tenant de Microsoft Entra ID llamado `fabrikam.onmicrosoft.com`, sincronizado mediante **Entra Connect Sync** con **Federation (ADFS)** como método de autenticación.
- Una granja de ADFS de dos servidores en la sede central.
- Recientemente se adquirió una empresa, Litware Components, con su propio bosque de AD DS **sin relación de confianza** con `fabrikam.local` y su propio tenant de Entra ID.
- Licencias actuales: **Microsoft 365 E3** para todos los empleados y **Entra ID P1**.

### Infraestructura

- Tres suscripciones de Azure organizadas bajo un único management group.
- Una topología hub-and-spoke en West Europe, con Azure Firewall **Standard** en el hub.
- 180 máquinas virtuales en Azure y 240 servidores físicos y virtuales on-premises.
- 40 instancias EC2 en una cuenta de AWS utilizada por el equipo de analítica.
- Un cluster de AKS que aloja 30 microservicios de la plataforma de pedidos.
- Varias App Services que exponen APIs a distribuidores externos.
- Una cuenta de Azure Storage que contiene documentación técnica de tres divisiones de producto.

### Seguridad actual

- Microsoft Defender for Cloud con el plan **Foundational CSPM** únicamente.
- Microsoft Sentinel desplegado, ingiriendo solo logs de actividad de Azure.
- Backups mediante Azure Backup en un Recovery Services vault, sin protecciones adicionales.

## Incidentes recientes

1. Hace tres meses, un ataque de ransomware cifró 60 estaciones de trabajo. La propagación llegó a OneDrive antes de que se contuviera. La investigación fue lenta porque no se sabía qué aplicaciones de negocio estaban afectadas.
2. Se detectó que una cuenta de servicio de AD DS había sido objeto de **Kerberoasting**. El ataque no se detectó hasta la revisión forense posterior.
3. Un empleado que dejó la empresa descargó 3.000 documentos de SharePoint Online durante su última semana. No hubo alerta.

## Requisitos planificados

### Identidad

- **R1.** Reducir la infraestructura on-premises de autenticación. La dirección quiere retirar ADFS.
- **R2.** Habilitar la detección de credenciales filtradas en el tenant.
- **R3.** Integrar los usuarios de Litware Components sin establecer una relación de confianza entre bosques y sin desplegar servidores adicionales en su red.
- **R4.** Los administradores globales deben elevar sus privilegios solo cuando lo necesiten, con aprobación y por tiempo limitado.
- **R5.** Los proveedores externos deben poder solicitar por sí mismos acceso a un conjunto de aplicaciones, con aprobación de un responsable interno y revisión trimestral.

### Protección de datos

- **R6.** Cada división de producto debe poder revocar de forma independiente el acceso a su documentación técnica en la cuenta de Storage, sin afectar a las demás divisiones.
- **R7.** Aplicar etiquetas de confidencialidad de forma automática a los documentos que contengan especificaciones de producto, sin intervención del usuario.
- **R8.** Impedir que los empleados suban documentación técnica a herramientas de IA generativa de terceros desde sus portátiles corporativos.

### Infraestructura y red

- **R9.** Detectar exfiltración de datos sobre tráfico HTTPS saliente desde las spokes.
- **R10.** Las APIs de las App Services no deben ser accesibles desde internet; solo desde la VNet corporativa. Los despliegues se realizan desde Azure Pipelines.
- **R11.** Asegurar la comunicación entre los 30 microservicios de AKS con mTLS, minimizando el esfuerzo administrativo.
- **R12.** Aplicar las mismas políticas de configuración de seguridad a las instancias EC2 y a los servidores on-premises que a las VMs de Azure.

### Operaciones de seguridad

- **R13.** Detectar ataques contra AD DS on-premises como Kerberoasting, DCSync y Pass-the-Hash.
- **R14.** Detectar descargas masivas realizadas por usuarios ya autenticados en aplicaciones SaaS.
- **R15.** Ingerir en Sentinel los logs CEF de los firewalls perimetrales on-premises y los logs de CloudTrail de la cuenta de AWS.
- **R16.** Los backups no deben poder eliminarse ni acortarse en su retención, ni siquiera por un administrador global comprometido del tenant.
- **R17.** Identificar identidades sobre-privilegiadas simultáneamente en Azure y AWS.

### Restricciones

- **C1.** Minimizar los costes de licenciamiento allí donde sea posible, pero se acepta actualizar licencias cuando un requisito lo exija.
- **C2.** Minimizar el esfuerzo administrativo y el número de servidores desplegados.
- **C3.** Las soluciones deben preferir servicios gestionados sobre despliegues propios.

---
---

# PREGUNTAS

---

**1.** Debes recomendar una solución de identidad híbrida que cumpla **R1**, **R2** y **R3**.

¿Qué deberías recomendar?

- A. Mantener ADFS y habilitar Password Hash Synchronization como respaldo
- B. Migrar a Pass-through Authentication y desplegar Entra Connect Sync en el bosque de Litware
- C. Migrar a Password Hash Synchronization y usar Entra Cloud Sync para el bosque de Litware
- D. Establecer una relación de confianza entre bosques y ampliar Entra Connect Sync

---

**2.** ¿Qué cambio de licenciamiento es necesario, como mínimo, para cumplir **R4** y **R5**?

- A. Ninguno: Entra ID P1 los cubre
- B. Actualizar a Entra ID P2
- C. Actualizar a Microsoft 365 E5
- D. Adquirir Entra Permissions Management

---

**3.** Debes cumplir **R6**.

¿Qué deberías recomendar?

- A. Una CMK única en Key Vault a nivel de cuenta de Storage
- B. Encryption scopes con una CMK distinta por container
- C. Tres cuentas de Storage separadas
- D. Client-side encryption

---

**4.** Debes cumplir **R7** y **R8**.

¿Qué **dos** cambios son necesarios? (Elige dos.)

- A. Actualizar a Microsoft 365 E5
- B. Realizar el onboarding de los dispositivos en Microsoft Purview
- C. Adquirir licencias de Microsoft 365 Copilot
- D. Desplegar Microsoft Entra Internet Access
- E. Habilitar Insider Risk Management únicamente

---

**5.** Debes cumplir **R9** y **R10**.

¿Qué deberías recomendar? (Elige **dos** acciones para R10 además del cambio para R9.)

- A. Actualizar Azure Firewall a Premium con TLS inspection e IDPS
- B. Habilitar VNet Integration en las App Services
- C. Configurar Private Endpoints y deshabilitar el acceso público, incluido el endpoint SCM
- D. Configurar Service Endpoints hacia las App Services
- E. Aplicar Access Restrictions por Service Tag

---

**6.** Debes cumplir **R11**.

- A. Flux
- B. Dapr
- C. Istio
- D. Envoy configurado manualmente en cada microservicio

---

**7.** Debes cumplir **R12**.

- A. Aplicar Azure Policy directamente sobre las instancias EC2
- B. Conectar AWS a Defender for Cloud mediante el conector nativo únicamente
- C. Desplegar Azure Arc en las instancias EC2 y en los servidores on-premises, y aplicar Azure Policy
- D. Desplegar Azure Blueprints

---

**8.** Debes cumplir **R13**, **R14** y **R15**.

Empareja cada requisito con la solución correcta:

| Requisito | Solución |
|---|---|
| R13 (Kerberoasting, DCSync) | ? |
| R14 (descargas masivas en SaaS) | ? |
| R15 (CEF on-prem + CloudTrail) | ? |

Opciones: *Microsoft Entra ID Protection · Microsoft Defender for Identity · Microsoft Defender for Cloud Apps · Microsoft Sentinel con servidor Syslog y AMA, más el conector de AWS S3 · Microsoft Defender for Endpoint*

---

**9.** Debes cumplir **R16**.

¿Qué **tres** acciones deberías realizar y en qué orden?

1. Bloquear la inmutabilidad del vault
2. Configurar MUA con un Resource Guard en un tenant distinto
3. Habilitar la inmutabilidad del vault

- A. 3 → 1 → 2
- B. 2 → 3 → 1
- C. 3 → 2 → 1
- D. 1 → 3 → 2

---

**10.** Debes cumplir **R17**.

- A. Microsoft Entra Privileged Identity Management
- B. Microsoft Entra Permissions Management
- C. Defender CSPM con Attack Path Analysis
- D. Microsoft Defender for Cloud Apps

---
---

# SOLUCIONARIO

---

**1 → C.** PHS + Entra Cloud Sync para Litware.

Tres requisitos, tres piezas del razonamiento:
- **R1** (retirar ADFS, reducir infraestructura on-prem) → descarta A y cualquier opción que mantenga la federación.
- **R2** (leaked credentials) → **PHS es la única forma de tenerlo**, sin excepción. Esto descarta B, porque PTA no lo soporta.
- **R3** (bosque sin trust, sin servidores adicionales) → **Cloud Sync**, que soporta *disconnected forests* con un agente ligero. Connect Sync exigiría un servidor dedicado, chocando con **C2**.

❌ D: establecer un trust entre bosques es exactamente lo que el requisito prohíbe.
🔑 Regla: *"disconnected forests" o "M&A sin trust" → Cloud Sync. "Leaked credentials" → PHS, siempre.*

---

**2 → B.** Entra ID P2.

- **R4** (elevación temporal con aprobación) → **PIM** → P2.
- **R5** (autoservicio, aprobación, revisión trimestral) → **Entitlement Management** + **Access Reviews** → P2.

Ambos están fuera de P1, así que P2 es el mínimo. ❌ C: E5 aporta Purview, no funciones de identidad. ❌ D: Permissions Management es un SKU separado que resuelve **R17**, no estos.
⚠️ Si el requisito incluyera además Lifecycle Workflows (joiner/mover/leaver), el SKU necesario sería **Entra ID Governance**, no P2 a secas. Aquí no aparece.

---

**3 → B.** Encryption scopes con CMK por container.

Sin encryption scopes, toda la cuenta comparte una única clave: revocarla afectaría a las tres divisiones a la vez, incumpliendo la independencia que pide **R6**.
❌ C: funciona, pero es rediseño innecesario existiendo la feature nativa, y choca con **C2**. ❌ D: client-side es la respuesta cuando la clave no puede existir en Azure, que aquí no se pide.

---

**4 → A + B.** Actualizar a E5 y hacer onboarding de dispositivos en Purview.

- **R7** (auto-labeling) → **E5**. E3 solo da etiquetado manual.
- **R8** (bloquear subidas a IA de terceros desde portátiles) → **Endpoint DLP**, que exige tanto E5 como el **onboarding del dispositivo** en Purview.

❌ C: **la trampa de la actualización de 2026.** La visibilidad y control sobre apps de IA de **terceros** requiere endpoint onboarding, **no** licencia de Copilot. La licencia de Copilot solo hace falta para ver el uso de **Microsoft 365 Copilot**.
❌ E: IRM detecta comportamiento, no bloquea la acción.
⚠️ Fíjate en que **C1** dice "minimizar costes *pero* se acepta actualizar cuando un requisito lo exija": el enunciado te está autorizando explícitamente el salto a E5. En casos de estudio, esa cláusula suele ser la pista.

---

**5 → A + C.**

- **R9** (exfiltración sobre HTTPS) → **A**: Firewall **Premium**. Standard filtra por FQDN pero no descifra TLS, así que no ve el contenido.
- **R10** (sin acceso desde internet, despliegues desde Pipelines) → **C**: Private Endpoints **+ deshabilitar el acceso público** **+ incluir el endpoint SCM**.

❌ B: VNet Integration es **outbound**; no aísla la app del acceso entrante. Es el error más común de este bloque. ❌ D: Service Endpoint va en dirección VNet→PaaS, la contraria a la que se necesita. ❌ E: las Access Restrictions filtran, pero la app conserva su IP pública, así que no cumple "no accesible desde internet".
⚠️ **El detalle del SCM es el que decide la pregunta:** si aíslas solo el endpoint principal y olvidas Kudu, los despliegues desde Azure Pipelines fallan.

---

**6 → C.** Istio.

🔑 **mTLS servicio-a-servicio + minimizar esfuerzo administrativo → service mesh gestionado.** Con **C3** (preferir servicios gestionados), el add-on de Istio para AKS refuerza la elección.
❌ D: Envoy sabe hacer mTLS, pero configurarlo microservicio a microservicio contradice frontalmente **C2**. ❌ A: Flux es GitOps. ❌ B: Dapr aporta componentes de aplicación, no seguridad de red.

---

**7 → C.** Azure Arc + Azure Policy.

**Azure Policy solo alcanza lo que está en el plano de control de ARM.** Arc es lo que proyecta EC2 y los servidores on-prem dentro de ese plano para poder aplicarles política.
❌ B: el conector de AWS da postura CSPM y recomendaciones, pero **no** permite aplicar Azure Policy. ❌ D: Blueprints está deprecado.
⚠️ Matiz que cae: el **escaneo agentless** de EC2 **no** requiere Arc (usa snapshots). Arc sí es necesario para **Azure Policy** y para MDE/detección runtime.

---

**8 →**

| Requisito | Solución |
|---|---|
| **R13** Kerberoasting, DCSync | **Microsoft Defender for Identity** |
| **R14** Descargas masivas en SaaS | **Microsoft Defender for Cloud Apps** |
| **R15** CEF on-prem + CloudTrail | **Sentinel** con servidor Syslog y AMA, más el conector de AWS S3 |

🔑 Las tres detecciones de identidad, aplicadas al caso:
- **Domain Controller, Kerberos, NTLM, DCSync** → Defender for Identity. Entra ID Protection es **ciego** a ataques on-prem puros; ese es exactamente el motivo por el que el incidente 2 del escenario no se detectó.
- **Post-login, dentro de la app SaaS, comportamiento masivo** → Defender for Cloud Apps. Resuelve el incidente 3.
- **Logs de terceros y de otras nubes** → Sentinel. Las Logic Apps **nunca** son el mecanismo de ingesta.

---

**9 → A.** Habilitar inmutabilidad → bloquear inmutabilidad → configurar MUA con Resource Guard en otro tenant.

Dos protecciones complementarias, no alternativas:
- **Inmutabilidad bloqueada** impide eliminar backups y acortar la retención. El paso de *habilitar* no basta, porque en ese estado un administrador aún puede deshabilitarla; el **lock** es lo que lo hace irreversible.
- **MUA con Resource Guard** impide ejecutar operaciones críticas sin aprobación de un segundo actor.

🔑 **"Ni siquiera un administrador global comprometido del tenant"** → el Resource Guard debe vivir en **otro tenant**, no en otra suscripción. Otra suscripción sigue estando bajo el alcance de ese Global Admin.
La secuencia lógica: no puedes bloquear lo que no has habilitado, y MUA se configura sobre un vault ya protegido.

---

**10 → B.** Entra Permissions Management (CIEM).

🔑 **Multinube (Azure + AWS) + identidades sobre-privilegiadas → CIEM**, con el Permission Creep Index como métrica.
❌ A: PIM gestiona roles de Entra y Azure con JIT, y **no cubre AWS ni GCP**. ❌ C: Attack Path muestra rutas de ataque, no analiza permisos en profundidad. ❌ D: MDA analiza sesiones SaaS.

---

# LO QUE ESTE CASO ENTRENA

Cada pregunta está resuelta por **una restricción concreta del escenario**, no por conocimiento general. Repásalas al revés:

| Pregunta | Restricción que la decide |
|---|---|
| 1 | "sin trust" + "sin servidores adicionales" → Cloud Sync |
| 2 | R4 y R5 están ambos fuera de P1 |
| 3 | "de forma independiente, sin afectar a las demás" |
| 4 | "terceros" en R8 → onboarding, no Copilot |
| 5 | "despliegues desde Azure Pipelines" → no olvidar SCM |
| 6 | "minimizando el esfuerzo administrativo" → descarta Envoy |
| 7 | EC2 y on-prem fuera de ARM → Arc |
| 8 | "on-premises" en R13 → nunca ID Protection |
| 9 | "ni siquiera un administrador global" → otro tenant |
| 10 | "simultáneamente en Azure y AWS" → descarta PIM |

**Método para el examen:** en cada pregunta de caso de estudio, antes de leer las opciones vuelve al requisito y subraya el adjetivo o la cláusula limitante. Ahí está la respuesta el 90% de las veces.
