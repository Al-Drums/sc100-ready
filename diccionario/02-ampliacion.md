# AMPLIACIÓN DEL DICCIONARIO SC-100 — Conceptos añadidos tras las últimas sesiones

Cómo usarla: cada sección indica **en qué bloque del diccionario va**. La sección 0 son **correcciones a entradas que ya tienes** (una de ellas cambia lo que está escrito). El resto son conceptos nuevos.

---

## 0. CORRECCIONES A ENTRADAS EXISTENTES ⚠️

### GitHub Advanced Security / Dependabot — CORRIGE la entrada actual
Lo que tienes escrito ("GHAS = paquete que integra SAST + Secret Scanning + dependency scanning") está desactualizado. Corrección:
- **Dependabot (alerts, security updates, version updates) es GRATIS en TODOS los repos**, públicos y privados. En públicos viene activado; en privados solo hay que habilitarlo. **No requiere un tier concreto ni Advanced Security.**
- Lo que **sí** es de pago en repos **privados**: **code scanning (CodeQL)** y **secret scanning** → requieren **Advanced Security**.
- GHAS se ha separado en licencias independientes (protección de secretos por un lado, seguridad de código por otro). Si el examen aún lo trata como paquete único, respóndele en sus términos.
- Regla: **Dependabot gratis en todas partes; CodeQL y secret scanning de pago en privados.**

### Secure Score — MATIZA la entrada de Defender for Cloud
Existen **dos modelos** de secure score y el factor de riesgo cambia entre ellos:
- **Clásico** (portal de Azure): solo puntúan las recomendaciones **integradas del MCSB**; el cálculo es ratio de recursos sanos ponderado por control. La priorización por riesgo **no** afecta.
- **Risk-based / Cloud Secure Score** (portal de Defender, con Defender CSPM): incorpora **criticidad del activo y factores de riesgo** al cálculo.
- En cualquiera de los dos: los estándares regulatorios (NIST/PCI/ISO), los personalizados y las recomendaciones en Preview **no** puntúan. Discrimina por **dónde vive la pregunta**: portal de Defender + CSPM → risk-based; portal de Azure → clásico.

---

## 1. IDENTIDAD DE APPS (añadir al bloque)

### Application object vs Service Principal — regla cross-tenant
- **Application object** (app registration): el "molde" global, vive **solo en el tenant de origen** (el del fabricante).
- **Service principal** (enterprise application): la instancia local, se crea **en cada tenant que consume la app**.
- Trampa: si usas una app **multi-tenant de otro tenant**, lo que creas en el tuyo es un **service principal**, no un application object (ese ya existe en el tenant del fabricante). Pregúntate siempre: *¿en qué tenant estoy y qué me falta ahí?*

### Managed Identity — user-assigned vs system-assigned
- **System-assigned:** ligada al ciclo de vida del recurso; **muere con él**.
- **User-assigned:** recurso independiente; **sobrevive a la eliminación/recreación** del recurso.
- Trampa: **scale sets o VMs que se redepliegan constantemente → user-assigned** (la creas y le das acceso al Key Vault una vez, y la reasignas a cada VM nueva; además minimiza los principals con acceso). El token se pide al endpoint local **IMDS** (`169.254.169.254`), sin almacenar credenciales.
- Sustituir una cuenta de servicio con credenciales reutilizables (que un humano puede copiar) → **managed identity** (el token no es reutilizable fuera de la carga).

---

## 2. AISLAMIENTO DE RED Y APP SERVICES (añadir al bloque)

### Hybrid Connections
- Conecta una App Service a un recurso **on-premises** (puerto TCP estático) **sin VPN ni ExpressRoute**. El Hybrid Connection Manager se instala on-prem y abre un túnel **saliente** por TCP 443 → cero endpoints entrantes expuestos.
- Trampa: "App Service + on-prem + sin VPN/ER" → Hybrid Connections. **VNet Integration no sirve** (es outbound hacia la VNet, y la VNet no llega a on-prem sin VPN/ER). Private Endpoint es la dirección contraria (inbound hacia la app).

