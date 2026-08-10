# SIMULACRO SC-100 #3 → 20 preguntas

**Tiempo objetivo: 40 minutos.**

Ponderación según los fallos más comunes:
- **AKS, contenedores y service mesh** → 5
- **Azure Policy vs otros controles de gobernanza y DevOps** → 5
- **Global Secure Access en profundidad** → 4
- **Backup: inmutabilidad y secuencias** → 3
- Mixtas → 3

---

## FICHA PREVIA → Lo que no habías visto

### El ecosistema de Kubernetes: quién hace qué

Cuatro herramientas que el examen mezcla constantemente. La clave es que **cada una resuelve un problema distinto** y no compiten entre sí:

| Herramienta | Qué es | Resuelve |
|---|---|---|
| **Istio** | **Service mesh** completo | Comunicación **servicio-a-servicio**: mTLS automático, políticas de tráfico, observabilidad |
| **Envoy** | **Proxy** (sidecar) | Es el *componente* que Istio usa por debajo. Por sí solo no es un mesh: no orquesta ni gestiona políticas |
| **Dapr** | Runtime de **componentes** para microservicios | State stores, pub/sub, invocación de servicios. Es un modelo de programación, no una capa de seguridad de red |
| **Flux** | **GitOps** | Sincroniza manifiestos de Git hacia el cluster. Configuración, no tráfico |
| **Gatekeeper** | **Admission control** (OPA) | Impide que se despliegue lo que no cumple política |

**La regla que resuelve este escenario y sus variantes:**

> **mTLS entre servicios + mínimo esfuerzo administrativo → Istio.**

Por qué las otras fallan aunque suenen razonables:
- **Envoy** *sí* sabe hacer mTLS, pero configurarlo a mano en cada servicio es justo lo contrario de "minimizar esfuerzo administrativo". Istio inyecta los sidecars Envoy automáticamente.
- **Dapr** es para *construir* microservicios (estado, mensajería), no para asegurar el tráfico entre ellos.
- **Flux** despliega configuración; no toca el tráfico en ejecución.

Palabras gancho: *"service-to-service"*, *"mTLS"*, *"service mesh"*, *"sidecar"*, *"traffic policies entre microservicios"* → **Istio**.
Nota: en AKS existe el **Istio-based service mesh add-on**, que es la versión gestionada y refuerza aún más el criterio de "mínimo esfuerzo".

---

### Cuándo la respuesta es Azure Policy

Este es el discriminador que te falta, y es más simple de lo que parece. Azure Policy actúa sobre el **plano de control de Azure (ARM)**: evalúa y controla **recursos desplegados**, con independencia de cómo se hayan desplegado.

| El requisito habla de… | La respuesta es… |
|---|---|
| Que los **recursos desplegados** cumplan un estándar (MCSB, CIS, corporativo) | **Azure Policy** |
| **Impedir** que se cree un recurso mal configurado (efecto Deny) | **Azure Policy** |
| **Corregir** automáticamente configuración (DeployIfNotExists, Modify) | **Azure Policy** |
| Aplicar esto también a recursos **no-Azure** (EC2, GCE, on-prem) | **Azure Policy + Azure Arc** |
| Ver **hallazgos y recomendaciones** de seguridad de repos y pipelines | DevOps security en Defender for Cloud |
| Exigir **revisión de código** antes de mergear | Branch policies |
| **Detectar amenazas** en runtime sobre la app | El plan de Defender correspondiente |

**La distinción clave que falla la mayoría:** Defender for Cloud **recomienda y alerta**; Azure Policy **aplica y bloquea**. Si el verbo del enunciado es *"ensure compliance"*, *"enforce"*, *"prevent deployment of"*, *"maintain compliance"* → Azure Policy. Si es *"identify"*, *"detect"*, *"gain visibility"* → Defender.

Y ojo con la trampa del contexto DevOps: **que el enunciado mencione Azure DevOps no convierte la respuesta en una herramienta de DevOps.** En la ese escenario, lo que hay que garantizar es que las **apps ya desplegadas** cumplan el MCSB. Eso es gobernanza del recurso en Azure, no del pipeline. Azure Policy.

