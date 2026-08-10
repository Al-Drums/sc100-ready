# SIMULACRO CAPSTONE SC-100 — 20 preguntas

**Este es el repaso final: cada pregunta ataca un concepto que has fallado o pedido.**
Tiempo objetivo: 40 minutos. No mires el solucionario hasta terminar. Apunta también las dudas.

Cobertura: cada pregunta lleva entre corchetes el tema/fallo que entrena, para que al corregir sepas qué revisar.

---

## PREGUNTAS

---

**1.** [Synapse] Un Azure Synapse Analytics workspace usa un **Serverless** SQL pool para consultar datos de un Data Lake. Compliance exige cifrado con customer-managed key.

¿Dónde se aplica la CMK?

- A. TDE con CMK en el propio pool serverless
- B. En el Storage Account del Data Lake subyacente
- C. Recreando el workspace con CMK habilitada
- D. Always Encrypted con secure enclaves sobre el pool

---

**2.** [Aislamiento App Service] Una App Service debe quedar **inaccesible desde internet** y alcanzable solo desde la VNet corporativa. Los despliegues se hacen desde Azure Pipelines.

¿Qué **tres** acciones necesitas? (Elige tres.)

- A. VNet Integration
- B. Private Endpoint en el endpoint principal y en el SCM/Kudu
- C. Deshabilitar el acceso público
- D. Service Endpoint hacia la App Service
- E. Private DNS Zone
- F. Access Restrictions por Service Tag

---

**3.** [Private Access vs App Proxy] Usuarios remotos necesitan acceder, sin VPN, a una app web interna, a un servidor SSH y a una instancia de SQL Server on-premises.

- A. Entra Application Proxy para los tres
- B. Entra Private Access para los tres
- C. Application Proxy para la web, Private Access para SSH y SQL
- D. Entra Internet Access para los tres

---

**4.** [Silos vs Protected Users] Debes impedir que las cuentas de Tier 0 puedan autenticarse desde cualquier equipo que no sea una PAW designada.

- A. El grupo Protected Users
- B. Authentication Policy Silos
- C. User Rights Assignment vía GPO
- D. Windows LAPS

---

**5.** [CA ubicación] Debes impedir que los usuarios accedan a las enterprise applications desde una lista concreta de países.

- A. Sign-in risk policies en Entra ID Protection
- B. Una política de Conditional Access con named locations por país
- C. Activity policies en Defender for Cloud Apps
- D. User risk policies en Entra ID Protection

---

**6.** [DART investigación] Debes **optimizar la investigación** de un incidente de ransomware según el enfoque DART. ¿En qué orden?

1. Identificar el proceso de recuperación del compromiso
2. Evaluar la situación e identificar el alcance
3. Identificar qué aplicaciones de negocio están no disponibles

- A. 2 → 3 → 1
- B. 1 → 2 → 3
- C. 3 → 2 → 1
- D. 2 → 1 → 3

---

**7.** [SharePoint download] Usuarios autenticados acceden a SharePoint Online desde dispositivos **no gestionados**. Debes impedir la descarga de archivos permitiendo la visualización en navegador.

¿Qué **dos** servicios necesitas?

- A. Conditional Access
- B. Purview Information Protection
- C. Defender for Cloud Apps
- D. Entra Application Proxy
- E. Intune app protection policies

---

**8.** [Step-up SharePoint] Documentos basados en un **formulario predefinido** con datos confidenciales en SharePoint. Debes disparar step-up authentication al descargarlos, vía session policy de Defender for Cloud Apps.

¿Qué método de inspección de DCS y qué opción de Conditional Access?

- A. Keyword matching + Sign-in risk
- B. Fingerprint + Authentication context
- C. EDM + Device compliance
- D. Regex + Named location

---

**9.** [Service principal cross-tenant] Una aplicación multi-tenant llamada App1 está registrada en el tenant `partner.com`. Necesitas que los usuarios de tu tenant `contoso.com` puedan autenticarse en App1.

¿Qué creas en contoso.com?

- A. Un application object
- B. Una system-assigned managed identity
- C. Un service principal (enterprise application)
- D. Una user-assigned managed identity

---

**10.** [IA de terceros] Purview protege EXO y SPO. Los empleados pegan datos de clientes en webs de IA generativa de terceros desde Windows. Debes bloquear esos prompts y escanear los enviados.

¿Qué necesitas y qué va primero?

