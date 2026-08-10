# Diccionario de repaso SC-100 — Conceptos y trampas

Organizado por bloque temático, en el orden en que se trabajó. Cada entrada: definición corta + dónde está la trampa.

---

## Identidad de apps

**Easy Auth (App Service Authentication)** — Feature nativa de App Service/Functions que gestiona la autenticación sin escribir código; actúa como proxy delante de tu app. Trampa: funciona igual en Web Apps que en Function Apps; no es exclusiva de apps web tradicionales.

**App Registration** — El "plano" global de identidad de una app en Entra. Trampa: cuando registras tu propia app, se crea automáticamente un Service Principal/Enterprise App en tu tenant. Cuando usas una app SaaS de terceros, el App Registration maestro vive en el tenant del fabricante; tú solo ves el Enterprise Application local.

**Enterprise Application** — La instancia local (Service Principal) de una app en tu tenant, donde se configura SSO, asignación de usuarios y Conditional Access. Trampa: para apps de la Gallery (SaaS terceros), se va directo aquí, no a "App Registrations".

**SSO SAML vs OIDC** — SAML usa XML, más orientado a apps enterprise legacy; OIDC usa OAuth2/JWT, más moderno. Trampa: si el enunciado menciona explícitamente "SAML 2.0", la respuesta debe mencionar configurar SSO tipo SAML en la Enterprise Application, no solo "Enterprise Applications" a secas.

**Entra External ID (CIAM)** — Tenant separado para gestionar identidades de consumidores externos (email, social login). Trampa: NO sirve para partners con tenant Entra propio — eso es B2B Collaboration.

**Entra B2B Collaboration** — Federación con partners que ya tienen su propio tenant Entra; entran como guests en el tenant corporativo. Trampa: la pregunta clave es "¿el usuario ya tiene identidad en otro tenant Entra, o parte de cero?" — si ya tiene, B2B, no External ID.

**Entra Application Proxy** — Solución legacy para publicar apps web (solo HTTP/HTTPS) on-prem sin abrir puertos. Trampa: no soporta protocolos no-HTTP (RDP, SSH, SQL).

**Entra Private Access** — ZTNA moderno, soporta cualquier protocolo TCP/UDP, reemplaza VPN y Application Proxy. Trampa: si el enunciado menciona SQL, RDP, SSH, FTP hacia on-prem → obligatoriamente Private Access, Application Proxy no puede.

---

## Aislamiento de red y App Services

**VNet Integration** — Permite a la App Service alcanzar recursos dentro de la VNet. Trampa: solo afecta tráfico OUTBOUND. La app sigue siendo pública para el inbound. No aísla nada por sí sola.

**Private Endpoint (en App Service)** — Da IP privada a la App Service; afecta al INBOUND. Hay que deshabilitar el acceso público para aislamiento real. Trampa: sin deshabilitar el acceso público explícitamente, la app sigue siendo alcanzable por su URL pública además de por la IP privada.

**Access Restrictions** — Reglas que permiten tráfico solo desde IPs/subnets/service tags/header FDID concretos. Trampa: la app sigue teniendo IP pública técnicamente; no es aislamiento real, solo filtrado. Válido cuando el requisito es "restringir origen", no "eliminar exposición pública total".

**ASEv3 + Isolated v2** — Infraestructura single-tenant dedicada dentro de la VNet. Trampa de nomenclatura: el entorno se llama v3, el plan se llama Isolated v2 — no existe "Isolated v3". Es la respuesta cuando piden aislamiento total + IPs dedicadas + migrar muchas apps sin rediseño, y el coste no es prioridad.

**SCM/Kudu endpoint** — Endpoint de deployment de App Service, separado del endpoint de la app. Trampa: si se configura Private Endpoint/Access Restrictions solo en el endpoint principal y se olvida el SCM, los deployments desde Azure DevOps (agentes Microsoft-hosted) fallan porque quedan bloqueados o expuestos según el caso.

**Private DNS Zone** — Necesaria SIEMPRE que se use Private Endpoint, para que el FQDN público resuelva a la IP privada (vía CNAME chain). Trampa: es necesaria incluso si todo el consumo es dentro de Azure, no solo cuando hay requisito on-prem.

**Azure DNS Private Resolver** — Solo necesario cuando además hace falta que on-premises (u otra nube) resuelva ese FQDN privado. Trampa: no confundir "siempre necesario" (Private DNS Zone) con "solo si hay on-prem" (DNS Resolver).

**Service Endpoint** — Optimiza ruta de una VNet hacia un servicio PaaS (dirección VNet→PaaS). Trampa grave: NO sirve para restringir acceso ENTRANTE a una App Service — esa es la confusión más peligrosa del bloque, dirección de tráfico opuesta a lo que se necesita.

---

## Cifrado en Storage

**Encryption Scope** — Permite asignar una CMK distinta a nivel container o incluso blob individual dentro de la misma Storage Account. Trampa: sin encryption scopes, toda la cuenta comparte una sola CMK. El scope a nivel blob se especifica en el momento del upload (header `x-ms-encryption-scope`), no es una propiedad fija de antemano.

**CMK (Customer-Managed Key)** — Clave en Key Vault que el cliente controla (rotación, revocación). Trampa: revocar la clave deja los datos irrecuperables de inmediato — esto es propiedad de CMK en general, a cualquier granularidad, no exclusivo de encryption scopes.

**BYOK (Bring Your Own Key)** — La clave se genera fuera y se importa a Key Vault/Managed HSM; termina existiendo en Azure. Trampa: es lo que realmente soporta Azure en el 99% de casos, aunque el enunciado hable de "generar fuera del proveedor".

**HYOK (Hold Your Own Key)** — La clave nunca sale del HSM físico propio. Trampa: para PaaS general (Storage, SQL, Cosmos DB) esto NO está soportado con cifrado nativo del servicio. La alternativa real es client-side encryption (se cifra antes de subir, Azure nunca ve la clave).

**Key Vault Premium (HSM-backed)** — FIPS 140-2 Level 3, pero en HSM compartido con otros tenants (aislamiento lógico, no físico). Trampa: cumple FIPS 140-2 L3 igual que Managed HSM — el nivel FIPS NO es el diferenciador.

