# SIMULACRO SC-100 — 25 preguntas

**Tiempo objetivo: 50 minutos (2 min/pregunta).**
No mires el solucionario hasta terminar las 25. Apunta tu respuesta y, si dudas entre dos, apunta las dos y marca cuál elegiste — eso es lo que luego analizamos.

Distribución: Cifrado (4) · Ransomware y backup (4) · Identidad Entra (5) · AD on-prem (2) · Red y perímetro (4) · SecOps (3) · Gobernanza y frameworks (2) · DevSecOps (1)

---

## PREGUNTAS

---

**1.** Tu empresa tiene una Storage Account que almacena datos de tres unidades de negocio distintas en containers separados. Auditoría exige que cada unidad de negocio pueda revocar el acceso a **sus** datos de forma independiente, sin afectar a las otras.

¿Qué deberías recomendar?

- A. Una CMK en Key Vault a nivel de Storage Account
- B. Encryption scopes con una CMK distinta por container
- C. Tres Storage Accounts separadas con Microsoft-managed keys
- D. Client-side encryption con claves gestionadas por cada unidad

---

**2.** Una entidad financiera exige que las claves de cifrado residan en hardware **dedicado, no compartido con otros clientes**, con validación FIPS 140-2 Level 3.

¿Qué deberías recomendar?

- A. Azure Key Vault Standard
- B. Azure Key Vault Premium
- C. Azure Managed HSM
- D. Client-side encryption con HYOK

---

**3.** Tienes una tabla de Azure SQL Database con una columna `NumeroCuenta` (alta cardinalidad) y una columna `Pais` (baja cardinalidad). Ambas deben cifrarse con Always Encrypted **sin enclave**. La aplicación necesita hacer `WHERE NumeroCuenta = @valor` pero nunca consulta `Pais` directamente.

¿Qué tipo de cifrado deberías usar para cada columna?

- A. `NumeroCuenta`: Randomized · `Pais`: Deterministic
- B. `NumeroCuenta`: Deterministic · `Pais`: Randomized
- C. Ambas Deterministic
- D. Ambas Randomized

---

**4.** Tienes un Azure Synapse Analytics workspace **ya desplegado** en producción con un Dedicated SQL Pool. Compliance ahora exige cifrado con customer-managed key a nivel de workspace.

¿Qué deberías hacer?

- A. Activar la CMK desde la hoja Encryption del workspace existente
- B. Recrear el workspace con CMK habilitada y migrar los datos
- C. Aplicar la CMK sobre el Storage Account del Data Lake subyacente
- D. Habilitar TDE con CMK en el Dedicated SQL Pool

---

**5.** Tu organización sufre un incidente de ransomware. Los endpoints afectados sincronizan con OneDrive y Exchange ActiveSync.

Siguiendo la metodología DART de Microsoft, ¿cuál es el **primer** paso?

- A. Ejecutar un escaneo completo con Microsoft Defender Antivirus
- B. Restaurar los archivos desde la última copia de seguridad
- C. Deshabilitar la sincronización de OneDrive y Exchange ActiveSync
- D. Rotar las credenciales de todas las cuentas privilegiadas

---

**6.** Diseñas una solución de backup para 200 servidores. El requisito es: *"ni siquiera un Global Administrator comprometido del tenant debe poder eliminar las copias de seguridad. Se requiere máxima protección."*

¿Qué deberías recomendar?

- A. Soft delete con retención extendida en el Recovery Services vault
- B. MUA con Resource Guard en otra suscripción del mismo tenant
- C. MUA con Resource Guard en un tenant distinto
- D. Security PIN para operaciones críticas

---

**7.** Un CISO pregunta qué control protege la **integridad** de los datos de backup frente a ransomware (la "I" de la tríada CIA).

¿Qué deberías recomendar?

- A. Cifrado AES-256 en reposo con CMK rotada cada 90 días
- B. Immutable vault con retención WORM y versionado
- C. Geo-redundant storage con restauración multi-región
- D. BitLocker en todos los servidores de origen

---

**8.** Una empresa quiere proteger las estaciones de trabajo contra ransomware **de día cero** que un antivirus basado en firmas no detectaría. El requisito específico es impedir que procesos no autorizados escriban en las carpetas de documentos de usuario.

¿Qué deberías recomendar?