---

### Global Secure Access: compliant network

Concepto que aparece con frecuencia y que conviene tener claro:

- El **compliant network check** es una condición de Conditional Access que verifica que el usuario llega **a través del servicio Global Secure Access**, no desde una IP concreta. Sustituye al modelo antiguo de named locations por IP.
- Para que un dispositivo esté "en la red conforme" tiene que llegar **por uno de dos caminos**: el **cliente de Global Secure Access** instalado, o una **remote network** (túnel del router de la oficina) configurada y asignada a la política.
- Si una oficina no tiene ni cliente desplegado ni remote network asignada, **su tráfico no pasa por GSA** y por tanto no cumple el compliant network check.

---

## PREGUNTAS

---

**1.** Vas a desplegar varias aplicaciones de microservicios en contenedores sobre AKS. Necesitas gestionar secretos, proporcionar cifrado, asegurar la comunicación servicio-a-servicio con mTLS y minimizar el esfuerzo administrativo.

- A. Flux
- B. Envoy
- C. Dapr
- D. Istio

---

**2.** Debes garantizar que ningún pod se despliegue en AKS si su imagen procede de un registro no autorizado. El bloqueo debe producirse **en el momento del despliegue**.

- A. Istio con políticas de tráfico
- B. Gatekeeper (OPA admission control)
- C. Flux
- D. Defender for Containers con runtime protection

---

**3.** Necesitas que la configuración de tus clusters AKS sea consistente y se restaure automáticamente al estado declarado en Git si alguien la modifica manualmente.

- A. Flux
- B. Gatekeeper
- C. Dapr
- D. Azure Policy add-on para AKS

---

**4.** Debes detectar comportamiento malicioso **en tiempo de ejecución** dentro de los contenedores de AKS, como shells inversos o minería de criptomonedas.

- A. Vulnerability assessment en Defender for Containers
- B. Runtime threat protection en Defender for Containers
- C. Gatekeeper
- D. Defender CSPM con Attack Path Analysis

---

**5.** Debes restringir el acceso al **API server público** de un cluster AKS para que solo sea accesible desde las IPs públicas de las oficinas corporativas.

- A. Service Endpoints
- B. Private Endpoint
- C. Authorized IP Ranges
- D. Un NSG en la subnet del nodepool

---

**6.** Planeas desplegar aplicaciones de App Service mediante Azure DevOps. Debes asegurar que las aplicaciones desplegadas **mantengan el cumplimiento** de las recomendaciones del Microsoft Cloud Security Benchmark.

- A. DevOps security en Microsoft Defender for Cloud
- B. Microsoft Defender for App Service
- C. Azure Policy
- D. Una branch policy en Azure DevOps

---

**7.** Debes **impedir** que se creen Storage Accounts que permitan acceso público a blobs, en todas las suscripciones de un management group.

- A. Una recomendación de Defender for Cloud
- B. Azure Policy con efecto Deny
- C. Azure Policy con efecto Audit
- D. Una RBAC deny assignment

---

**8.** Debes asegurar que todas las máquinas virtuales nuevas tengan instalada la extensión de Azure Monitor Agent, **sin intervención manual** y corrigiendo también las existentes.

- A. Azure Policy con efecto DeployIfNotExists y una remediation task
- B. Azure Policy con efecto Audit
- C. Defender for Servers Plan 2 con auto-provisioning
- D. Una branch policy en el repositorio de IaC

---

**9.** Debes obtener visibilidad de secretos expuestos y configuraciones inseguras **en los repositorios y pipelines** de Azure DevOps y GitHub, agregando los hallazgos junto al resto de la postura cloud.

- A. Azure Policy
- B. DevOps security en Microsoft Defender for Cloud
- C. Branch policies
- D. Microsoft Sentinel

---

**10.** Debes aplicar las mismas políticas de configuración de seguridad a máquinas virtuales de AWS EC2 y a servidores físicos on-premises que ya gestionas desde Azure.

