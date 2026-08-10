# CASO DE ESTUDIO SC-100 → Wingtip Financial Group

**Gemelo estructural del caso Litware.** Mismos patrones, distinto disfraz. Hazlo a papel cerrado y compara tu razonamiento con el del caso original: si aciertas ambos por el mismo motivo, tienes el patrón dominado.

Tiempo objetivo: 30 minutos para 10 preguntas. No mires el solucionario hasta terminar.

---

# ESCENARIO

## Visión general

Wingtip Financial Group es una empresa de servicios financieros con sedes en **Chicago y Toronto**, 25 oficinas y empleados remotos que se conectan a las sedes mediante **VPN**. Ha crecido de forma acelerada por **fusiones y adquisiciones** en los últimos dos años, incluidas varias empresas **basadas en Alemania**.

## Entorno actual

- Un tenant de Entra ID sincronizado con un bosque de AD DS llamado `wingtip.com`, ligado a 18 suscripciones de Azure.
- **Entra Connect con Pass-through Authentication (PTA)**. **Password Hash Synchronization DESHABILITADO**. Password writeback habilitado.
- Todos los usuarios con licencia **Microsoft 365 E5**.
- El entorno incluye además **varios bosques AD DS, varios tenants de Entra y cientos de suscripciones** pertenecientes a las **filiales** adquiridas.

## Cambios planificados

- Crear una jerarquía de management groups **por cada tenant** de Entra.
- Diseñar una estrategia de **landing zones** para refactorizar el entorno de Azure y desplegar las cargas futuras.
- Implementar **Entra Application Proxy** para dar acceso seguro a las aplicaciones internas hoy accedidas por VPN.

## Requisitos

**Negocio:** minimizar la infraestructura on-premises adicional; minimizar el coste operativo y el overhead administrativo.

**Híbridos:** gestionar recursos on-prem desde Azure (Azure Policy para enforcement y compliance, change tracking + inventario de activos, patch management); **gestión centralizada de suscripciones entre tenants SIN mantener cuentas de invitado**.

**Sentinel:** aprovechar las capacidades SIEM/SOAR de Microsoft Sentinel y centralizar el SOC.

**Identidad:**
- Detectar ataques de fuerza bruta **dirigidos directamente a cuentas de AD DS**.
- Implementar **leaked credential detection** en el tenant de Entra de Wingtip.
- **Impedir que las cuentas de AD DS se bloqueen** por ataques de fuerza bruta dirigidos a cuentas de Azure AD.
- Implementar **gestión delegada** de usuarios y grupos, con delegación **por unidad de negocio**.

**Compliance regulatorio:** garantizar residencia de datos al recoger logs y telemetría de cada filial de **Norteamérica y Alemania**; usar definiciones **integradas de Azure Policy** para evaluar el compliance regulatorio; principio de least privilege.

**Landing zones:** enrutar todo el tráfico saliente a internet a través de **Azure Firewall en una suscripción dedicada** (hub); proporcionar un **secure score con alcance de cada landing zone**; garantizar que las VMs de cada zona se comuniquen con las App Services de la misma zona **por la red troncal de Microsoft, no por endpoints públicos**; minimizar la exfiltración; maximizar el ancho de banda; namespace DNS `wingtip.com`.

**Seguridad de aplicaciones:** identificar las apps internas que soportarán SSO vía App Proxy; **monitorizar y controlar el acceso a datos de SharePoint Online y Exchange Online en tiempo real**.

---
---

# PREGUNTAS

---

**1.** Debes recomendar una estrategia para asegurar el bosque `wingtip.com` que cumpla los requisitos de identidad. Empareja cada requisito con la solución:

| Requisito | Solución |
|---|---|
| Detectar fuerza bruta dirigida directamente a cuentas de AD DS | ? |
| Impedir que las cuentas de AD DS se bloqueen por ataques a cuentas de Azure AD | ? |

Opciones: *Microsoft Entra ID Protection · Microsoft Defender for Identity · Smart Lockout · Un account lockout policy en AD DS*

---

**2.** Debes recomendar una estrategia SIEM/SOAR que cumpla los requisitos híbridos, de Sentinel y de compliance regulatorio. Dos cajas:

**(a)** ¿Cómo debes segmentar los workspaces de Microsoft Sentinel?
- A. Un único workspace centralizado
- B. Por región y tenant de Azure AD
- C. Por suscripción
- D. Por unidad de negocio

**(b)** ¿Qué mecanismo permite operar los workspaces de todos los tenants desde el tenant gestor?
- A. Azure AD B2B collaboration
- B. Azure Lighthouse
- C. Cross-tenant synchronization
- D. Cuentas de invitado con rol Reader

---