- A. BitLocker con TPM en todos los endpoints
- B. Controlled Folder Access
- C. Attack Surface Reduction rules
- D. Windows LAPS

---

**9.** Fabrikam colabora con un partner que ya tiene su propio tenant de Entra ID. Los usuarios del partner necesitan acceso a un canal compartido de Microsoft Teams, **manteniendo su identidad nativa y sin crear cuentas de invitado**.

¿Qué deberías recomendar?

- A. B2B Collaboration
- B. B2B Direct Connect
- C. Cross-tenant Synchronization
- D. Entra External ID

---

**10.** Una aplicación legacy migrada a Azure requiere consultas LDAP y autenticación Kerberos. La organización es cloud-only, no tiene AD DS on-premises y quiere **minimizar el esfuerzo administrativo**.

¿Qué deberías recomendar?

- A. Desplegar controladores de dominio AD DS en máquinas virtuales de Azure
- B. Microsoft Entra Domain Services
- C. Microsoft Entra ID con Application Proxy
- D. Microsoft Entra External ID

---

**11.** Necesitas exponer a usuarios remotos, sin VPN, tres recursos on-premises: una aplicación web interna, un servidor SSH y una instancia de SQL Server.

¿Qué deberías recomendar?

- A. Entra Application Proxy para los tres
- B. Entra Private Access para los tres
- C. Application Proxy para la web y Private Access para SSH y SQL
- D. Entra Internet Access para los tres

---

**12.** Para cumplir SOX, ningún usuario debe poder tener simultáneamente el acceso "Finance — Read" y "Finance — Write". La solución debe **prevenir** la asignación, no detectarla a posteriori.

¿Qué deberías recomendar?

- A. Una política de Conditional Access que bloquee la combinación
- B. Access Reviews mensuales sobre ambos grupos
- C. Incompatible access packages en Entitlement Management
- D. Administrative Units separadas por función

---

**13.** Litware usa Pass-through Authentication. Seguridad exige implementar **leaked credential detection** en el tenant, manteniendo PTA como método principal de autenticación.

¿Qué deberías recomendar?

- A. Es incompatible: hay que migrar completamente a Password Hash Synchronization
- B. Habilitar Password Hash Synchronization como respaldo, manteniendo PTA
- C. Habilitar Smart Lockout en el tenant
- D. Desplegar ADFS con claims rules personalizadas

---

**14.** Un pipeline de Azure DevOps necesita autenticarse contra recursos de Azure para desplegar infraestructura. La recomendación debe seguir las best practices del MCSB y el CAF.

¿Qué deberías recomendar?

- A. Un service principal con secret rotado cada 90 días almacenado en Key Vault
- B. Workload Identity Federation con una user-assigned managed identity
- C. Una cuenta de usuario dedicada con PIM y MFA
- D. Un certificado autofirmado renovado anualmente

---

**15.** Necesitas impedir que las cuentas de administrador de Tier 0 puedan autenticarse desde cualquier equipo que no sea una PAW designada.

¿Qué deberías recomendar?

- A. El grupo Protected Users
- B. Authentication Policy Silos
- C. User Rights Assignment vía GPO
- D. Windows LAPS

---

**16.** Se detecta un ataque de Kerberoasting contra cuentas de servicio en el dominio AD DS on-premises.

¿Qué producto detecta este ataque?

- A. Microsoft Entra ID Protection
- B. Microsoft Defender for Identity
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Defender for Cloud

---

**17.** Una App Service debe quedar **completamente inaccesible desde internet** y ser alcanzable solo desde una VNet. Los despliegues se hacen desde Azure Pipelines.

¿Qué combinación deberías recomendar? (Elige **tres**.)

- A. VNet Integration
- B. Private Endpoint en el endpoint principal
- C. Deshabilitar el acceso público
- D. Service Endpoint hacia la App Service
- E. Private DNS Zone
- F. Access Restrictions por Service Tag

---

**18.** Necesitas detectar exfiltración de datos hacia dominios maliciosos sobre tráfico **HTTPS** saliente desde las spokes.

¿Qué deberías recomendar?

- A. Azure Firewall Standard con reglas de aplicación por FQDN
- B. Azure Firewall Premium con TLS inspection e IDPS
- C. NSG con Service Tags de denegación
- D. Azure Front Door con WAF y OWASP CRS