**Managed HSM** — FIPS 140-2 Level 3 en hardware dedicado single-tenant, con RBAC de plano de datos propio. Trampa: la señal que activa esta respuesta es "hardware dedicado / no compartido / exclusivo", no solo "FIPS 140-2 L3" (eso ya lo da Premium).

---

## Cifrado en SQL / Synapse

**TDE (Transparent Data Encryption) con CMK** — Cifrado en reposo de Azure SQL Database; se puede activar/rotar en cualquier momento sin recrear el recurso. Trampa: no protege contra usuarios con permisos elevados (DBA/sysadmin) — solo cifra en reposo/tránsito.

**Synapse Dedicated SQL Pool — workspace encryption con CMK** — Solo configurable al crear el workspace, no se puede añadir retroactivamente. Trampa: la limitación NO es "recifrado en caliente con doble clave" — es que si el workspace ya existe sin CMK, hay que recrearlo enteramente.

**Synapse Serverless SQL Pool** — No tiene TDE (no tiene storage propio); el cifrado con CMK se aplica a nivel del Storage Account del Data Lake subyacente. Trampa: confundir Dedicated con Serverless es un fallo de examen documentado.

**Always Encrypted (clásico, sin enclave)** — Cifra datos de forma que ni siquiera sysadmin ve el valor en claro; la clave nunca reside en el servidor SQL. Trampa: NO es lo mismo que TDE ni que Dynamic Data Masking (que solo oculta visualmente, el dato real sigue accesible con permisos).

**Deterministic** — Mismo valor en claro = mismo cifrado siempre; permite igualdad exacta (`WHERE =`, `JOIN`). Trampa de razonamiento: el riesgo de frequency analysis es MAYOR cuanto MÁS SE REPITE el valor (baja cardinalidad), no al revés. Se usa para columnas de alta cardinalidad que se necesiten consultar (números de cuenta, DNI).

**Randomized** — Mismo valor en claro = cifrado distinto cada vez; máxima seguridad pero CERO operaciones de consulta (ni igualdad). Trampa: se usa para columnas de baja cardinalidad (país, género, booleano) que NO se necesiten consultar directamente.

**Secure Enclaves (VBS / Intel SGX)** — Única forma de hacer operaciones de rango (`>`, `<`), patrón (`LIKE`) o in-place re-encryption sobre datos cifrados. Trampa: ni deterministic ni randomized (en modo clásico, sin enclave) soportan rango — el enclave descifra temporalmente en memoria aislada, hace el cálculo en claro ahí dentro, y nunca expone el resultado en claro fuera. VBS = SQL Server on VM/on-prem; Intel SGX = Azure SQL DB series DC.

---

## Ransomware y endpoints

**Windows LAPS** — Rota automáticamente la contraseña del admin local de cada equipo; storage en Entra ID para dispositivos hybrid/Entra-joined. Trampa: no es "elevación de privilegios", es acceso controlado y rotado a una cuenta admin local que ya existe, rompiendo el patrón de lateral movement por contraseña compartida.

**Controlled Folder Access (CFA) / Protected Folders** — Allow-list de apps que pueden escribir en carpetas específicas protegidas. Trampa: solo actúa sobre las carpetas designadas, no sobre todo el disco. Modelo allow-list (default deny), por eso protege contra 0-day que un antivirus de firmas no detecta.

**ASR Rules (Attack Surface Reduction)** — Bloquea patrones de comportamiento de proceso (Office lanzando child process, scripts obfuscados) ANTES de tocar archivos, independiente de carpeta. Trampa: se confunde con CFA — CFA protege archivos en carpetas concretas, ASR bloquea comportamientos de proceso en general.

**BitLocker** — Cifra el disco completo. Trampa gravísima: protege contra ROBO FÍSICO del dispositivo, NO contra ransomware — el ransomware cifra por encima de BitLocker sin problema, porque el SO ve los archivos descifrados en uso normal.

**MUA (Multi-User Authorization) con Resource Guard** — Two-person rule para operaciones críticas sobre Recovery Services vault. Trampa confirmada por Learn: para máxima protección, el Resource Guard debe vivir en OTRO TENANT (no solo otra suscripción) — esto garantiza que ni un Global Admin comprometido del tenant original puede tocarlo.

**Secuencia DART (respuesta a ransomware)** — 1) Contain: disable OneDrive sync + Exchange ActiveSync (primer paso literal) → 2) Investigate: alcance del ataque → 3) Eradicate: rotar credenciales, eliminar malware, preservando evidencia forense → 4) Recover: a dispositivos limpios, nunca a infectados → 5) Lessons learned. Trampa: el primer paso NUNCA es "escanear con Defender", "contactar autoridades" o "recuperar archivos" — es aislar deshabilitando la sincronización.

---

## Las tres detecciones de identidad (recordatorio transversal)

**Entra ID Protection** — Riesgo en sign-in cloud (viaje imposible, IP TOR, leaked credentials — requiere PHS). Trampa: ciego a ataques on-premises puros como Kerberoasting o DCSync, porque no tiene visibilidad de tráfico de red local ni protocolos Kerberos/NTLM.

**Defender for Identity** — Ataques AD on-prem (Pass-the-Hash, Golden Ticket, Kerberoasting, DCSync, DCShadow). Trampa: si el enunciado menciona Domain Controller, Kerberos, NTLM → siempre esta, nunca Entra ID Protection.

**Defender for Cloud Apps** — Comportamiento anómalo en sesiones SaaS (mass download, mass delete). Trampa: post-login, dentro de la app; distinto de Entra ID Protection que es en el momento del login.

---

## Regla general de oro para todo el repaso

Cuando dudes entre dos servicios que "suenan parecido", pregúntate:

1. ¿Es dirección de tráfico entrante o saliente? (VNet Integration=out, Private Endpoint=in, Service Endpoint=out desde VNet hacia PaaS)
2. ¿La clave sale de Azure o se queda en Azure? (BYOK=entra a Azure, HYOK=nunca sale, no soportado en PaaS nativo)
3. ¿Protege datos en reposo, en tránsito, o contra quién los administra? (TDE=reposo/tránsito, Always Encrypted=incluso contra admins)
4. ¿Es ataque cloud o on-premises? (Entra ID Protection=cloud, Defender for Identity=on-prem)
5. ¿Es la etiqueta más exagerada o la mínima suficiente? (el examen premia la solución mínima que cumple el requisito exacto, no la más robusta a cualquier coste — salvo que el enunciado pida explícitamente "máxima protección", como en Resource Guard)