- A. Azure Policy directamente sobre los recursos
- B. Azure Arc + Azure Policy
- C. Defender for Cloud con el conector de AWS únicamente
- D. Azure Blueprints

---

**11.** Tu empresa tiene dos oficinas. Despliegas Global Secure Access con el perfil de Microsoft 365, una política de tráfico para todo el tráfico de M365 y una política de Conditional Access que exige **compliant network check**. Asignas una **remote network** solo a la Oficina 1 y despliegas el **cliente GSA** solo en los dispositivos de la Oficina 2.

¿Desde qué oficinas pueden acceder los usuarios a M365 con dispositivos conformes?

- A. Solo Oficina 1
- B. Solo Oficina 2
- C. Ambas oficinas
- D. Ninguna de las dos

---

**12.** ¿Qué verifica exactamente el **compliant network check** de Conditional Access?

- A. Que el dispositivo esté marcado como compliant en Intune
- B. Que la IP de origen esté en una named location de confianza
- C. Que el tráfico llegue a través del servicio Global Secure Access
- D. Que el usuario esté usando MFA resistente al phishing

---

**13.** Debes impedir que empleados con dispositivos corporativos se autentiquen en tenants de Microsoft 365 que no sean el de la empresa, incluso si usan cuentas personales.

- A. Entra Private Access
- B. Entra Internet Access con universal tenant restrictions v2
- C. Cross-tenant access settings
- D. Conditional Access bloqueando aplicaciones externas

---

**14.** Tu empresa quiere sustituir la VPN para acceso a aplicaciones internas **y además** filtrar el tráfico saliente hacia internet de los mismos dispositivos.

- A. Solo Entra Private Access
- B. Solo Entra Internet Access
- C. Entra Private Access e Internet Access, ambos bajo Global Secure Access
- D. Azure Firewall Premium con forced tunneling

---

**15.** Tienes un grupo de recursos con múltiples Azure File shares. Debes desplegar una solución de backup que impida la eliminación de los backups y del vault, impida ataques de escalada de privilegios contra la solución de backup e impida modificar el periodo de retención.

¿En qué orden deben realizarse estas tres acciones?

1. Bloquear (lock) la inmutabilidad del vault
2. Crear un Recovery Services vault
3. Habilitar la inmutabilidad del vault

- A. 2 → 3 → 1
- B. 2 → 1 → 3
- C. 3 → 2 → 1
- D. 1 → 2 → 3

---

**16.** ¿Qué diferencia hay entre habilitar la inmutabilidad de un vault y bloquearla?

- A. Ninguna: son la misma operación con nombres distintos
- B. Habilitada se puede deshabilitar después; bloqueada es irreversible
- C. Habilitada aplica solo a backups nuevos; bloqueada a todos
- D. Habilitada requiere MUA; bloqueada no

---

**17.** Debes proteger los backups frente a un administrador comprometido que intente **detener las operaciones de backup**, exigiendo aprobación de un segundo actor.

- A. Inmutabilidad bloqueada del vault
- B. MUA con Resource Guard
- C. Soft delete mejorado
- D. Security PIN

---

**18.** Necesitas detectar malware en archivos **después de que se suban** a un container de Blob Storage.

- A. Sensitive data threat detection en Defender for Storage
- B. Runtime threat protection en Defender for Containers
- C. Vulnerability assessment en Defender for Containers
- D. Malware scanning en Defender for Storage

---

**19.** Un Synapse **Serverless** SQL pool debe cumplir un requisito de cifrado con customer-managed key.

- A. TDE con CMK en el propio pool serverless
- B. CMK sobre el Storage Account del Data Lake subyacente
- C. Recrear el workspace con CMK
- D. Always Encrypted con secure enclaves

---

**20.** Debes asegurar que un pipeline de Azure DevOps no pueda desplegar código que no haya pasado por revisión de al menos dos personas.

- A. Azure Policy
- B. Branch policies en Azure Repos
- C. Workload Identity Federation
- D. DevOps security en Defender for Cloud

