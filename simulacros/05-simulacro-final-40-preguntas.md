# SIMULACRO FINAL SC-100 — 40 preguntas (formato tipo examen)

**El último antes del examen del viernes.** Tiempo objetivo: 80 minutos (~2 min/pregunta, ritmo real).
Hazlo a papel cerrado. Apunta también las que dudes aunque aciertes. Solucionario al final.

Formato mixto como en el examen: single-choice, multi-select ("elige N"), HOTSPOT (emparejar), secuencias ordenadas y una serie Yes/No.

---

## PREGUNTAS

---

**1.** Necesitas publicar una aplicación web interna on-premises para usuarios remotos sin desplegar VPN ni abrir puertos entrantes. La app usa solo HTTP. ¿Qué recomiendas?
- A. Entra Private Access
- B. Entra Application Proxy
- C. Entra Internet Access
- D. Azure Bastion

---

**2.** La misma empresa necesita ahora dar acceso remoto a un servidor SSH y a una instancia SQL on-premises, sin VPN. ¿Qué recomiendas?
- A. Entra Application Proxy
- B. Entra Private Access
- C. Azure Bastion
- D. Service Endpoints

---

**3.** Una App Service debe quedar completamente inaccesible desde internet y alcanzable solo desde una VNet. Los despliegues vienen de Azure Pipelines. ¿Qué **tres** acciones incluyes? (Elige tres.)
- A. VNet Integration
- B. Private Endpoint en el endpoint principal y en el SCM
- C. Deshabilitar el acceso público
- D. Service Endpoint hacia la App Service
- E. Private DNS Zone
- F. Access Restrictions por IP

---

**4.** Necesitas conectar una App Service a un SQL Server on-premises (puerto TCP estático) sin VPN ni ExpressRoute, minimizando endpoints expuestos a internet. ¿Qué recomiendas?
- A. VNet Integration
- B. Hybrid Connections
- C. Private Endpoint en la App Service
- D. NAT Gateway

---

**5.** Una Storage Account guarda datos de tres divisiones en containers separados. Cada división debe poder revocar el acceso a sus datos de forma independiente. ¿Qué recomiendas?
- A. Una CMK única a nivel de cuenta
- B. Encryption scopes con una CMK por container
- C. Tres cuentas de Storage separadas
- D. Client-side encryption

---

**6.** Una entidad exige que las claves residan en hardware **dedicado, no compartido con otros clientes**, FIPS 140-2 Level 3, y autorización de plano de datos granular. ¿Qué recomiendas?
- A. Azure Key Vault Standard
- B. Azure Key Vault Premium con access policies
- C. Azure Managed HSM con RBAC
- D. Client-side encryption con HYOK

---

**7.** Un requisito exige que la clave de cifrado **nunca exista dentro de Azure**, ni siquiera en Managed HSM, para datos en Blob Storage. ¿Qué recomiendas?
- A. CMK en Managed HSM con BYOK
- B. Encryption scopes por container
- C. Client-side encryption con claves gestionadas fuera de Azure
- D. Infrastructure encryption

---

**8.** Un Synapse **Serverless** SQL pool debe cumplir cifrado con CMK. ¿Dónde se aplica?
- A. TDE con CMK en el propio pool
- B. En el Storage Account del Data Lake subyacente
- C. Recreando el workspace con CMK
- D. Always Encrypted con secure enclaves

---

**9.** Una columna `NumeroCuenta` (alta cardinalidad) necesita soportar `WHERE =` bajo Always Encrypted sin enclave; una columna `Pais` (baja cardinalidad) no se consulta. ¿Qué cifrado para cada una?
- A. NumeroCuenta: Randomized · Pais: Deterministic
- B. NumeroCuenta: Deterministic · Pais: Randomized
- C. Ambas Deterministic
- D. Ambas Randomized

---

**10.** Necesitas ejecutar consultas de **rango** (`>`, `<`) sobre una columna cifrada con Always Encrypted. ¿Qué requieres?
- A. Cifrado Deterministic
- B. Cifrado Randomized
- C. Secure Enclaves
- D. Dynamic Data Masking