---

## Frameworks y Zero Trust

**CAF (Cloud Adoption Framework)** — Gobernanza a nivel organización completa: landing zones, ciclo de vida de adopción, estructura de suscripciones. Trampa: si el enunciado menciona "governance + landing zones + compliance organizacional a escala empresarial" → siempre CAF, nunca WAF.

**WAF (Well-Architected Framework)** — 5 pilares (Security, Reliability, Performance, Cost, Operational Excellence) aplicados a una workload concreta. Trampa: WAF es "cómo diseño esta app", CAF es "cómo estructuro todo Azure".

**MCSB (Microsoft Cloud Security Benchmark)** — Baseline técnico de controles de seguridad, mapeado a NIST/CIS/PCI. Trampa: no es un framework de gobernanza organizacional, es un benchmark de controles técnicos que vive DENTRO del CAF (fase Secure).

**MCRA (Microsoft Cybersecurity Reference Architectures)** — Catálogo de referencia de capacidades y servicios de seguridad de Microsoft y cómo se interconectan.

**Zero Trust (3 principios)** — Verify explicitly, least privilege, assume breach. Trampa: "verify explicitly" se mapea a Conditional Access; "assume breach" a microsegmentación/monitoreo; "least privilege" a JIT/PIM.

**NIST 800-207 — PE, PA, PEP** — Policy Engine (decide, aplica trust algorithm) + Policy Administrator (ejecuta la decisión, genera tokens) + Policy Enforcement Point (aplica físicamente en el data plane). Trampa mnemotécnica: "PDP piensa, PEP pega". En Microsoft: PE+PA = Conditional Access; PEP = Entra ID en el sign-in, App Proxy/Private Access para apps.

---

## Azure Landing Zones

**Azure Landing Zone Accelerator** — Despliegue automatizado de landing zones con NSGs, Firewall, Key Vault, Bastion preconfigurados. Trampa: si piden "minimizar esfuerzo de despliegue" + "seguir best practices CAF" → esta es la respuesta, no Well-Architected Framework ni Azure Advisor.

**Platform Landing Zone vs Application Landing Zone** — Recursos compartidos (Firewall, Gateway, DNS, Bastion) viven en Platform LZ; recursos específicos de app (Key Vault, Storage, compute) viven en Application LZ. Trampa: pregúntate "¿este recurso se comparte entre apps o es específico de una?" — Key Vault es específico de app, aunque "suene a seguridad compartida".

**Management Groups** — Jerarquía Tenant Root → Intermediate → Platform/Landing Zones/Sandbox/Decommissioned.

---

## Hub-and-spoke y redes

**Azure Firewall Standard vs Premium** — Standard = L3/L4 + FQDN filtering, sin TLS inspection ni IDPS. Premium = añade TLS inspection, IDPS, URL filtering. Trampa: "detectar exfiltración sobre HTTPS" → siempre Premium, Standard no abre TLS.

**Forced Tunneling (UDR)** — Ruta `0.0.0.0/0` hacia el Firewall del hub para inspección centralizada de egress.

**VNet Peering — no transitivo** — Peering A↔Hub y Hub↔B NO da conectividad A↔B automáticamente. Trampa: para tráfico inter-spoke pasando por el Firewall, hace falta UDR explícita en cada spoke apuntando al Firewall del hub como next hop.

**Multi-región = un hub por región** — Un hub único centralizado es single point of failure regional. Trampa: si el enunciado menciona múltiples regiones + inspección, la respuesta es un hub por región, no un hub central.

**Azure Bastion — SKUs** — Basic (RDP/SSH básico) → Standard (native client, IP-based, scaling) → Premium (session recording, private-only sin IP pública propia). Trampa: "grabación de sesiones" o "Bastion sin IP pública propia" → Premium obligatorio.

**Bastion + peering** — Un solo Bastion puede servir a VNets peered, no hace falta uno por VNet. Trampa de conteo: en topologías con peering, se cuentan hubs/regiones, no VNets individuales.

**JIT VM Access** — Abre puertos RDP/SSH temporalmente bajo demanda, vía Defender for Cloud. Distinto de PIM (que activa roles, no puertos).

**Azure Virtual WAN / Secured Virtual Hub** — Para conectividad mesh global multi-región, branches en muchos países. Trampa: si menciona "global" o "mesh entre regiones" → Virtual WAN, no Hub-Spoke tradicional.

**NSG + ASG (Application Security Groups)** — Microsegmentación por rol/función sin gestionar IPs individuales que cambian. Trampa: ASG (agrupación lógica de VMs) ≠ Service Tag (etiqueta predefinida de Microsoft para servicios Azure) — si dice "agrupar por rol" → ASG; si dice "tráfico hacia Storage/SQL/KeyVault" → Service Tag.

**AKS Authorized IP Ranges** — Restringe acceso al API server público de AKS por IP. Trampa: NO es Service Endpoint ni Private Endpoint — es una feature nativa específica de AKS.

**Front Door access restriction** — Para que solo Front Door llegue a la App Service, se usa el header `X-Azure-FDID`. Trampa: Service Tags y backend IP de Front Door NO sirven (compartidos entre tenants, cambian).

---

## Defender for Cloud multinube (CSPM/CWPP)

**Foundational CSPM (Free)** — Secure Score, MCSB recommendations, asset inventory básico.

**Defender CSPM (Paid)** — Attack Path Analysis, Cloud Security Explorer, agentless scanning (vulnerability + secret scanning sin agentes), AI security posture, regulatory compliance completo (NIST/ISO/PCI). Trampa: si el enunciado pide cualquiera de estos términos → Defender CSPM, no Foundational.

**CSPM vs CWPP** — CSPM = postura de configuración (¿está bien configurado?). CWPP = protección runtime (¿está siendo atacado?).

**Defender for Servers Plan 1 vs Plan 2** — Plan 1 = MDE + vulnerability assessment básico. Plan 2 = añade JIT VM Access, File Integrity Monitoring, Adaptive Application Controls, Adaptive Network Hardening. Trampa: JIT/FIM/Adaptive controls → siempre Plan 2.

**Azure Arc** — Extiende el control plane de Azure a recursos no-Azure (on-prem, EC2, GCE) para aplicar Azure Policy, MDE, compliance. Trampa: agentless scanning de EC2 NO requiere Arc (usa snapshots directos); pero Azure Policy y detección runtime con MDE SÍ requieren Arc.