---

**19.** Tienes una topología hub-and-spoke con Spoke A y Spoke B ambos peered al hub. Se requiere que el tráfico entre Spoke A y Spoke B pase por el Azure Firewall del hub.

¿Qué deberías configurar?

- A. Nada: el peering transitivo enruta automáticamente por el hub
- B. Peering directo entre Spoke A y Spoke B
- C. Una UDR en cada spoke con el Firewall del hub como next hop
- D. Service Endpoints en ambas spokes

---

**20.** Tu App Service debe aceptar tráfico **únicamente** procedente de tu instancia de Azure Front Door.

¿Qué deberías usar?

- A. Una Access Restriction basada en el Service Tag `AzureFrontDoor.Backend`
- B. Una Access Restriction que valide el header `X-Azure-FDID`
- C. Un NSG que permita solo las IPs de backend de Front Door
- D. Un Private Endpoint en la App Service

---

**21.** Necesitas ingerir logs CEF de firewalls de terceros on-premises en Microsoft Sentinel.

¿Qué deberías recomendar?

- A. Una Logic App que consulte los firewalls y escriba en el workspace
- B. Un servidor Syslog on-premises con Azure Monitor Agent
- C. El conector nativo de Defender for Cloud
- D. Azure Arc en cada dispositivo firewall

---

**22.** Debes almacenar en Sentinel un volumen muy alto de logs de red de baja criticidad, con **coste mínimo** y retención de un año para investigaciones ocasionales.

¿Qué tipo de tabla deberías recomendar?

- A. Analytics logs
- B. Basic logs
- C. Auxiliary logs
- D. Archive de un Storage Account con acceso vía Data Explorer

---

**23.** Un cliente necesita identificar identidades sobre-privilegiadas simultáneamente en Azure, AWS y GCP, con una métrica cuantitativa del exceso de permisos.

¿Qué deberías recomendar?

- A. Microsoft Entra Privileged Identity Management
- B. Microsoft Entra Permissions Management
- C. Defender CSPM con Attack Path Analysis
- D. Microsoft Purview DSPM

---

**24.** Una organización necesita evaluar la postura de sus recursos de Azure, AWS y GCP frente a NIST 800-53, y además usar Cloud Security Explorer y escaneo agentless de secretos.

¿Qué deberías recomendar?

- A. Foundational CSPM (gratuito)
- B. Defender CSPM
- C. Defender for Servers Plan 2
- D. Microsoft Purview Compliance Manager

---

**25.** Necesitas estructurar toda la organización en Azure: jerarquía de management groups, landing zones, modelo de suscripciones y ciclo de vida de adopción.

¿Qué framework deberías usar?

- A. Well-Architected Framework
- B. Cloud Adoption Framework
- C. Microsoft Cloud Security Benchmark
- D. Microsoft Cybersecurity Reference Architectures

---
---

# SOLUCIONARIO

*(No sigas leyendo hasta haber contestado las 25.)*

---

**1 — B.** Encryption scopes con CMK distinta por container.
Sin encryption scopes, toda la Storage Account comparte una única CMK: revocar afecta a las tres unidades a la vez. El scope permite granularidad de container (o incluso de blob, especificándolo en el upload con `x-ms-encryption-scope`).
❌ A: granularidad de cuenta, no aísla. ❌ C: funciona, pero es rediseño innecesario cuando existe la feature nativa — el examen premia la solución mínima suficiente. ❌ D: client-side es la respuesta cuando la clave **nunca** puede entrar en Azure (HYOK), no aquí.

**2 — C.** Managed HSM.
🔑 La señal es **"hardware dedicado / no compartido"**. Key Vault Premium también cumple FIPS 140-2 Level 3 — el nivel FIPS **no** es el diferenciador, es el aislamiento físico single-tenant.
❌ B: HSM compartido entre tenants (aislamiento lógico). ❌ D: HYOK no aplica, la clave puede vivir en Azure.

**3 — B.** `NumeroCuenta` Deterministic, `Pais` Randomized.
Deterministic es el **único** de los dos que permite igualdad exacta (`WHERE =`, `JOIN`). El riesgo de frequency analysis crece cuanto **más se repite** el valor, así que Deterministic encaja en alta cardinalidad. Randomized es máxima seguridad pero cero operaciones de consulta → perfecto para `Pais`, que no se consulta.
⚠️ La trampa clásica es invertir la lógica de cardinalidad.