---

**11.** Los DBAs (sysadmin) no deben poder ver el valor real de una columna, ni con herramientas de gestión SQL. ¿Qué recomiendas?
- A. TDE con CMK
- B. Always Encrypted
- C. Dynamic Data Masking
- D. Row-Level Security

---

**12.** Debes proteger las estaciones de trabajo contra ransomware de día cero impidiendo que procesos no autorizados escriban en las carpetas de documentos. ¿Qué recomiendas?
- A. BitLocker
- B. Controlled Folder Access
- C. Attack Surface Reduction rules
- D. Windows LAPS

---

**13.** Debes bloquear patrones de comportamiento como Office lanzando procesos hijo o scripts ofuscados, con independencia de la carpeta. ¿Qué recomiendas?
- A. Controlled Folder Access
- B. Attack Surface Reduction rules
- C. BitLocker
- D. Microsoft Defender Antivirus con firmas

---

**14.** Diseñas backups para 200 servidores. Requisito: *"ni un Global Administrator comprometido del tenant puede eliminar los backups. Máxima protección."* ¿Qué recomiendas?
- A. Soft delete con retención extendida
- B. MUA con Resource Guard en otra suscripción del mismo tenant
- C. MUA con Resource Guard en un tenant distinto
- D. Security PIN para operaciones críticas

---

**15.** Un CISO pregunta qué protege la **integridad** de los backups frente a ransomware (la "I" de CIA). ¿Qué recomiendas?
- A. Cifrado AES-256 con CMK
- B. Immutable vault con WORM y versionado
- C. Geo-redundant storage
- D. BitLocker en los servidores de origen

---

**16.** Debes optimizar la **investigación** de un incidente de ransomware según DART. Ordena:
1. Identificar el proceso de recuperación del compromiso
2. Evaluar la situación e identificar el alcance
3. Identificar qué aplicaciones de negocio están no disponibles

- A. 2 → 3 → 1
- B. 1 → 2 → 3
- C. 3 → 2 → 1
- D. 2 → 1 → 3

---

**17.** Se detecta Kerberoasting contra cuentas de servicio en el dominio AD DS on-premises. ¿Qué producto lo detecta?
- A. Microsoft Entra ID Protection
- B. Microsoft Defender for Identity
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Sentinel UEBA

---

**18.** Una cuenta inicia sesión desde Madrid y 20 minutos después desde Yakarta (viaje imposible en el login cloud). ¿Qué producto lo detecta?
- A. Microsoft Defender for Identity
- B. Microsoft Entra ID Protection
- C. Microsoft Defender for Cloud Apps
- D. Microsoft Defender for Endpoint

---

**19.** Un usuario ya autenticado descarga 5.000 archivos de SharePoint Online en diez minutos. ¿Qué producto lo detecta?
- A. Entra ID Protection
- B. Defender for Identity
- C. Defender for Cloud Apps
- D. Defender for Endpoint

---

**20.** Debes exigir que los administradores usen exclusivamente FIDO2 o Windows Hello para acceder al portal de Azure. ¿Qué configuras?
- A. Security defaults
- B. Conditional Access con Authentication Strengths
- C. Conditional Access exigiendo MFA
- D. Sign-in risk policy

---

**21.** Debes impedir que los usuarios accedan a las enterprise applications desde una lista concreta de países. ¿Qué recomiendas?
- A. Sign-in risk policy en Entra ID Protection
- B. Conditional Access con named locations
- C. Activity policy en Defender for Cloud Apps
- D. User risk policy en Entra ID Protection

---

**22.** Necesitas que EXO, SPO y Teams reaccionen **casi en tiempo real** cuando se deshabilita una cuenta o se restablece una contraseña, sin esperar a la caducidad del token. ¿Qué recomiendas?
- A. Reducir la vida del token
- B. Continuous Access Evaluation con Conditional Access
- C. Sign-in frequency de 1 hora
- D. PIM con activación de 1 hora