**Entra Permissions Management (CIEM)** — Detecta identidades sobreprivilegiadas multinube (Azure+AWS+GCP), métrica PCI (Permission Creep Index), right-sized policies. Trampa: NO es lo mismo que PIM (que gestiona roles Entra/Azure con JIT) ni que Defender CSPM Attack Path (que ve rutas de ataque, no analiza permisos en profundidad).

**Defender for AI services** — Runtime detection de prompt injection/jailbreak en Azure OpenAI, alertas en Defender XDR. Distinto de Defender CSPM AI security posture (evalúa configuración) y de Purview DSPM for AI (visibilidad de uso humano de IA).

---

## SecOps: Sentinel vs Defender XDR vs Defender for Cloud

**Microsoft Sentinel** — SIEM/SOAR. Ingiere de CUALQUIER fuente (terceros, on-prem, CEF/Syslog). Trampa clave: "across all services", "centrally manage", "hunting", "audit logs de todo" → siempre Sentinel.

**Defender XDR** — Correlación de los Defenders de Microsoft (Endpoint, Office, Identity, Cloud Apps) + AIR (self-healing) + Attack Disruption. Trampa: solo cubre productos Microsoft, no "todos los servicios" en general.

**Defender for Cloud** — CNAPP, postura + protección de workloads Azure/AWS/GCP.

**CEF logs a Sentinel** — Vía Syslog server on-prem con Azure Monitor Agent (AMA). Trampa: NUNCA Logic Apps para ingesta (eso es para automatización/playbooks, no para ingest).

**Sentinel — tipos de logs por coste** — Analytics logs (caro, completo) → Basic logs (barato, búsqueda limitada) → Auxiliary logs (más barato aún, alto volumen, retención hasta 1 año). Trampa: "alto volumen + bajo coste + logs no críticos" → Auxiliary logs.

**AIR (Automated Investigation and Response) / self-healing** — Nativo de Defender XDR, investiga y remedia automáticamente (cuarentena, detener procesos). Trampa: si dice "nativo de XDR" → AIR, no Sentinel Automation Rules (que es orquestación cross-tool vía Logic Apps).

---

## Identidad híbrida

**Entra Connect Sync vs Entra Cloud Sync** — Connect Sync = motor local, servidor dedicado, soporta ADFS/PTA/device writeback. Cloud Sync = motor en Microsoft, agente ligero, soporta disconnected forests, NO soporta ADFS ni PTA. Trampa: "disconnected forests" o "M&A sin trust" → Cloud Sync casi siempre.

**PHS (Password Hash Sync)** — Hashes en Entra; habilita leaked credentials detection; resiliente si AD on-prem cae. Trampa: es la ÚNICA forma de tener leaked credentials detection, sin excepción, aunque se use PTA o Federation como método primario (se puede combinar "PHS as backup").

**PTA (Pass-through Authentication)** — Contraseñas nunca salen de on-prem, agentes validan en tiempo real. Trampa: dependencia total de on-prem; NO soporta leaked credentials.

**Federation (ADFS)** — Mucha infraestructura on-prem, Microsoft la desaconseja activamente para casos nuevos. Trampa: si dice "minimize on-premises infrastructure" → Federation se descarta automáticamente.

**Password Writeback** — Cambios de contraseña en Entra (SSPR) se propagan de vuelta a AD on-prem. Requiere P1 mínimo.

**Seamless SSO** — Usuarios en red corporativa entran sin teclear contraseña. Compatible con PHS/PTA, NO con Federation (ADFS ya tiene su propio SSO).

**Smart Lockout** — Filtra intentos de password spray a nivel Entra ANTES de reenviarlos a AD on-prem, evitando que ataques cloud bloqueen cuentas locales. Trampa: no confundir con "subir el threshold en AD" (eso debilita la seguridad).

**Cross-tenant Access Settings** — Control inbound/outbound entre tenants Entra, con trust de MFA configurable por tenant partner.

**Tenant Restrictions v2** — Previene que dispositivos corporativos accedan a tenants Entra no autorizados (incluso con identidad personal). Parte de Entra Internet Access. Trampa: para "prevenir autenticación a otros tenants M365" la respuesta oficial es Entra Internet Access con Tenant Restrictions v2, no Private Access.

---

## Identity Governance y Entitlement Management

**Connected Organization** — Representa un tenant externo para que sus usuarios soliciten access packages sin crear guests manualmente.

**Catalog** — Contenedor delegable de recursos para que NO-admins puedan crear access packages. Trampa: distinto de Administrative Units (que delegan gestión de usuarios/grupos, no creación de access packages).

**Access Package** — Bundle de recursos + políticas de aprobación, autoservicio vía portal myaccess.

**Access Reviews** — Revisiones recurrentes (self-review, manager, group owner), auto-apply results con "remove if no response".

**Lifecycle Workflows** — Automatización Joiner/Mover/Leaver disparada por atributos HR (`employeeHireDate`). Requiere Entra ID Governance.

**Incompatible Access Packages (SoD)** — Impide combinaciones peligrosas de acceso (ej. Finance Read + Finance Write) para cumplir Separation of Duties SOX. Trampa: NO se hace con Conditional Access (que bloquea sign-ins, no asignaciones) ni con Access Reviews (que detectan después, no previenen).

**B2B Collaboration vs B2B Direct Connect vs Cross-tenant Sync** — Collaboration = guests generales. Direct Connect = SOLO para Teams Shared Channels, sin crear guests, identidad nativa. Cross-tenant Sync = sincronización automática entre tenants de la misma organización (M&A). Trampa: "Teams shared channels" → siempre Direct Connect.

---

## Conditional Access y PIM

**Authentication Strengths** — Exige métodos concretos (FIDO2, Windows Hello, certificados) — "phishing-resistant MFA". Trampa: distinto de Device Compliance (estado del dispositivo) y Session Control (qué pasa después del login).

**PIM (Privileged Identity Management)** — JIT para ROLES Entra/Azure, con aprobación y MFA. Trampa: NO cubre roles AWS/GCP (para eso, Permissions Management/CIEM). Distinto de JIT VM Access (que abre puertos, no roles).

**Enterprise Application necesaria antes de CA** — Para aplicar Conditional Access a una SaaS app, primero hay que registrarla/añadirla como Enterprise Application; si no existe en el tenant, CA no puede targetearla.