---
---

# SOLUCIONARIO

---

**1 → D.** Istio.
🔑 **mTLS servicio-a-servicio + mínimo esfuerzo → service mesh → Istio.**
❌ B: Envoy es el proxy que Istio usa por debajo; sabe hacer mTLS pero exige configurarlo y orquestarlo a mano, lo contrario de "minimizar esfuerzo". Es el distractor mejor diseñado de la pregunta. ❌ C: Dapr aporta bloques de construcción (state, pub/sub), no una capa de seguridad de red completa. ❌ A: Flux es GitOps, despliega configuración.

**2 → B.** Gatekeeper.
🔑 **"Impedir el despliegue" en Kubernetes → admission control → Gatekeeper (OPA).**
❌ D: Defender for Containers detecta en runtime, es decir **después** de que el pod corra.
⚠️ Nota: el **Azure Policy add-on para AKS** está construido sobre Gatekeeper y sería válido si el enunciado menciona gobernanza centralizada desde Azure. Si la pregunta es puramente de Kubernetes, Gatekeeper.

**3 → A.** Flux.
🔑 **"Estado declarado en Git" + "consistencia de configuración" → GitOps → Flux.**
La reconciliación continua (revertir cambios manuales) es exactamente la definición de GitOps.

**4 → B.** Runtime threat protection en Defender for Containers.
🔑 **"En tiempo de ejecución" + comportamiento malicioso → CWPP.**
❌ A: el vulnerability assessment analiza imágenes en busca de CVEs, es postura, no detección activa. Es el contraste CSPM/CWPP aplicado a contenedores.

**5 → C.** Authorized IP Ranges.
🔑 Feature **nativa de AKS**. No es Service Endpoint ni Private Endpoint. (Private Endpoint sería la respuesta si el enunciado pidiera un cluster **privado sin API server público**, que es otra pregunta.)

**6 → C.** Azure Policy.
🔑 **La clave está en el objeto del requisito: las apps YA DESPLEGADAS deben cumplir el MCSB.** Eso es gobernanza sobre recursos de Azure. Que el despliegue venga de Azure DevOps es contexto, no el objeto.
❌ A: DevOps security da visibilidad de repos y pipelines, no aplica cumplimiento sobre el recurso desplegado. ❌ B: Defender for App Service **detecta amenazas**, no aplica cumplimiento. ❌ D: las branch policies gobiernan el código, no la configuración del recurso en Azure.
⚠️ **Recuerda: Defender recomienda y alerta; Azure Policy aplica y bloquea.**

**7 → B.** Azure Policy con efecto **Deny**.
🔑 **"Impedir que se creen" → Deny.** Audit solo registra el incumplimiento.
❌ D: las deny assignments existen pero se usan vía Blueprints/managed apps, no son el mecanismo de gobernanza de configuración.

**8 → A.** DeployIfNotExists + remediation task.
🔑 **"Sin intervención manual" + "corregir también las existentes"**. DeployIfNotExists actúa sobre lo nuevo; la **remediation task** es la que aplica a lo que ya existía. Si el enunciado menciona recursos preexistentes, la tarea de remediación tiene que aparecer.

**9 → B.** DevOps security en Defender for Cloud.
🔑 **"Visibilidad" + "repositorios y pipelines" + "junto al resto de la postura" → DevOps security.**
Contrasta deliberadamente con la 6: aquí el objeto es el **pipeline**; allí era el **recurso desplegado**.

**10 → B.** Azure Arc + Azure Policy.
🔑 **Azure Policy solo alcanza lo que está en el plano de control de ARM.** Arc es lo que proyecta EC2 y servidores on-prem dentro de ese plano.
❌ D: Blueprints está deprecado; casi nunca es correcto.