---

**23.** Empareja cada requisito de identidad on-prem con su control (HOTSPOT):

| Requisito | Control |
|---|---|
| Restringir DESDE DÓNDE puede autenticarse una cuenta Tier 0 (solo desde PAW) | ? |
| Impedir Pass-the-Hash en cuentas privilegiadas (sin NTLM, sin cache) | ? |
| Controlar el TIPO de logon permitido (interactive vs RDP) | ? |

Opciones: *Protected Users · Authentication Policy Silos · User Rights Assignment (GPO) · Windows LAPS*

---

**24.** Una app de facturación SaaS multi-tenant está registrada en el tenant del fabricante `vendor.com`. Necesitas que los usuarios de tu tenant se autentiquen en ella. ¿Qué creas en tu tenant?
- A. Un application object
- B. Una enterprise application (service principal)
- C. Una managed identity
- D. Un relying party trust en AD FS

---

**25.** Unas VMs que se **destruyen y recrean varias veces al día** deben leer secretos de un Key Vault, minimizando los principals con acceso y sin almacenar secretos. ¿Qué identidad y endpoint?
- A. IMDS + system-assigned managed identity
- B. IMDS + user-assigned managed identity
- C. Private endpoint + service principal con secreto
- D. Endpoint público + certificado

---

**26.** Litware usa PTA con PHS deshabilitado. Debe habilitar leaked credential detection manteniendo PTA como método principal. ¿Qué recomiendas?
- A. Migrar completamente a PHS
- B. Habilitar PHS como respaldo, manteniendo PTA
- C. Habilitar Smart Lockout
- D. Desplegar AD FS

---

**27.** Una empresa adquiere otra con un bosque AD DS **sin relación de confianza** y quiere sincronizar sus usuarios sin desplegar servidores adicionales. ¿Qué recomiendas?
- A. Entra Connect Sync
- B. Entra Cloud Sync
- C. AD FS con claims rules
- D. Cross-tenant synchronization

---

**28.** Debes impedir que un usuario tenga simultáneamente "Finance Read" y "Finance Write" (SoD, SOX), **previniendo la asignación**. ¿Qué recomiendas?
- A. Conditional Access
- B. Access Reviews
- C. Incompatible access packages en Entitlement Management
- D. Administrative Units

---

**29.** Partners externos con tenant Entra propio necesitan acceder a un **canal compartido de Teams** manteniendo su identidad nativa y sin cuentas de invitado. ¿Qué recomiendas?
- A. B2B Collaboration
- B. B2B Direct Connect
- C. Cross-tenant synchronization
- D. Entra External ID

---

**30.** Necesitas ingerir logs CEF de firewalls de terceros on-premises en Microsoft Sentinel. ¿Qué recomiendas?
- A. Una Logic App
- B. Un servidor Syslog on-prem con Azure Monitor Agent
- C. El conector nativo de Defender for Cloud
- D. Azure Arc en cada firewall

---

**31.** Empareja cada término SecOps con su descripción (HOTSPOT):

| Escenario | Producto |
|---|---|
| "Across all deployed services", SIEM/SOAR, hunting, third-party logs | ? |
| Correlación endpoint+identidad+email+cloud apps de Microsoft, self-healing | ? |
| Postura + protección de workloads Azure/AWS/GCP, Secure Score | ? |

Opciones: *Microsoft Sentinel · Microsoft Defender XDR · Microsoft Defender for Cloud*

---

**32.** Necesitas identificar identidades sobre-privilegiadas simultáneamente en Azure, AWS y GCP, con una métrica del exceso de permisos. ¿Qué recomiendas?
- A. Privileged Identity Management
- B. Entra Permissions Management
- C. Defender CSPM Attack Path Analysis
- D. Conditional Access

---