**3.** Debes recomendar una solución de seguridad **multi-tenant e híbrida** que cumpla los requisitos de negocio e híbridos. ¿Qué dos componentes incluyes?

- A. Azure Lighthouse
- B. Azure Arc
- C. Azure Stack Hub
- D. Azure AD B2B
- E. Azure Stack Edge

---

**4.** Debes recomendar una solución para asegurar las landing zones que cumpla los requisitos de landing zone y de negocio. ¿Qué configuras en cada landing zone?

- A. Un ExpressRoute gateway
- B. Microsoft Defender for Cloud
- C. Una Azure Private DNS zone
- D. Azure DDoS Protection Standard

---

**5.** Debes cumplir el requisito de **leaked credential detection** en el tenant de Wingtip. ¿Qué debes hacer?

- A. Habilitar Password Hash Synchronization (puede ser como respaldo, manteniendo PTA)
- B. Migrar de PTA a Federation con AD FS
- C. Habilitar Smart Lockout
- D. Desplegar Microsoft Defender for Identity

---

**6.** Debes implementar la **gestión delegada de usuarios y grupos por unidad de negocio**. ¿Qué recomiendas?

- A. Administrative Units
- B. Catalogs en Entitlement Management
- C. Access Reviews
- D. Roles personalizados de RBAC de Azure

---

**7.** Debes garantizar que las VMs de una landing zone se comuniquen con las App Services de la misma zona **por la red troncal de Microsoft y no por endpoints públicos**, minimizando la exfiltración.

- A. Service Endpoints hacia la App Service
- B. Private Endpoint + Azure Private Link con Private DNS zone
- C. VNet Integration en la App Service
- D. Un NSG que bloquee el tráfico público

---

**8.** Debes **monitorizar y controlar el acceso a datos de SharePoint Online y Exchange Online en tiempo real**. ¿Qué recomiendas?

- A. Microsoft Purview Information Protection
- B. Microsoft Defender for Cloud Apps con session policies y Conditional Access App Control
- C. Microsoft Defender for Cloud
- D. Insider Risk Management

---

**9.** Debes reemplazar el acceso por VPN a las aplicaciones **web internas** y proporcionarles SSO. ¿Qué recomiendas?

- A. Entra Application Proxy
- B. Entra Internet Access
- C. Una VPN de punto a sitio
- D. Azure Bastion

---

**10.** Debes enrutar y **centralizar la inspección** de todo el tráfico saliente a internet de las landing zones a través del Azure Firewall del hub. ¿Qué configuras en cada landing zone?

- A. Un Service Endpoint
- B. Forced tunneling con una UDR 0.0.0.0/0 hacia el Firewall del hub
- C. Un NAT Gateway
- D. Un segundo Azure Firewall por landing zone

---
---

# SOLUCIONARIO

---

**1 →**

| Requisito | Solución |
|---|---|
| Fuerza bruta directa contra AD DS | **Microsoft Defender for Identity** |
| Evitar bloqueo de cuentas AD DS por ataques a Azure AD | **Smart Lockout** |

🔑 **"Directamente sobre AD DS / DC / Kerberos" → Defender for Identity**, nunca Identity Protection (que es ciego a lo on-prem puro). **"Evitar que un ataque cloud bloquee cuentas on-prem" → Smart Lockout**, que filtra el password spray en Entra antes de reenviarlo a AD. Un account lockout policy en AD DS haría lo contrario: facilitar el bloqueo.
⚠️ Ojo: algunos bancos públicos imprimen mal esta pregunta.

---

**2 → (a) B · (b) B.** Segmentar por región y tenant + Azure Lighthouse.

- **Segmentar por región/tenant** → lo exige la **residencia de datos**: filiales en Norteamérica y Alemania, cuyos logs deben quedarse en su región. Un workspace centralizado violaría el requisito regulatorio.
- **Azure Lighthouse** → gestión **cross-tenant delegada SIN guests**, exactamente el requisito. B2B crearía invitados.

🔑 *"Datos de cada país en su región" → segmentar. "Cross-tenant sin guests" → Lighthouse.*

---

**3 → A + B.** Azure Lighthouse + Azure Arc.

- **Lighthouse** → gestión cross-tenant sin guests de los cientos de suscripciones de las filiales.
- **Azure Arc** → proyecta los recursos **on-prem** en el plano de control de Azure para aplicarles **Azure Policy, change tracking, inventario y patch management** (los requisitos híbridos literales).

❌ Stack Hub/Edge son plataformas de cloud híbrida / gateway de almacenamiento, no gestión de recursos existentes. ❌ B2B crea guests.

🔑 *On-prem gestionado desde Azure → Arc. Ver/gestionar entre tenants sin guests → Lighthouse. Las dos juntas.*

---

**4 → B.** Microsoft Defender for Cloud.