**4 — B.** Recrear el workspace.
La CMK de workspace en Synapse **solo se configura en el momento de la creación**. Si el workspace ya existe sin CMK, no hay camino retroactivo.
❌ A: esa hoja no permite activarla a posteriori. ❌ C: eso aplica al **Serverless** SQL Pool (que no tiene TDE porque no tiene storage propio) — confundir Dedicated con Serverless es el fallo documentado del examen. ❌ D: TDE con CMK es de Azure SQL Database, no resuelve el requisito de workspace encryption.

**5 — C.** Deshabilitar la sincronización de OneDrive y Exchange ActiveSync.
Fase 1 de DART = **Contain**, y el primer paso literal es cortar la sincronización para que el cifrado no se propague al cloud.
❌ A/B/D corresponden a fases posteriores (Investigate, Recover, Eradicate). El primer paso nunca es escanear, restaurar ni contactar autoridades.

**6 — C.** Resource Guard en un tenant distinto.
🔑 La palabra gancho es **"máxima protección"** + "Global Admin comprometido". Un Resource Guard en otra suscripción del mismo tenant sigue estando bajo el alcance de ese Global Admin.
❌ A: soft delete da 14 días de margen, pero no impide la operación. ❌ D: el Security PIN es anti-error humano; el atacante con la misma credencial también lo genera.

**7 — B.** Immutable vault con WORM y versionado.
🔑 Trampa CIA: el cifrado protege **confidencialidad**, no integridad. Contra alteración/borrado de backups por ransomware → inmutabilidad.
❌ C es disponibilidad. ❌ D es robo físico del dispositivo, no ransomware.

**8 — B.** Controlled Folder Access.
Modelo **allow-list** (default deny) sobre carpetas designadas → por eso frena 0-day que las firmas no ven. El enunciado dice explícitamente "escribir en carpetas".
❌ C ASR es la respuesta cuando el enunciado habla de **comportamientos de proceso** (Office lanzando child process, scripts ofuscados), no de carpetas. ❌ A BitLocker no protege contra ransomware en absoluto. ❌ D LAPS es rotación de admin local.

**9 — B.** B2B Direct Connect.
🔑 **"shared Teams channels" + "identidad nativa" + "sin guests"** → siempre Direct Connect.
❌ A crea guests. ❌ C es sincronización entre tenants de la **misma** organización (M&A). ❌ D es para consumidores que parten de cero, no partners con tenant propio.

**10 — B.** Entra Domain Services.
🔑 **"LDAP + cloud-only + minimizar esfuerzo administrativo"** → respuesta automática.
❌ A cumple pero maximiza el esfuerzo admin, justo lo contrario del requisito. ❌ C/D no hablan LDAP/Kerberos.

**11 — B.** Private Access para los tres.
Application Proxy **solo** publica HTTP/HTTPS: no puede con SSH ni SQL. Private Access (ZTNA) soporta cualquier TCP/UDP y además cubre la web app, así que una sola solución resuelve todo.
❌ C funcionaría técnicamente, pero mantiene dos soluciones cuando una basta — y Application Proxy es legacy que Microsoft recomienda migrar. ❌ D Internet Access es filtrado de salida (SWG), dirección opuesta.

**12 — C.** Incompatible access packages.
El requisito clave es **prevenir**, no detectar.
❌ A Conditional Access bloquea sign-ins, no asignaciones de acceso. ❌ B Access Reviews detectan **después**. ❌ D las AUs delegan gestión, no controlan combinaciones.

**13 — B.** Habilitar PHS como respaldo manteniendo PTA.
PHS es la **única** forma de tener leaked credential detection, sin excepción — pero se puede activar en modo backup conviviendo con PTA como método primario.
❌ A: la migración total no es necesaria. ❌ C: Smart Lockout mitiga password spray, no detecta credenciales filtradas.

**14 — B.** Workload Identity Federation + user-assigned managed identity.
Respuesta canónica de DevSecOps/CAF: cero secretos que gestionar o rotar.
❌ A sigue teniendo un secreto. ❌ C usar identidades humanas para pipelines viola el modelo. ❌ D mismo problema de gestión de credenciales.