**33.** Debes aplicar las mismas políticas de configuración a instancias EC2 de AWS y servidores on-premises que ya gestionas desde Azure. ¿Qué recomiendas?
- A. Azure Policy directamente sobre EC2
- B. Azure Arc + Azure Policy
- C. Defender for Cloud con el conector de AWS únicamente
- D. Azure Blueprints

---

**34.** Detectas actividad de autenticación sospechosa en el dashboard de Defender for Cloud y necesitas evaluar y remediar las alertas con workflow automation, minimizando el esfuerzo de desarrollo. ¿Qué incluyes?
- A. Azure Monitor webhooks
- B. Azure Event Hubs
- C. Azure Functions apps
- D. Azure Logic Apps

---

**35.** Debes asegurar la comunicación servicio-a-servicio con mTLS entre 30 microservicios de AKS, minimizando el esfuerzo administrativo. ¿Qué recomiendas?
- A. Flux
- B. Envoy configurado manualmente
- C. Dapr
- D. Istio

---

**36.** Despliegas App Services mediante Azure DevOps y debes asegurar que las apps desplegadas **mantengan el cumplimiento** de las recomendaciones del MCSB. ¿Qué recomiendas?
- A. DevOps security en Defender for Cloud
- B. Defender for App Service
- C. Azure Policy
- D. Una branch policy

---

**37.** Empareja cada tarea DevSecOps con su fase del CI/CD (HOTSPOT):

| Tarea | Fase |
|---|---|
| Threat modeling | ? |
| Dynamic application security testing (DAST) | ? |
| Actionable intelligence (threat intel/monitorización) | ? |

Opciones: *Plan and develop · Build and test · Operate*

---

**38.** Debes bloquear que los empleados envíen prompts con datos confidenciales a webs de IA de terceros (ChatGPT, Gemini) desde Windows, y escanear los prompts. Purview solo protege EXO y SPO hoy. ¿Qué necesitas y qué va primero?
- A. Sensitivity labels + Insider Risk; primero auto-labeling
- B. Endpoint DLP + DSPM for AI; primero licenciar Copilot
- C. Endpoint DLP + DSPM for AI; primero onboarding de dispositivos en Purview
- D. Conditional Access + MDA session policy; primero crear la session policy

---

**39.** Serie Yes/No — Debes proteger backups en un Recovery Services vault contra un administrador comprometido. Cada solución es independiente; responde Sí/No a si cumple el objetivo de *"exigir aprobación de un segundo actor para operaciones críticas"*:

- **39a.** Habilitar soft delete con retención de 180 días. → Sí / No
- **39b.** Configurar MUA con un Resource Guard. → Sí / No
- **39c.** Requerir un Security PIN para operaciones críticas. → Sí / No

---

**40.** Empareja cada requisito multi-tenant/M&A con su solución (HOTSPOT):

| Requisito | Solución |
|---|---|
| Gestión centralizada de cientos de suscripciones entre tenants SIN cuentas de invitado | ? |
| Aplicar Azure Policy y patch management a recursos on-premises desde Azure | ? |
| Residencia de datos al recoger logs de filiales en distintos países | ? |

Opciones: *Azure Lighthouse · Azure Arc · Segmentar workspaces de Sentinel por región/tenant*

---
---

# SOLUCIONARIO

---

**1 — B.** Application Proxy. App web interna HTTP + reemplazar VPN + sin abrir puertos. (Si fuera no-HTTP → Private Access.)

**2 — B.** Private Access. SSH y SQL no son HTTP → App Proxy queda descartado; Private Access soporta cualquier TCP/UDP.

**3 — B, C, E.** Private Endpoint (principal + SCM) + deshabilitar acceso público + Private DNS Zone. VNet Integration es outbound (no aísla); Service Endpoint va VNet→PaaS (dirección contraria); Access Restrictions filtra pero conserva IP pública. **El SCM es lo que evita que fallen los despliegues.**

**4 — B.** Hybrid Connections (túnel saliente por 443 desde on-prem). VNet Integration no llega a on-prem sin VPN/ER; Private Endpoint es la dirección contraria.