- A. Sensitivity labels + Insider Risk; primero auto-labeling
- B. Endpoint DLP + DSPM for AI; primero licenciar Copilot
- C. Endpoint DLP + DSPM for AI; primero onboarding de dispositivos en Purview
- D. Conditional Access + MDA session policy; primero crear la session policy

---

**11.** [GSA compliant network] Despliegas Entra Internet Access con perfil M365 y una CA que exige compliant network check. Asignas una **remote network** a la política solo para la Oficina A, y despliegas el **cliente GSA** en todos los dispositivos de la Oficina B, pero **la política no tiene asignado el camino de cliente de la Oficina B**.

¿Desde qué oficinas se cumple el compliant network check?

- A. Solo Oficina A
- B. Solo Oficina B
- C. Ambas
- D. Ninguna

---

**12.** [Service mesh] Despliegas microservicios en AKS. Necesitas gestionar secretos, cifrado, comunicación servicio-a-servicio con mTLS y minimizar el esfuerzo administrativo.

- A. Flux
- B. Envoy configurado manualmente
- C. Dapr
- D. Istio

---

**13.** [Azure Policy discriminador] Despliegas App Services mediante Azure DevOps. Debes asegurar que las apps desplegadas **mantengan el cumplimiento** de las recomendaciones del MCSB.

- A. DevOps security en Defender for Cloud
- B. Defender for App Service
- C. Azure Policy
- D. Una branch policy en Azure DevOps

---

**14.** [Firewall vs WAF] Debes detectar exfiltración hacia dominios maliciosos sobre tráfico **HTTPS** saliente desde las spokes.

- A. Azure Firewall Standard con reglas de aplicación por FQDN
- B. Azure Firewall Premium con TLS inspection e IDPS
- C. Front Door con WAF y OWASP CRS
- D. NSG con Service Tags de denegación

---

**15.** [Information Barriers] Tienes banca de inversión, análisis y un equipo legal. Banca y análisis no pueden comunicarse; legal puede hablar con ambos. ¿Mínimo de segmentos y políticas?

- A. 2 segmentos, 2 políticas
- B. 3 segmentos, 2 políticas
- C. 3 segmentos, 3 políticas
- D. 4 segmentos, 4 políticas

---

**16.** [Hybrid Connections] Una App Service debe conectarse a un SQL Server on-premises en un puerto TCP estático. No hay VPN ni ExpressRoute, y hay que minimizar los endpoints accesibles desde internet.

- A. VNet Integration
- B. Hybrid Connections
- C. Private Endpoint en la App Service
- D. NAT Gateway

---

**17.** [Managed HSM] Debes gestionar los certificados de 20 web apps en una solución de **tenancy propia**, FIPS 140-2 Level 3, least privilege, coste y esfuerzo mínimos.

- A. Key Vault Premium con access policies
- B. Key Vault Premium con RBAC
- C. Managed HSM con un único vault y RBAC
- D. Un Managed HSM por web app

---

**18.** [Resource Guards] Dos Recovery Services vaults requieren MUA para {disable soft delete, remove MUA, disable immutability}. Dos Backup vaults requieren MUA para {disable soft delete, remove MUA}. ¿Número mínimo de Resource Guards?

- A. 1
- B. 2
- C. 3
- D. 4

---

**19.** [Teams ransomware] Debes mitigar ransomware sobre los **mensajes** de canales de Teams y los **ficheros** de canales de Teams.

- A. Mensajes → Files Restore; Ficheros → Single Item Recovery
- B. Mensajes → Single Item Recovery; Ficheros → Files Restore
- C. Ambos → Azure Backup del tenant
- D. Mensajes → Retention labels; Ficheros → Immutable storage

---

**20.** [Purview scanning + Azure Files] Dos partes:

**(a)** Debes escanear blobs de Azure y carpetas compartidas on-prem con SITs personalizados. Empareja: Blobs → ? · File shares on-prem → ?
Opciones: *Data Map · Information Protection scanner*

**(b)** En un Azure File share necesitas que RRHH lea/escriba en `\RRHH` pero no vea `\Finanzas`, dentro del mismo share. ¿Con qué se consigue la granularidad por carpeta?

- A. Asignando roles RBAC por carpeta
- B. Con Windows ACLs (NTFS) por carpeta, sobre el rol RBAC de share
- C. Con Conditional Access por grupo
- D. Con encryption scopes por carpeta

---
---

# SOLUCIONARIO

---