**Workload Identity Federation** — Managed identity + federated credential para CI/CD pipelines sin secrets. Trampa: la respuesta canónica de DevSecOps/CAF para autenticar pipelines, nunca service principal con password ni cuenta de usuario con PIM.

---

## Active Directory on-premises

**Tiered Administration Model** — Tier 0 (DCs, ADFS, control plane) / Tier 1 (servers) / Tier 2 (workstations). Regla: nunca usar cuenta de un tier en otro.

**Protected Users group** — Sin NTLM, sin cache de credenciales, TGT de vida corta. Trampa: protege contra Pass-the-Hash pero rompe apps legacy que dependan de NTLM.

**Authentication Policy Silos** — Restringe DESDE DÓNDE puede autenticarse una cuenta (ej. Tier 0 solo desde PAW). Distinto de Protected Users (que son restricciones globales del grupo).

**User Rights Assignment (GPO)** — Controla el TIPO de logon permitido (interactive, RDP, servicio). "Allow log on locally" vs "Deny log on through Remote Desktop Services".

**DSRM (Directory Services Restore Mode)** — Modo de recuperación de DCs; su contraseña puede sincronizarse con una cuenta protegida (DSRM password sync).

**Ataques AD y quién los detecta (todos → Defender for Identity)**: Pass-the-Hash, Pass-the-Ticket, Golden Ticket, Silver Ticket, Kerberoasting, AS-REP Roasting, DCSync, DCShadow, Skeleton Key.

**PAW (Privileged Access Workstation)** — Dispositivo dedicado para tareas Tier 0. Trampa de least privilege: el admin gestor de PAWs es local admin en TODOS los PAWs; el usuario Security Admin es local admin SOLO en su propio PAW asignado, nunca en los de otros.

---

## Microsoft Purview

**Sensitivity Labels** — Clasificación + protección (cifrado, watermark, restricción de acciones). Auto-labeling requiere E5.

**DLP (Data Loss Prevention)** — Previene exfiltración de contenido sensible detectado. Endpoint DLP requiere E5.

**Insider Risk Management** — Detecta comportamiento anómalo de empleados (data theft, departing users). Requiere E5.

**Adaptive Protection** — Integra IRM + DLP + Conditional Access: si IRM detecta riesgo alto, escala automáticamente las restricciones.

**DSPM (Data Security Posture Management)** — Vista unificada de postura de datos M365 + multinube.

**DSPM for AI** — Third-party AI apps (ChatGPT, Gemini) requiere ENDPOINT ONBOARDING, no licencia Copilot. Microsoft 365 Copilot requiere licencia Copilot + Purview. Trampa de la actualización 2026: no asumir que toda visibilidad de IA requiere licencia Copilot.

**Compliance Manager vs Defender for Cloud Regulatory Compliance** — Compliance Manager = controles organizacionales, evidencias, tareas asignadas a personas (M365/procesos). Defender for Cloud = postura técnica de recursos cloud. Mnemotécnica: "Defender mide máquinas, Purview mide personas".

**Microsoft Priva** — Producto SEPARADO de Purview, para privacy risk y subject rights requests (GDPR DSARs).

**eDiscovery Standard vs Premium** — Premium añade custodians, predictive coding, deduplicación ML (requiere E5).

**Audit Standard vs Premium** — Premium = retención hasta 10 años + eventos forenses adicionales (`MailItemsAccessed`).

---

## DevSecOps y CAF

**STRIDE** — Modelo de threat modeling top-down de Microsoft, usado para iniciar el proceso (no DREAD, que es scoring posterior).

**Branch Policies (Azure Repos)** — Fuerzan pull request antes de merge/deploy.

**SAST vs Secret Scanning vs SCA/Dependabot** — SAST = bugs en el código propio. Secret Scanning = claves hardcodeadas olvidadas. SCA/Dependabot = vulnerabilidades en librerías de terceros. Mnemotécnica: "¿de quién es el código defectuoso? Mío=SAST, secreto olvidado=Secret Scanning, ajeno=SCA".

**Flux (GitOps)** — Sincroniza manifest files de Git hacia clusters Kubernetes de forma consistente. Trampa: NO es Dependabot (que actualiza librerías de código, no configuración K8s).

**Gatekeeper** — Policy enforcement en Kubernetes (OPA-based admission control), distinto de Flux (que sincroniza configuración).

**GitHub Advanced Security** — Paquete que integra SAST + Secret Scanning + dependency scanning en GitHub/Azure DevOps.

---

## Licencias — tablas rápidas

**Entra**: Free (security defaults) → P1 (Conditional Access, Auth Strengths, SSPR writeback, App Proxy) → P2 (todo P1 + ID Protection, PIM, Access Reviews, Entitlement Management). SKUs separados: Entra ID Governance (P2 + Lifecycle Workflows + SoD), Permissions Management (CIEM).

**Purview/M365**: E3 (labels manuales, DLP básico, eDiscovery/Audit Standard) → E5 (todo E3 + auto-labeling, Endpoint DLP, Insider Risk, eDiscovery/Audit Premium, DSPM for AI, Adaptive Protection).

**Defender for Cloud**: Foundational CSPM (free) → Defender CSPM (paid: Attack Path, Cloud Security Explorer, agentless, AI posture) + Defender Plans independientes por workload (Servers, Containers, Storage, etc.).

---

## Identidad para cargas legacy

**Entra Domain Services** — LDAP/Kerberos/NTLM gestionado en cloud, sin gestionar DCs propios. Trampa: para apps legacy migradas a cloud-only que necesitan LDAP, minimizando esfuerzo admin.

**Managed Identity (para servicios Azure como APIM)** — Siempre preferible sobre service principals con secret/password según MCSB best practice. Elimina gestión de credenciales por completo.

---

## Backup y ransomware (complemento)

**Soft Delete** — 14 días default en Storage/backups, protección contra borrado accidental/malicioso.

**Immutable Vault / Immutable Storage** — Backups no modificables ni borrables durante retención (WORM). Protege INTEGRIDAD (la "I" de CIA) contra ransomware — el cifrado protege confidencialidad, no integridad. Trampa CIA: si preguntan por integridad de datos ante ransomware → Immutable Storage + versionado, NUNCA cifrado AES-256 (eso es confidencialidad).