**5 — B.** Encryption scopes con CMK por container. Sin scopes, toda la cuenta comparte una clave. Tres cuentas separadas funciona pero es rediseño innecesario (no la mínima suficiente).

**6 — C.** Managed HSM con RBAC. "Hardware dedicado / no compartido" → Managed HSM (Premium también es FIPS L3 pero HSM compartido). Managed HSM solo admite RBAC.

**7 — C.** Client-side encryption. "La clave nunca existe en Azure" = HYOK, no soportado en cifrado nativo de PaaS → cifras antes de subir. BYOK mete la clave en Azure (justo lo prohibido).

**8 — B.** CMK en el Storage Account del Data Lake. El serverless no tiene storage propio → no tiene TDE.

**9 — B.** NumeroCuenta Deterministic (permite `WHERE =`, apto para alta cardinalidad), Pais Randomized (máxima seguridad, no se consulta). Invertir la lógica de cardinalidad es la trampa.

**10 — C.** Secure Enclaves. Ni deterministic ni randomized (clásico) soportan rango; el enclave descifra en memoria aislada.

**11 — B.** Always Encrypted. La clave nunca reside en el servidor SQL, así que ni sysadmin ve el claro. TDE cifra en reposo pero el DBA sí ve el dato al consultar.

**12 — B.** Controlled Folder Access (allow-list sobre carpetas designadas → frena 0-day). ASR es comportamientos de proceso; BitLocker no protege contra ransomware.

**13 — B.** Attack Surface Reduction rules (bloquea comportamientos de proceso, independiente de carpeta). CFA es carpetas concretas.

**14 — C.** Resource Guard en un tenant distinto. "Ni un Global Admin comprometido" + "máxima protección" → otro tenant, no otra suscripción.

**15 — B.** Immutable vault con WORM + versionado. El cifrado protege confidencialidad, no integridad. Geo-redundancia es disponibilidad; BitLocker es robo físico.

**16 — A.** 2 → 3 → 1 (evaluar alcance → LOB apps caídas → proceso de recuperación). *Entender antes de priorizar, priorizar antes de actuar.* Es la secuencia de **investigación** (la de recuperación empieza deshabilitando la sincronización).

**17 — B.** Defender for Identity. Kerberos/DCSync/on-prem → siempre esta; ID Protection es ciego a lo on-prem puro.

**18 — B.** Entra ID Protection. Viaje imposible en el login cloud = riesgo de sign-in.

**19 — C.** Defender for Cloud Apps. Post-login, comportamiento masivo en SaaS.

**20 — B.** Authentication Strengths. "Phishing-resistant / FIDO2 / Windows Hello" → método concreto. "MFA a secas" (C) admite SMS, no resistente.

**21 — B.** Conditional Access con named locations. País/ubicación es condición determinista, no riesgo calculado (eso sería ID Protection).

**22 — B.** Continuous Access Evaluation. Reacciona casi en tiempo real a eventos críticos sin esperar al token. Sign-in frequency sigue siendo "esperar".

**23 —**
| Requisito | Control |
|---|---|
| Desde dónde puede autenticarse Tier 0 | **Authentication Policy Silos** |
| Impedir Pass-the-Hash (sin NTLM, sin cache) | **Protected Users** |
| Tipo de logon (interactive vs RDP) | **User Rights Assignment (GPO)** |

**24 — B.** Enterprise application (service principal). El application object ya existe en `vendor.com`; en tu tenant creas el service principal al consentir la app.

**25 — B.** IMDS + user-assigned managed identity. La UAMI sobrevive a la recreación de las VMs y minimiza los principals con acceso; IMDS entrega el token sin almacenar credenciales.

**26 — B.** PHS como respaldo manteniendo PTA. PHS es la única forma de leaked credential detection; no hace falta migrar del todo.

**27 — B.** Entra Cloud Sync. "Disconnected forest / M&A sin trust / sin servidores adicionales" → Cloud Sync (agente ligero). Connect Sync exige servidor dedicado.