El gancho es *"secure score scoped to the landing zone"* → el Secure Score es de Defender for Cloud.
❌ Los distractores (ExpressRoute, Private DNS, DDoS) resuelven otros requisitos de red de la landing zone, no el de postura de seguridad.

🔑 *"Secure score" → Defender for Cloud.*

---

**5 → A.** Habilitar Password Hash Synchronization.

**PHS es la única vía de leaked credential detection.** Wingtip lo tiene deshabilitado y usa PTA, así que hay que habilitarlo → se puede como respaldo, conviviendo con PTA como método primario.
❌ B: Federation añade infraestructura on-prem, contra el requisito de negocio. ❌ C: Smart Lockout mitiga password spray, no detecta credenciales filtradas. ❌ D: Defender for Identity detecta ataques on-prem, no credenciales filtradas en el tenant.

🔑 *"Leaked credential detection" → PHS, sin excepción.*

---

**6 → A.** Administrative Units.

Delegan la gestión de usuarios y grupos por ámbito (unidad de negocio).
❌ B: los Catalogs delegan la **creación de access packages**, no la gestión de usuarios/grupos. ❌ C: Access Reviews revisan, no delegan gestión.

🔑 *"Delegar gestión de usuarios/grupos por unidad de negocio" → Administrative Units. "Delegar creación de access packages" → Catalogs.* No confundir.

---

**7 → B.** Private Endpoint + Private Link con Private DNS zone.

*"Por la red troncal de Microsoft, no por endpoints públicos"* + *"minimizar exfiltración"* = **Private Link**. El tráfico va por el backbone y desaparece la ruta pública.
❌ A: Service Endpoint sigue usando la IP pública del servicio (aunque optimizada) y no minimiza exfiltración igual. ❌ C: VNet Integration es outbound de la app, no la comunicación privada VM→App Service.

🔑 *"Backbone de Microsoft, no público" + "minimizar exfiltración" → Private Link.*

---

**8 → B.** Defender for Cloud Apps con session policies + Conditional Access App Control.

*"En tiempo real"* + *"controlar acceso a datos de SharePoint/Exchange Online"* = **MDA**. El control en sesión (bloquear descarga, etc.) necesita Conditional Access App Control, que enruta la sesión por el proxy inverso de MDA.
❌ A: las etiquetas protegen el documento, no controlan la sesión en tiempo real. ❌ C: Defender for Cloud es postura de recursos cloud, no control de sesión SaaS.

🔑 *"Tiempo real" + "control de acceso a datos SaaS" → Defender for Cloud Apps.*

---

**9 → A.** Entra Application Proxy.

Las apps internas son **web (HTTP/HTTPS)** y el caso pide SSO → Application Proxy las publica sin VPN.
❌ B: Internet Access es filtrado de salida, no publicación de apps. ❌ D: Bastion da acceso a VMs, no publica apps web.
⚠️ Si el enunciado mencionara protocolos no-HTTP (SSH, RDP, SQL), la respuesta cambiaría a **Entra Private Access**.

🔑 *App web interna + SSO + reemplazar VPN → Application Proxy. Protocolo no-HTTP → Private Access.*

---

**10 → B.** Forced tunneling con UDR 0.0.0.0/0 hacia el Firewall del hub.

Topología hub-and-spoke con **inspección de egress centralizada**: cada landing zone enruta su tráfico saliente al Firewall del hub mediante una UDR por defecto.
❌ D: un Firewall por zona rompe la centralización y dispara el coste. ❌ C: NAT Gateway da IP de salida, no inspección.

🔑 *"Inspección centralizada de egress en hub-and-spoke" → forced tunneling (UDR) al Firewall del hub.*

---

# COMPARA CON LITWARE

Cada pregunta aquí tiene su gemela exacta en el caso Litware. Si has acertado ambas **por el mismo motivo**, el patrón está dominado:

| Wingtip | Litware | Patrón |
|---|---|---|
| 1 | 306 | Defender for Identity + Smart Lockout (no Identity Protection) |
| 2 | 307 | Segmentar por región + Lighthouse |
| 3 | 308 | Lighthouse + Arc |
| 4 | 309 | Defender for Cloud (secure score) |
| 5 | hook | PHS para leaked creds |
| 6 | hook | Administrative Units |
| 7 | hook | Private Link |
| 8 | hook | Defender for Cloud Apps |
| 9 | hook | Application Proxy |
| 10 | hook | Forced tunneling al Firewall del hub |

**Los dos ejes que lo deciden casi todo, otra vez:** multi-tenant sin guests → **Lighthouse**; on-prem gestionado desde Azure → **Arc**. Y el gancho regulatorio: residencia de datos multi-país → **segmentar**, nunca centralizar.