**Security PIN for critical operations** — Anti-error humano, NO anti-ataque (el atacante con la misma credencial también puede generar el PIN).

---

## Chuleta final — palabras gancho por producto

Cómo usar esta chuleta: léela una vez entera antes del examen. Después, solo hace falta mirarla si te bloqueas en una pregunta. La idea es que las palabras gancho del enunciado activen el producto correcto en segundos.

### Los 5 reyes de las trampas (memorizar sí o sí)

**1. Sentinel vs Defender XDR vs Defender for Cloud**

| Si ves en el enunciado... | Es... |
|---|---|
| "across **all** services", "centrally manage", "SIEM", "SOAR", "hunting", "third-party logs", "CEF", "Syslog", "custom data sources", "audit logs", "ingest from anywhere" | Microsoft Sentinel |
| "cross-domain investigation", "endpoint + identity + email + cloud apps", "incident across Microsoft Defenders", "Attack Disruption", "AIR / self-healing" | Microsoft Defender XDR |
| "posture", "Secure Score", "regulatory compliance", "CSPM", "CWPP", "Attack Path", "Cloud Security Explorer", "Azure/AWS/GCP resources", "agentless scanning" | Microsoft Defender for Cloud |

**2. Las 3 detecciones de identidad**

| Si ves... | Es... |
|---|---|
| "Domain Controller", "Kerberos", "NTLM", "Pass-the-Hash", "DCSync", "Golden Ticket", "Kerberoasting", "AD on-prem", "lateral movement on-prem" | Defender for Identity |
| "sign-in", "atypical travel" (en login), "anonymous IP", "TOR", "leaked credentials", "password spray", "user risk", "sign-in risk", "Entra cloud login" | Entra ID Protection |
| "SaaS app", "SharePoint", "OneDrive", "mass download", "mass delete", "session activity", "shadow IT", "anomaly detection in cloud apps" | Defender for Cloud Apps |

**3. Compliance: técnico vs organizacional**

| Si ves... | Es... |
|---|---|
| "compliance vs ISO/NIST/PCI for cloud resources", "posture against standard", "Azure/AWS/GCP resources cumplen X" | Defender for Cloud Regulatory Compliance dashboard |
| "assign tasks to teams", "evidence documentation", "improvement actions", "regulatory assessments organizacionales", "M365 controls", "templates premium" | Microsoft Purview Compliance Manager |
| "translate regulatory into technical controls", "GDPR controls mapping", "framework templates" | Purview Compliance Manager |

Mnemotécnica: "Defender mide máquinas, Purview mide personas".

**4. CSPM Free vs Defender CSPM (paid)**

| Si ves... | Es... |
|---|---|
| "Secure Score básico", "MCSB", "asset inventory" | Foundational CSPM (Free) |
| "Attack Path Analysis", "Cloud Security Explorer", "agentless vulnerability/secret scanning", "AI security posture", "data-aware security posture", "regulatory compliance completo (NIST, ISO, PCI)" | Defender CSPM (Paid) |

**5. PIM vs JIT VM Access (los dos JIT)**

| Si ves... | Es... |
|---|---|
| "temporary role activation", "Global Admin solo X horas", "approval to elevate", "eligible role" | PIM |
| "RDP/SSH temporal access", "abrir puerto N horas", "JIT en VM", "request access to virtual machine" | JIT VM Access (en Defender for Cloud) |

### Identidad Entra — palabras gancho

**Colaboración externa (trampa recurrente)**

| Si ves... | Es... |
|---|---|
| "shared Teams channels", "users mantienen identidad nativa", "no guest accounts" | B2B Direct Connect |
| "external partners as guests", "invitar usuarios externos", "general SaaS access" | B2B Collaboration |
| "same company multiple tenants", "M&A subsidiarias", "auto-sync usuarios entre tenants" | Cross-Tenant Synchronization |
| "users without explicitly creating guests", "external users self-request access" | Connected Organization + Access Packages |

**Acceso a apps (otra trampa típica)**

| Si ves... | Es... |
|---|---|
| "replace VPN", "access private apps", "TCP/UDP non-HTTP", "ZTNA", "on-prem FTP/SSH/RDP desde internet", "Kerberos web app moderna" | Entra Private Access |
| "web filtering outbound", "block social networks", "URL categories", "SWG", "Microsoft 365 profile traffic" | Entra Internet Access |
| "publish internal HTTP/HTTPS web app externally without VPN", "Kerberos web apps legacy" | Entra Application Proxy (o migrar a Private Access) |
| "prevent users from authenticating to other M365 tenants", "tenant restrictions" | Entra Internet Access (con Tenant Restrictions v2) |

**Identidad descentralizada y externa**

| Si ves... | Es... |
|---|---|
| "decentralized identity", "user controls own credential", "verifiable credential", "wallet", "W3C DID" | Entra Verified ID |
| "customer identity store", "Facebook/Google/Microsoft accounts login", "customizable branding for app", "B2C", "MAU pricing" | Entra External ID (B2C) |
| "LDAP queries + cloud-only + minimize admin effort" | Entra Domain Services (NO Entra ID base, NO External ID) |

**CA, PIM, Governance**

| Si ves... | Es... |
|---|---|
| "phishing-resistant MFA", "FIDO2", "passkey específicamente", "certificate-based auth" | Authentication Strengths (en CA) |
| "device compliance", "Intune-managed", "hybrid joined" | Conditional Access con device condition (P1) |
| "sign-in risk o user risk como condición CA" | Entra ID Protection (requiere P2) |
| "JIT activation of role" / "Global Admin temporal con approval" | PIM (P2) |
| "access packages", "self-service portal myaccess", "request bundle of apps" | Entitlement Management (P2) |
| "joiner/mover/leaver", "employeeHireDate trigger", "automated onboarding" | Lifecycle Workflows (Entra ID Governance) |
| "SoD", "incompatible access packages", "SOX prevent both roles" | Entitlement Management Incompatible Access Packages |
| "delegate access package creation to non-admins" | Catalogs (NO Administrative Units) |
| "delegate user/group management by business unit" | Administrative Units (NO Catalogs) |
| "identify over-privileged identities multinube (AWS+Azure+GCP)", "PCI (Permission Creep Index)", "right-sized policies" | Entra Permissions Management (CIEM) |