**15 — B.** Authentication Policy Silos.
Restringen **desde dónde** puede autenticarse una cuenta.
❌ A Protected Users es una restricción global del grupo (sin NTLM, sin cache, TGT corto), no controla origen. ❌ C User Rights Assignment controla el **tipo** de logon (interactive vs RDP), no el equipo de origen.

**16 — B.** Defender for Identity.
🔑 Kerberos / NTLM / Domain Controller / DCSync / Golden Ticket → **siempre** Defender for Identity. Entra ID Protection es ciego a ataques on-prem puros porque no ve tráfico Kerberos local.

**17 — B, C, E.** Private Endpoint + deshabilitar acceso público + Private DNS Zone.
El Private Endpoint da la IP privada (inbound), pero **sin deshabilitar el acceso público la app sigue siendo alcanzable por su URL**. La Private DNS Zone es obligatoria siempre que uses Private Endpoint, incluso si todo el consumo es interno.
❌ A VNet Integration es solo **outbound**, no aísla nada. ❌ D Service Endpoint va en dirección VNet→PaaS: es la confusión más peligrosa del bloque. ❌ F filtra, pero no elimina la exposición pública.
⚠️ Extra de examen: acuérdate del endpoint **SCM/Kudu** — si lo olvidas, los despliegues desde Pipelines fallan.

**18 — B.** Azure Firewall Premium.
🔑 **"detectar sobre HTTPS"** → Premium obligatorio. Standard hace FQDN filtering pero **no abre TLS**, así que no ve el contenido.
❌ D Front Door WAF es inbound; el enunciado es egress.

**19 — C.** UDR en cada spoke apuntando al Firewall del hub.
El peering **no es transitivo**: A↔Hub y Hub↔B no dan A↔B.
❌ B daría conectividad directa saltándose el Firewall — justo lo contrario del requisito.

**20 — B.** El header `X-Azure-FDID`.
❌ A y C fallan por la misma razón: las IPs y el Service Tag de Front Door son **compartidos entre tenants y cambian**, así que dejarían entrar tráfico de otros clientes de Front Door.

**21 — B.** Servidor Syslog on-premises con AMA.
❌ A: las Logic Apps son para automatización y playbooks, **nunca** para ingesta.

**22 — C.** Auxiliary logs.
🔑 **"alto volumen + bajo coste + no crítico + retención larga"**. Basic logs es más barato que Analytics pero Auxiliary es el escalón inferior y llega a un año de retención.

**23 — B.** Entra Permissions Management (CIEM).
🔑 **Multinube + identidades sobre-privilegiadas + métrica** → el Permission Creep Index es exactamente esa métrica.
❌ A PIM es JIT de roles Entra/Azure y **no cubre AWS/GCP**. ❌ C Attack Path ve rutas de ataque, no analiza permisos en profundidad.

**24 — B.** Defender CSPM.
Cloud Security Explorer, agentless scanning y regulatory compliance completo (NIST/ISO/PCI) son todos features de pago.
❌ A el gratuito solo da Secure Score, MCSB e inventario. ❌ D Compliance Manager mide procesos y personas (M365), no postura técnica de recursos cloud — *"Defender mide máquinas, Purview mide personas"*.

**25 — B.** Cloud Adoption Framework.
🔑 **"landing zones + management groups + gobernanza organizacional"** → siempre CAF. WAF es "cómo diseño **esta** workload" (5 pilares); MCSB es el baseline técnico de controles que vive dentro del CAF.

---

## CÓMO INTERPRETAR TU RESULTADO

| Aciertos | Lectura |
|---|---|
| 22-25 | Nivel de aprobado holgado. Repasa solo los fallos. |
| 19-21 | Aprobarías, pero con poco margen. Mira si los fallos se concentran en un dominio. |
| 15-18 | Zona de riesgo — es aproximadamente donde estabas con 679. |
| <15 | Vuelve al diccionario antes de más preguntas. |

**Huecos que este simulacro no cubre:** ninguna de estas 25 preguntas cubre Azure Files (granularidad de share), AzureBastionSubnet como control NSG ni el tier de GitHub para Dependabot en repos privados. Si alguno de esos temas resulta flojo, conviene un mini-bloque adicional centrado solo en esos tres puntos y el resto del temario de Bastion y DevSecOps.