### Azure Files — granularidad de la autorización
- **Dos capas que se evalúan juntas, gana la más restrictiva:**
  - **Share-level → Azure RBAC** (roles Storage File Data SMB Share Reader/Contributor/Elevated Contributor). **RBAC NO baja de share.**
  - **Directory/file-level → Windows ACLs (NTFS).** Para permisos por carpeta o archivo dentro del mismo share.
- **Elevated Contributor** = el rol de share que además permite **modificar las ACLs NTFS**.
- Trampa: "permisos distintos por carpeta dentro del mismo share" → RBAC de share + **ACLs NTFS**, nunca "RBAC por carpeta" (no existe). Si no puedes sincronizar el AD on-prem con Entra → **default share-level permission** + ACLs.

---

## 3. CIFRADO EN STORAGE / SQL / SYNAPSE (añadir al bloque)

### Infrastructure encryption (doble cifrado) — "segunda capa"
- Es una **segunda capa** de cifrado en reposo, adicional al cifrado de servicio por defecto.
- Trampa, Synapse: **Serverless SQL pool → Infrastructure encryption** (sobre el Storage Account del Data Lake, porque no tiene storage propio ni TDE). **Dedicated SQL pool → TDE** como segunda capa (sobre el cifrado de servicio del storage).
- Es un concepto **distinto** al de CMK: aquí piden "segunda capa", no "clave gestionada por el cliente".

### Managed HSM — solo RBAC
- Añadir a la entrada existente: Managed HSM **solo admite autorización RBAC** (no access policies). El discriminador para elegirlo sigue siendo **"single tenancy / hardware dedicado"**, no el nivel FIPS (que también da Premium).

---

## 4. RANSOMWARE Y BACKUP (añadir / matizar)

### DART — las DOS secuencias (matiza la entrada existente)
Tu diccionario tiene la de **recuperación**. Falta la de **investigación**, y el examen las mezcla:
- **Investigación** ("investigate / optimize investigations"): 1) evaluar situación y alcance → 2) identificar qué **LOB apps** están caídas → 3) identificar el proceso de recuperación del compromiso.
- **Recuperación** ("recovery plan / first step"): 1) **deshabilitar OneDrive sync + ActiveSync** (contención) → 2) investigar → 3) erradicar preservando evidencia → 4) recuperar a equipos limpios.
- Regla: *entender antes de priorizar, priorizar antes de actuar.* Si dice "investigate" → primera. Si dice "first step / recovery" → contención.

### MUA / Resource Guard — dos matices nuevos
- **Conteo:** el número mínimo de Resource Guards = **número de conjuntos distintos de operaciones protegidas**. Un Resource Guard protege muchos vaults, pero un vault se asocia a uno. Vaults con el mismo conjunto de operaciones comparten Resource Guard.
- **Rol del equipo de backup:** al equipo de backup se le da **solo Reader** sobre el Resource Guard (no puede auto-aprobar operaciones críticas). Un actor separado con Contributor aprueba. Máxima seguridad = Resource Guard en **otro tenant**.

### MABS vs MARS
- **MARS (agente):** backup de ficheros/carpetas/system state **directo a Azure** → el servidor necesita salida a internet.
- **MABS (servidor):** servidor de backup **local**; los servidores respaldan contra él y MABS envía a Azure. Soporta **app-aware** (SQL, SharePoint, Exchange, VMs).
- Trampa: "servidor **sin salida a internet**" o "backup app-aware" → **MABS** + Recovery Services vault. MARS solo para ficheros directos a Azure.

### Ransomware en Microsoft Teams
- Teams **no tiene almacenamiento propio**: mensajes en **Exchange Online**, ficheros en **SharePoint/OneDrive**.
- Trampa: mensajes de canal → **Single Item Recovery** (EXO); ficheros de canal → **Files Restore** (SPO). La protección es la de la carga subyacente. (Mismo patrón mental que Synapse serverless: los datos viven en otro sitio.)

---

## 5. RED Y PERÍMETRO (añadir al bloque)