**11 → A.** Solo Oficina 1.
Este es el razonamiento completo:
- La política de Conditional Access exige **compliant network check**, que solo se satisface si el tráfico atraviesa Global Secure Access.
- **Oficina 1** tiene una **remote network asignada** a la política → su tráfico entra por GSA → cumple.
- **Oficina 2** tiene el cliente GSA desplegado, pero la **remote network asignada a la política es la de Oficina 1**. El tráfico de Oficina 2 no queda cubierto por la asignación de la política, así que no supera el compliant network check.
🔑 La lección: **desplegar el cliente no basta si la política no está asignada a ese camino.** Hay que mirar siempre a qué está asignada la política, no solo qué se ha instalado.

**12 → C.** Que el tráfico llegue a través de Global Secure Access.
🔑 Es un sustituto moderno de las named locations por IP: en vez de "vienes de esta IP", comprueba "vienes por mi servicio". ❌ A: eso es device compliance, otra condición distinta de CA.

**13 → B.** Internet Access con universal tenant restrictions v2.
🔑 Respuesta confirmada. Funciona porque el tráfico **sale** hacia otro tenant y GSA lo inspecciona inyectando las cabeceras de restricción, tanto en el plano de autenticación como en el de datos.
❌ C: cross-tenant access settings controla la colaboración **entrante y saliente de tu tenant**, no impide que un dispositivo se autentique en un tenant ajeno con cuenta personal.
⚠️ Ojo: algunos bancos públicos responden Private Access aquí; es incorrecto.

**14 → C.** Ambos, bajo el paraguas de Global Secure Access.
No son alternativas: **Private Access = tráfico hacia recursos privados de la organización; Internet Access = tráfico hacia internet y SaaS.** Un mismo despliegue de GSA puede tener ambos perfiles activos.

**15 → A.** Crear vault → habilitar inmutabilidad → bloquear inmutabilidad.
🔑 **La secuencia es siempre: crear el contenedor, activar la protección, hacerla irreversible.** No puedes bloquear lo que no has habilitado, ni habilitar sobre un vault que no existe.

**16 → B.** Habilitada se puede deshabilitar; bloqueada es irreversible.
🔑 Este es el matiz que hace correcta la secuencia de la 15. El estado *habilitado* protege pero sigue siendo reversible por un administrador → por eso, si el requisito es *"ni siquiera un administrador puede modificar la retención"*, hay que llegar hasta el **lock**.
⚠️ El lock es permanente de verdad: no hay marcha atrás. Por eso es el último paso.

**17 → B.** MUA con Resource Guard.
🔑 **"Aprobación de un segundo actor" / two-person rule → MUA.** La inmutabilidad protege los datos de backup; **MUA protege las operaciones**.
Distinción útil: *inmutabilidad = no puedes borrar ni acortar retención. MUA = no puedes ejecutar operaciones críticas sin que otro apruebe.* La 15 y la 17 son complementarias, no alternativas.

**18 → D.** Malware scanning en Defender for Storage.
🔑 **"Detectar amenazas en archivos tras la subida" → malware scanning.**
❌ A: sensitive data threat detection identifica **datos sensibles expuestos**, no malware. Es el distractor del mismo producto, que es el patrón habitual.

**19 → B.** CMK sobre el Storage Account del Data Lake subyacente.
🔑 **El pool serverless no tiene storage propio, luego no tiene TDE.** El cifrado se aplica donde viven los datos.
⚠️ Contraste con **Dedicated**: ahí sí hay TDE, y la CMK de workspace **solo se configura al crear el workspace**.

**20 → B.** Branch policies.
🔑 **Revisión de código antes del merge → branch policies.** Aquí el objeto sí es el pipeline, a diferencia de la 6.

---

## AUTOEVALUACIÓN

| Aciertos | Lectura |
|---|---|
| 18-20 | Los cuatro temas están cerrados. |
| 15-17 | Repasa el bloque donde se concentren los fallos. |
| <15 | Vuelve a la ficha previa antes de seguir. |

**Las que más discriminan:** la 6 frente a la 9 y la 20 (mismo contexto DevOps, tres respuestas distintas según el **objeto** del requisito), y la 11 (asignación de política vs despliegue de cliente en GSA). Si esas cuatro salen limpias, tienes el criterio, no la memoria.