**1 — B.** CMK en el Storage Account del Data Lake.
🔑 **El pool serverless no tiene almacenamiento propio → no tiene TDE.** Los datos viven en el Data Lake, y ahí se cifra.
⚠️ **Este es un error frecuente.** Truco: ante "Synapse + cifrado", busca la palabra *Serverless* o *Dedicated*. Serverless → Storage Account. Dedicated → workspace encryption, y **solo al crear el workspace** (recrear si ya existe).

**2 — B, C, E.** Private Endpoint (principal + SCM) + deshabilitar acceso público + Private DNS Zone.
🔑 El Private Endpoint da IP privada (inbound), pero **la URL pública sigue respondiendo hasta que deshabilitas el acceso público** — dos acciones distintas. La Private DNS Zone es obligatoria con cualquier Private Endpoint. Y el **SCM/Kudu** debe incluirse o los despliegues desde Pipelines fallan.
❌ A: VNet Integration es **outbound**, no aísla. ❌ D: Service Endpoint va VNet→PaaS, dirección contraria. ❌ F: filtra pero conserva IP pública.

**3 — B.** Private Access para los tres.
🔑 **SSH y SQL no son HTTP → Application Proxy queda descartado de inmediato** (solo publica HTTP/HTTPS). Private Access (ZTNA) cubre cualquier TCP/UDP e incluye la web app, así que una sola solución resuelve todo. Meter App Proxy en paralelo sería sumar una solución legacy sin necesidad.

**4 — B.** Authentication Policy Silos.
🔑 **"Desde dónde puede autenticarse" → Authentication Policy Silos.** Protected Users es una restricción global del grupo (sin NTLM, sin cache, TGT corto), no controla el origen. User Rights Assignment controla el **tipo** de logon (interactive/RDP), no el equipo.

**5 — B.** Conditional Access con named locations por país.
🔑 **País/ubicación es una condición determinista conocida de antemano → Conditional Access.** ID Protection reacciona a **riesgo calculado** (viaje imposible, IP anónima), no a "esta lista de países". MDA activity policies actúan post-login dentro de la sesión.

**6 — A.** 2 → 3 → 1.
Evaluar situación y alcance → identificar LOB apps caídas → identificar el proceso de recuperación.
🔑 **Entender antes de priorizar, priorizar antes de actuar.**
⚠️ Recuerda las **dos secuencias DART**: esta es la de **investigación**. La de **recuperación** empieza distinto: *deshabilitar OneDrive sync + ActiveSync* (contención). Si el enunciado dice "investigate" → esta; si dice "recovery / first step" → contención.

**7 — A + C.** Conditional Access + Defender for Cloud Apps.
🔑 **"Bloquear descarga en dispositivos no gestionados" → siempre las dos:** la CA enruta la sesión al proxy inverso (Conditional Access App Control) usando el estado del dispositivo, y la **session policy de MDA** bloquea la descarga permitiendo ver. Una sola no es respuesta completa.

**8 — B.** Fingerprint + Authentication context.
🔑 **"Formulario predefinido / plantilla" → document Fingerprint.** **"Step-up auth condicional a una acción concreta a mitad de sesión" → Authentication context**, que es lo que integra la session policy de MDA con Conditional Access.

**9 — C.** Service principal (enterprise application).
🔑 El **application object de App1 ya existe en `partner.com`**; no se crea en el tenant consumidor. Lo que le falta a contoso.com es la instancia local: el **service principal**, que se crea al consentir la app multi-tenant.
⚠️ Trampa habitual: la clave es *¿en qué tenant estoy y qué me falta ahí?* Consumo app ajena → service principal. Managed identities son para recursos de Azure, no federan apps.

**10 — C.** Endpoint DLP + DSPM for AI; primero onboarding de dispositivos.
🔑 Endpoint DLP **bloquea** el envío de contenido sensible a webs de IA desde Windows; DSPM for AI da la **visibilidad**. Ambos exigen que los **dispositivos estén onboarded en Purview** — ese es el primer paso.
⚠️ La trampa (B): las apps de IA de **terceros** NO requieren licencia de Copilot; esa licencia es solo para ver *Microsoft 365 Copilot*.

**11 — A.** Solo Oficina A.
🔑 **La regla que se te resistía: no es "¿está el cliente instalado?" sino "¿está ese camino asignado a la política?".**
- Oficina A: remote network **asignada** a la política → entra por GSA → cumple.
- Oficina B: el cliente está desplegado, **pero su camino no está asignado a la política** → su tráfico no queda contemplado → no cumple.
El despliegue del cliente sin la asignación correspondiente no basta. (Si el enunciado hubiera dicho que el camino de B sí está asignado, cumplirían ambas — ese es el matiz que cambia el resultado.)