### AzureBastionSubnet como objetivo de NSG
- **AzureBastionSubnet SÍ admite NSG** (la que NO lo admite es **AzureFirewallSubnet**). Microsoft recomienda usarlo, pero **debes configurar todas las reglas requeridas** o Bastion se rompe silenciosamente.
- Requisitos: nombre exacto `AzureBastionSubnet`, **/26 mínimo**, sin route tables ni delegaciones, IP pública Standard estática.
- Trampa central: **3389 y 22 NO van en entrada** sobre esa subnet. El usuario entra por **443**; el RDP/SSH lo origina Bastion **en salida** hacia la VM (destino VirtualNetwork). Si el portal carga pero la sesión no abre → falta la regla de **salida** 3389/22.

---

## 6. AKS, CONTENEDORES Y SERVICE MESH (bloque nuevo)

Tu diccionario tiene Flux y Gatekeeper. Faltan las tres que se confunden con ellos:

| Herramienta | Qué es | Resuelve |
|---|---|---|
| **Istio** | Service mesh | Comunicación **servicio-a-servicio**: mTLS automático, políticas de tráfico |
| **Envoy** | Proxy (sidecar) | Componente que Istio usa por debajo; solo, exige config manual |
| **Dapr** | Runtime de componentes | State, pub/sub, invocación; modelo de programación, no seguridad de red |
| **Flux** | GitOps | Sincroniza manifiestos Git→cluster (ya lo tienes) |
| **Gatekeeper** | Admission control (OPA) | Impide desplegar lo que no cumple política (ya lo tienes) |

- Trampa: **mTLS servicio-a-servicio + mínimo esfuerzo → Istio.** Envoy sabe hacer mTLS pero configurarlo a mano contradice "minimizar esfuerzo"; Istio inyecta los sidecars Envoy solo. En AKS existe el **Istio add-on gestionado**.
- Recordatorio: restringir el **API server público de AKS** por IP → **Authorized IP Ranges** (feature nativa, no Service/Private Endpoint).

---

## 7. GOBERNANZA — cuándo es Azure Policy (añadir al bloque de frameworks)

- **Discriminador de una línea: Defender for Cloud recomienda y alerta; Azure Policy aplica y bloquea.**
- Azure Policy actúa sobre el **plano de control de Azure (ARM)**: recursos desplegados, con independencia de cómo se desplegaran.
  - "Ensure/maintain compliance", "enforce", "prevent deployment of" → **Azure Policy** (efecto **Deny**).
  - "Corregir configuración automáticamente" → **DeployIfNotExists / Modify** + una **remediation task** (para los recursos ya existentes).
  - Recursos no-Azure (EC2/GCE/on-prem) → **Azure Policy + Azure Arc**.
- Trampa: que el enunciado mencione **Azure DevOps no lo convierte en pregunta de DevOps**. Si el objeto es que las **apps ya desplegadas** cumplan el MCSB → Azure Policy. Si el objeto es visibilidad de **repos/pipelines** → DevOps security en Defender for Cloud. Si es revisión de código antes del merge → branch policies.

---

## 8. GLOBAL SECURE ACCESS — compliant network (añadir al bloque de identidad/red)

- El **compliant network check** es una condición de Conditional Access que verifica que el tráfico llega **a través del servicio Global Secure Access** (sustituye a las named locations por IP).
- Un dispositivo/usuario cumple si su tráfico entra a GSA por **uno de dos caminos**: el **cliente GSA** instalado, o una **remote network** configurada **y asignada a la política**.
- Trampa: **lo que decide no es "¿está el cliente instalado?" sino "¿está ese camino asignado a la política?".** Desplegar el cliente sin asignar su camino no basta; una remote network no cubre un dispositivo si no está asignada. (La explicación que circula en algunos bancos públicos es confusa aquí; ancla el mecanismo, no su respuesta.)
- Recordatorio ya tuyo: **Private Access** = hacia recursos privados de la organización; **Internet Access** = hacia internet/SaaS (y Tenant Restrictions v2).

---

## 9. IDENTITY GOVERNANCE — Information Barriers (añadir al bloque)