**28 — C.** Incompatible access packages. Previene la asignación. CA bloquea sign-ins (no asignaciones); Access Reviews detectan después.

**29 — B.** B2B Direct Connect. "Shared Teams channels + identidad nativa + sin guests" → siempre Direct Connect.

**30 — B.** Servidor Syslog on-prem con AMA. Logic Apps nunca para ingesta.

**31 —**
| Escenario | Producto |
|---|---|
| SIEM/SOAR, across all services, third-party | **Microsoft Sentinel** |
| Correlación de los Defenders de Microsoft, self-healing | **Microsoft Defender XDR** |
| Postura/protección Azure/AWS/GCP, Secure Score | **Microsoft Defender for Cloud** |

**32 — B.** Entra Permissions Management (CIEM). Multinube + Permission Creep Index. PIM no cubre AWS/GCP.

**33 — B.** Azure Arc + Azure Policy. Arc proyecta EC2/on-prem en el plano de control de Azure. Blueprints está deprecado.

**34 — D.** Azure Logic Apps. Workflow automation de Defender for Cloud dispara Logic Apps sobre alertas. Functions requeriría más esfuerzo de desarrollo.

**35 — D.** Istio. mTLS servicio-a-servicio + mínimo esfuerzo → service mesh. Envoy sabe hacer mTLS pero a mano (contra el requisito); Dapr son bloques de app; Flux es GitOps.

**36 — C.** Azure Policy. El objeto son las apps ya desplegadas cumpliendo el MCSB → gobernanza del recurso. Que venga de Azure DevOps es contexto. *Defender recomienda y alerta; Azure Policy aplica y bloquea.*

**37 —**
| Tarea | Fase |
|---|---|
| Threat modeling | **Plan and develop** |
| DAST | **Build and test** |
| Actionable intelligence | **Operate** |

**38 — C.** Endpoint DLP + DSPM for AI; primero onboarding de dispositivos en Purview. La trampa (B): las apps de IA de terceros NO requieren licencia de Copilot; eso es solo para ver M365 Copilot.

**39 —**
- **39a. No.** Soft delete da margen temporal, pero no exige aprobación de un segundo actor.
- **39b. Sí.** MUA con Resource Guard es precisamente la regla de dos personas.
- **39c. No.** El Security PIN es anti-error humano; el atacante con la misma credencial lo genera. No es un segundo actor.

**40 —**
| Requisito | Solución |
|---|---|
| Gestión cross-tenant sin guests | **Azure Lighthouse** |
| Azure Policy/patching sobre on-prem | **Azure Arc** |
| Residencia de datos por país | **Segmentar workspaces de Sentinel por región/tenant** |

---

## AUTOEVALUACIÓN

| Aciertos | Lectura |
|---|---|
| 36-40 | Listo. Llegas al viernes con margen de sobra. |
| 32-35 | Aprobado holgado. Repasa solo el bloque donde se concentren los fallos. |
| 28-31 | Aprobarías, pero mira si hay un patrón (un producto que confundes) y aíslalo. |
| <28 | Un concepto atascado, no falta de nivel. Identifícalo con la tabla de abajo. |

**Mapa pregunta → bloque del diccionario** (para localizar rápido qué repasar si fallas):
1-2 acceso a apps · 3-4 aislamiento App Service · 5-7 cifrado storage · 8-11 cifrado SQL/Synapse · 12-13 endpoints · 14-16 ransomware/backup · 17-19 tres detecciones de identidad · 20-22 Conditional Access · 23 AD on-prem · 24-25 identidad de apps · 26-27 identidad híbrida · 28-29 identity governance · 30-31 SecOps · 32-33 multinube · 34 automation · 35 AKS/service mesh · 36-37 DevSecOps · 38 Purview/IA · 39 MUA · 40 M&A/Lighthouse.

Si sacas 36+, no metas más volumen mañana ni el jueves: relee el diccionario una vez y descansa. Llegas afilado el viernes.