CA + workload identities: Conditional Access para workload identities se aplica a **service principals** (no a managed identities, que tienen su propio mecanismo). DevOps pipelines + service connections con least privilege → Workload Identity Federation + user-assigned managed identities.

### AD on-prem — palabras gancho

| Si ves... | Es... |
|---|---|
| "prevent Pass-the-Hash for high-privileged accounts", "no NTLM", "no cached credentials" | Protected Users group |
| "restrict where Tier 0 accounts can log on", "solo desde PAW" | Authentication Policy Silos |
| "restrict type of sign-in" (interactive vs RDP), "built-in admin solo consola", "deny RDP for admin" | User Rights Assignment (GPO) |
| "rotate local admin password automatically", "different password per device", "store passwords in Entra" (hybrid joined) | Windows LAPS (moderno, no legacy) |
| "DSRM credentials backed up" | DSRM password sync + Windows LAPS |
| Cualquier ataque AD on-prem (Kerberos, NTLM, DCSync, etc.) | Defender for Identity lo detecta |
| "LDAP cloud-only minimize admin" | Entra Domain Services |
| "ADFS retiring", "minimize on-prem infrastructure" | Migrar a PHS o PTA |

### Purview — palabras gancho

| Si ves... | Es... |
|---|---|
| "watermark", "header/footer", "marca visual en doc/email", "classification + protection" | Sensitivity labels (Information Protection) |
| "auto-labeling based on content" | Sensitivity labels con auto-labeling (requiere E5) |
| "block exfiltration of sensitive content", "credit card / SSN / PII detection", "block upload to AI tools" | Data Loss Prevention (DLP) |
| "Endpoint DLP", "block copy to USB", "control acciones en device" | Endpoint DLP (requiere E5) |
| "anomalous user behavior", "data theft by departing user", "insider risk patterns" | Insider Risk Management (E5) |
| "adaptive response based on user risk", "DLP escalation when IRM detects risk" | Adaptive Protection (combina IRM + DLP + CA) |
| "privacy risk", "GDPR subject rights requests (DSAR)", "personal data overexposed" | Microsoft Priva (NO Purview, producto separado) |
| "visibility de Copilot M365", "ver qué datos accede Copilot", "prompts/responses analysis" | Purview DSPM for AI (requiere licencia Copilot) |
| "third-party AI apps" (ChatGPT, Gemini), "endpoint onboarding to Purview", "block uploads to AI" | Purview DSPM for AI + Endpoint DLP |
| "advanced eDiscovery", "custodians", "predictive coding", "near-duplicates" | eDiscovery Premium (E5) |
| "long-term audit retention", "MailItemsAccessed", "forensic investigation" | Audit Premium (E5) |

### Red y perímetro — palabras gancho

**Tipos de protección de red**

| Si ves... | Es... |
|---|---|
| "OWASP CRS", "Top 10", "L7 HTTP/HTTPS protection", "anti-bot" | WAF (App Gateway WAF o Front Door WAF) |
| "TLS inspection", "IDPS", "detect malicious domains over HTTPS", "filtering egress por FQDN/categoría" | Azure Firewall Premium (no Standard, no Basic) |
| "L3/L4 filtering subnet", "microsegmentation entre VMs", "agrupar por rol sin gestionar IPs" | NSG + ASGs |
| "centralized egress inspection multi-spoke" | Azure Firewall en Hub (Hub-and-Spoke) |

Application Gateway WAF → regional, dentro de la VNet, ideal para apps internas. Front Door WAF → global, edge, multi-región, anti-DDoS L7.

**Acceso administrativo (trampa recurrente)**

| Si ves... | Es... |
|---|---|
| "access via Azure portal browser sin cliente", "no public IP en VMs", "RDP/SSH from portal" | Azure Bastion |
| "Bastion sin IP pública propia", "session recording administrative", "private-only Bastion" | Azure Bastion Premium |
| "ventana temporal de acceso", "aprobación bajo demanda", "abrir puerto solo cuando se solicita" | JIT VM Access (Defender for Cloud) |
| "Bastion + peering minimize costs" | Un Bastion centralizado (no uno por VNet) |
| "cuántos Bastion subnets" con peering | Por hub regional, no por VNet |

**PaaS connectivity**

| Si ves... | Es... |
|---|---|
| "private IP for Storage/SQL/KeyVault", "no public access", "DNS privada", "resoluble desde on-prem" | Private Endpoint + Private DNS Zone |
| "optimize route to PaaS desde VNet", "restrict PaaS to a VNet" | Service Endpoint (tráfico va por IP pública pero optimizada) |
| "AKS public API restricted to specific IPs" | Authorized IP Ranges (NO Service Endpoint) |

Front Door access restrictions: restringir App Service a tráfico solo de Front Door → HTTP header `X-Azure-FDID`. Service tags y backend IP NO funcionan (compartidos con otros tenants, cambian).

Multinube: conectar AWS/GCP a Defender for Cloud → connectors nativos (CSPM básico sin agentes). Aplicar Azure Policy a EC2/GCE/on-prem → Azure Arc (extiende el control plane). Threats en AWS CloudTrail → Sentinel (con S3 connector) detecta automáticamente.

### Defender for Cloud — palabras gancho

| Si ves... | Es... |
|---|---|
| "JIT VM Access", "FIM", "Adaptive Application Controls", "Adaptive Network Hardening" | Defender for Servers Plan 2 |
| "Vulnerability assessment con Qualys engine" | Defender for Servers (Plan 1 o 2) |
| "security baselines compliance vs CIS/STIG", "monitor devices que ya no cumplen baseline" | Security Baselines Assessments en MDVM (no Secure Score) |
| "Defender plans + auto-provisioning", "AKS excluido del Secure Score" | Enable Defender for Containers + auto-provisioning |
| "malware scanning en blobs", "anomalías de acceso a Storage" | Defender for Storage |
| "SQL injection, brute force en DB", "data exfiltration de SQL" | Defender for Databases |
| "suspicious sign-ins en Key Vault", "enumeración de secretos" | Defender for Key Vault |
| "control plane Azure anomalies", "deployments masivos sospechosos" | Defender for Resource Manager |
| "API abuse", "PII en respuestas de API", "Azure API Management security" | Defender for APIs |
| "prompt injection runtime", "jailbreak attempts en Azure OpenAI", "Defender XDR alerts para AI" | Defender for AI services |
| "AI security posture", "are my Azure OpenAI resources configured correctly" | Defender CSPM con AI security posture |