- **Information Barriers (IB):** restringe **quién puede comunicarse con quién** en Teams/SharePoint/OneDrive. Requiere **E5**.
- Cálculo: **segmentos = grupos con comportamiento de comunicación distinto** (si dos grupos se comportan igual, se fusionan en uno); **políticas = número de segmentos que necesitan restricción** (un segmento sin restricciones no consume política).
- Trampa: contar un segmento por cada grupo organizativo, o crear una política "por simetría" para el grupo sin restricciones.
- No es Conditional Access (bloquea sign-ins, no comunicación entre personas) ni DLP (bloquea contenido, no interlocutores).

---

## 10. PURVIEW (añadir al bloque)

### Escaneo: Data Map vs Information Protection scanner
- **Blobs / fuentes cloud de Azure → Microsoft Purview Data Map** (escaneo cloud-native).
- **Carpetas compartidas on-prem / file servers → Information Protection scanner** (componente que escanea, clasifica y etiqueta ficheros on-prem).
- Trampa: cloud → Data Map; file share on-prem → IP scanner.

### Step-up auth sobre descargas de SharePoint
- Trampa: documento basado en **formulario/plantilla predefinida** → método de inspección **Fingerprint** (document fingerprinting). Disparar step-up auth a mitad de sesión → opción de Conditional Access **Authentication context** (lo que integra la session policy de MDA con CA).

### Bloquear descarga en dispositivo no gestionado
- Trampa: "bloquear descarga en unmanaged device permitiendo ver" → **Conditional Access + Defender for Cloud Apps session policy**, las **dos**. La CA enruta la sesión al proxy inverso (CA App Control); la session policy de MDA bloquea la descarga. Una sola no es respuesta completa.

---

## 11. SECOPS / MULTI-TENANT — Azure Lighthouse (añadir al bloque de SecOps)

- **Azure Lighthouse:** gestión **delegada entre tenants SIN crear cuentas de invitado**. Es la respuesta a "gestionar cientos de suscripciones de filiales / operar el SOC de varios tenants / cross-tenant sin guests".
- Distinción clave frente a Azure Arc: **Lighthouse = ver y gestionar entre tenants; Arc = gestionar recursos on-prem/otra nube desde Azure.** En escenarios de M&A suelen ir **las dos juntas**.
- **Residencia de datos multi-país → segmentar los workspaces de Sentinel por región/tenant**, nunca centralizar en uno.

---

## 12. AZURE API MANAGEMENT (bloque nuevo)

- **Product** = agrupación lógica de varias APIs; los desarrolladores **se suscriben a Products**. **Subscription** = la relación dev↔product, que genera las **subscription keys**.
- Trampa: distintos grupos con distintos conjuntos de APIs y rate limits → un **Product por grupo** + **rate limit a nivel de Product**. Tier de producción sin sobrecoste → **Standard v2**.
- Trampa: en el modelo de gestión, **RBAC → scope Subscriptions** (quién administra), **keys → scope Products** (quién consume). *Gestión→Subscriptions, consumo→Products.*
- Autenticar APIM contra otros servicios Azure → **managed identity**, nunca service principal con secreto (MCSB best practice; ya lo tienes).

---

## 13. CONDITIONAL ACCESS — condición de ubicación (añadir al bloque de CA)

- Trampa: "bloquear acceso desde una **lista de países**" → **Conditional Access con named locations** (condición **determinista** conocida de antemano). **No** es ID Protection, que reacciona a **riesgo calculado** (viaje imposible, IP anónima). Regla: *¿condición conocida (país/dispositivo/app/IP) o riesgo calculado por Microsoft?* → CA vs ID Protection.

---

## RESUMEN DE LO AÑADIDO (checklist)

Correcciones: **Dependabot** (gratis en privados), **Secure Score** (dos modelos).
Conceptos nuevos: user-assigned vs system-assigned MI · Hybrid Connections · Azure Files RBAC+ACL · infrastructure encryption (Synapse segunda capa) · Managed HSM RBAC-only · DART dos secuencias · Resource Guards conteo + rol Reader · MABS vs MARS · Teams ransomware · AzureBastionSubnet NSG · Istio/Envoy/Dapr · discriminador Azure Policy · GSA compliant network · Information Barriers · Data Map vs IP scanner · SharePoint step-up (Fingerprint + auth context) · descarga unmanaged (CA + MDA) · Azure Lighthouse · APIM (Products/Subscriptions) · CA condición de país.