**12 — D.** Istio.
🔑 **mTLS servicio-a-servicio + mínimo esfuerzo → service mesh → Istio**, que inyecta sidecars Envoy automáticamente.
❌ B: Envoy sabe hacer mTLS pero configurarlo a mano contradice "minimizar esfuerzo". ❌ C: Dapr son bloques de construcción de app, no seguridad de red. ❌ A: Flux es GitOps.

**13 — C.** Azure Policy.
🔑 El objeto del requisito son las **apps ya desplegadas** cumpliendo el MCSB → gobernanza del recurso. Que venga de Azure DevOps es contexto, no el objeto.
⚠️ **Defender recomienda y alerta; Azure Policy aplica y bloquea.** DevOps security da visibilidad de repos/pipelines (otro objeto); branch policies gobiernan el código.

**14 — B.** Azure Firewall Premium.
🔑 **Inspeccionar contenido dentro de HTTPS → Premium obligatorio** (TLS inspection + IDPS). Standard filtra por FQDN vía SNI pero no descifra. Front Door WAF es inbound; aquí es egress.

**15 — B.** 3 segmentos, 2 políticas.
Segmentos: Banca, Análisis, Legal. Políticas solo donde hay restricción: Banca bloquea Análisis, Análisis bloquea Banca. Legal no lleva política (el default es permitir, y eso ya cumple "habla con ambos").
🔑 **Segmentos = grupos con comportamiento distinto; políticas = segmentos que necesitan restricción.**

**16 — B.** Hybrid Connections.
🔑 Sin VPN/ER la VNet no llega a on-prem. El Hybrid Connection Manager en on-prem abre un túnel **saliente** por 443 → cero endpoints entrantes.
❌ A: VNet Integration es outbound hacia la VNet. ❌ C: Private Endpoint es inbound hacia la app, dirección contraria.

**17 — C.** Managed HSM con un único vault y RBAC.
🔑 **El discriminador es "tenancy propia", no el FIPS** (Premium también es L3, pero HSM compartido). Managed HSM = single-tenant dedicado y **solo admite RBAC**. Un único vault para las 20 apps minimiza coste.

**18 — B.** 2.
🔑 **Nº de Resource Guards = nº de conjuntos distintos de operaciones protegidas.** Los RSVaults necesitan un conjunto (3 operaciones), los Backup vaults otro (2). Un Resource Guard protege varios vaults del mismo conjunto → 2.
⚠️ No 4 (uno por vault) ni 1 (uno para todo): se agrupa por conjunto de operaciones.

**19 — B.** Mensajes → Single Item Recovery; Ficheros → Files Restore.
🔑 **Teams no tiene storage propio:** mensajes en Exchange Online (→ Single Item Recovery), ficheros en SharePoint/OneDrive (→ Files Restore). La protección es la de la carga subyacente.

**20 —**
**(a)** Blobs → **Data Map** (fuente cloud-native integrada con Purview); File shares on-prem → **Information Protection scanner** (escanea file servers on-prem).
**(b) B.** Windows ACLs (NTFS) por carpeta, sobre el rol RBAC de share.
🔑 **Azure RBAC no baja de share.** El rol RBAC es la puerta de entrada al share; para permisos por carpeta o archivo, ACLs NTFS. Gana el más restrictivo de las dos capas.
❌ A: no existe RBAC "por carpeta". ❌ D: encryption scopes son de Storage Account (blobs), no de Azure Files.

---

## AUTOEVALUACIÓN — este es el que cuenta

| Aciertos | Lectura |
|---|---|
| 18-20 | Listo para el 5. Los conceptos que fallabas están cerrados. |
| 15-17 | Aprobarías, pero mira qué fallo se repite: si es la 1 (Synapse) o la 11 (GSA), vuelve a su ficha. |
| <15 | No es falta de nivel, es un concepto concreto atascado. Identifícalo y aíslalo. |

**Vigila especialmente la 1, la 2 y la 11**, que son tus tres puntos históricamente más resbaladizos (Synapse serverless, aislamiento de App Service, y asignación en GSA). Si esas tres salen limpias, tienes el examen.

Con estas 20 llegas a **50 preguntas** de práctica en esta recta final, la media del SC-100. A partir de aquí, mantenimiento: relee el diccionario y descansa.