### DevSecOps — palabras gancho

| Si ves... | Es... |
|---|---|
| "SAST", "código fuente analizado", "vulnerabilities in my code" | SAST (en fase commit/PR) |
| "Secret scanning", "API keys hardcoded", "tokens en repo" | Secret Scanning (en fase commit) |
| "SCA / Dependabot", "vulnerabilities in third-party libraries", "dependency updates" | Dependabot / SCA |
| "GitOps for Kubernetes", "manifest files in repo to clusters", "consistent AKS config" | Flux (NO Dependabot, NO Gatekeeper) |
| "Policy enforcement in K8s", "OPA-based admission control" | Gatekeeper |
| "all code via PR before deploy" | Branch policies en Azure Repos |
| "SAST + secret scanning + DAST integrado, GitHub repos" | GitHub Advanced Security |
| "hard-coded secrets detection en Azure DevOps" | GitHub Advanced Security (recomendado) o Defender for DevOps |
| "threat modeling top-down para CAF" | STRIDE (NO DREAD, NO OWASP) |
| "landing zone deployment con NSG+Firewall+Bastion+KV", "minimize deployment effort + CAF" | Azure Landing Zone Accelerator |
| "DevOps pipeline service connection con least privilege" | Workload Identity Federation + user-assigned managed identities |
| "CEF logs to Sentinel" | Syslog server on-prem con AMA (NO Logic App) |

### Backup y ransomware — palabras gancho

| Si ves... | Es... |
|---|---|
| "compromised admin cannot stop backups", "MUA for backups", "two-person rule on vault" | Multi-User Authorization (MUA) con Resource Guard |
| "immutable backups", "WORM", "cannot delete during retention" | Immutable vault |
| "critical operations require PIN" (anti-error, no anti-ataque) | Security PIN for critical operations |
| "integrity of data (no alteración) vs ransomware" | Immutable Storage + versioning |
| "confidentiality" | Cifrado en reposo (AES-256, CMK si rotación custom) |
| "availability" | Geo-redundant storage, multi-region restore |

### Frameworks — palabras gancho

| Si ves... | Es... |
|---|---|
| "enterprise-wide governance", "landing zone structures", "ciclo de vida adopción", "migration + governance + operations" | CAF (Cloud Adoption Framework) |
| "5 pillars of a workload", "Security/Reliability/Performance/Cost/Operational Excellence" | WAF (Well-Architected Framework) |
| "baseline técnico de controles" multinube, mapeado a NIST/CIS | MCSB (Microsoft Cloud Security Benchmark) |
| "catálogo de capacidades de seguridad Microsoft", referencia de arquitectura | MCRA (Microsoft Cybersecurity Reference Architectures) |
| Principios "verify explicitly / least privilege / assume breach" | Zero Trust |

---

## Licencias — chuleta rápida

**Entra**
- Free: security defaults (MFA todo o nada), SSPR básico cloud
- P1: CA granular, device compliance condition, Auth Strengths, SSPR con writeback, App Proxy
- P2: todo P1 + ID Protection, PIM, Access Reviews, Entitlement Management
- Entra ID Governance (SKU separado): incluye P2 + Lifecycle Workflows + SoD avanzado
- Permissions Management (SKU separado): CIEM multinube

**Purview**
- M365 E3: sensitivity labels manuales, DLP básico, eDiscovery Standard, Audit Standard
- M365 E5: todo E3 + auto-labeling, Endpoint DLP, Insider Risk, Communication Compliance, eDiscovery Premium, Audit Premium, DSPM for AI, Records Management, Adaptive Protection

**Defender for Cloud**
- Foundational CSPM (Free): Secure Score, MCSB recommendations, asset inventory
- Defender CSPM (Paid): Attack Path, Cloud Security Explorer, agentless scanning, AI posture, regulatory compliance completo
- Defender Plans (Servers, Containers, Storage, Databases, etc.): cada uno facturado por separado

---

## Las 10 trampas más probables del examen

Si ves estas palabras → respuesta automática:

1. "across all deployed services" / SIEM → Sentinel (no XDR)
2. "phishing-resistant MFA" → Authentication Strengths (no MFA genérico)
3. "shared Teams channels" → B2B Direct Connect (no B2B Collaboration)
4. "LDAP cloud-only minimize admin" → Entra Domain Services
5. "AKS public API + restrict IPs" → Authorized IP Ranges
6. "CEF firewall logs to Sentinel" → Syslog server con AMA
7. "K8s manifest files + consistent config" → Flux (no Dependabot)
8. "compliance vs CIS benchmark" → Security Baselines Assessments en MDVM (no Secure Score)
9. "compromised admin cannot stop backups" → MUA con Resource Guard
10. "Front Door access restriction to App Service" → HTTP header X-Azure-FDID

---

## Estrategia de examen

**Antes de empezar**
- Llega descansado, comido, hidratado
- 100 min para 40-60 preguntas, son ~2 min por pregunta
- No te quedes atascado: marca para revisión y sigue

**Durante el examen**
- Lee la pregunta dos veces antes de mirar las opciones
- Subraya mentalmente las palabras gancho del enunciado
- Elimina opciones obviamente erróneas primero
- Si dudas entre 2: relee los requisitos del enunciado, uno suele descartar una opción
- HOTSPOT: cada hueco vale 1 punto, no te quedes atascado en uno; sigue con el otro
- Multi-select: el enunciado siempre dice cuántas marcar; no marques de más
- Yes/No serie: cada solución es independiente, no las compares entre ellas

**Patrones de eliminación**
- Opciones con servicios claramente fuera del scope (ej: SQL en pregunta de red) → fuera
- Servicios deprecated (ADFS, Azure Blueprints, AIP legacy) → casi nunca correctos
- Opciones que violan principios CAF (overdeployment, no follow least privilege) → fuera
- Si dos opciones parecen correctas → lee el requisito "minimize..." del enunciado, suele descartar una

**Si dudas absolutamente**
- En SC-100, las respuestas que mencionan Azure Policy + RBAC + JIT + MFA suelen ser correctas (Zero Trust mainstream)
- Las que mencionan VPN tradicional, ADFS, contraseñas estáticas suelen ser incorrectas
- Multinube + Microsoft solution → Defender for Cloud + Azure Arc casi siempre encaja
