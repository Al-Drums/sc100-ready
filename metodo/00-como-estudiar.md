# Cómo estudié el SC-100 (después de suspenderlo)

El primer intento lo preparé como se prepara cualquier examen técnico: temario, vídeos, apuntes. Saqué 679. El segundo lo preparé de otra forma y saqué 721.

Esto es lo que cambié.

---

## El diagnóstico: por qué falla el método clásico aquí

El SC-100 **no pregunta definiciones**. Pregunta cuál de dos servicios que suenan casi igual es el correcto, según una palabra concreta del enunciado.

Sabías perfectamente qué es VNet Integration y qué es un Private Endpoint. Y aun así fallas la pregunta, porque lo que se examina no es la definición: es que **VNet Integration afecta al tráfico saliente y no aísla nada**, mientras que el Private Endpoint afecta al entrante → y que aun poniendo el Private Endpoint, **la app sigue siendo accesible por su URL pública hasta que deshabilitas el acceso público explícitamente**.

Eso no se aprende leyendo definiciones. Se aprende chocando contra el escenario.

---

## Las 4 piezas del método

### 1. Escenario → razonar → Learn → variantes

Por cada concepto flojo, en este orden:

1. **Escenario concreto.** "Una App Service debe quedar inaccesible desde internet y los despliegues vienen de un pipeline."
2. **Razonar en voz alta** hasta atascarme, sin mirar nada.
3. **Validar en Microsoft Learn** solo entonces. El atasco previo es lo que hace que la lectura se fije.
4. **Variantes.** "¿Y si además hay que resolver el DNS desde on-premises?" "¿Y si el protocolo no fuera HTTP?"

El paso 4 es el que más rinde: convierte un dato en un criterio.

### 2. El diccionario de palabras gancho

Una hoja viva donde, cada vez que fallo algo, apunto **qué término del enunciado debería haberme activado qué servicio**. No la definición: el disparador.

Ejemplos reales de mi diccionario:

- SSH / RDP / SQL hacia on-prem sin VPN → **Private Access** (Application Proxy solo habla HTTP)
- "shared Teams channels" → **B2B Direct Connect** (no B2B Collaboration, que crea invitados)
- Synapse **Serverless** + cifrado → CMK sobre el **Storage Account** (no tiene TDE porque no tiene almacenamiento propio)
- "hardware dedicado / single tenancy" → **Managed HSM** (el nivel FIPS no distingue: Premium también es L3)
- Kerberos / DCSync / controlador de dominio → **Defender for Identity** (Entra ID Protection es ciego a lo on-prem)

Al final el diccionario era más útil que cualquier apunte, porque estaba escrito contra **mis** errores.

### 3. Simulacros con corrección razonada

Unas 150 preguntas de práctica, con una regla innegociable: **no marcar y pasar**.

Por cada pregunta, escribir:
- por qué la correcta es correcta,
- **y por qué cae cada distractor.**

Si no sabes explicar por qué falla el distractor, no la sabías: la acertaste por eliminación, y eso no se sostiene en el examen real.

### 4. Cuestionar la fuente

Los bancos de preguntas gratuitos tienen respuestas **mal argumentadas**. Encontré varias con explicaciones que no se sostenían al contrastarlas con Microsoft Learn.

Lejos de ser un problema, resultó el mejor ejercicio del proceso: obliga a razonar en lugar de memorizar, y es exactamente lo que hace un arquitecto de seguridad → no quedarse con la primera respuesta que suena bien. **Contrasta siempre con Learn.**

---

## Los 5 bloques donde se decide el examen

1. **Sentinel vs Defender XDR vs Defender for Cloud.** Sentinel ingiere de cualquier fuente (SIEM/SOAR); XDR correlaciona los productos Microsoft; Defender for Cloud es postura y protección de workloads. "Across all services" → Sentinel, siempre.

2. **Las tres detecciones de identidad.** Entra ID Protection (riesgo en el login cloud) · Defender for Identity (ataques a AD on-prem) · Defender for Cloud Apps (comportamiento post-login en SaaS). La palabra "on-premises" o "Kerberos" decide.

3. **Aislamiento de red.** La dirección del tráfico resuelve casi todo: VNet Integration sale, Private Endpoint entra, Service Endpoint va de la VNet al PaaS.

4. **Cifrado.** ¿La clave sale de Azure o se queda? ¿Protege en reposo, en tránsito, o contra quien administra? TDE cifra en reposo pero el DBA ve el dato; Always Encrypted ni siquiera el sysadmin.

5. **Gobernanza.** CAF estructura toda la organización, WAF diseña una workload. Y la frase que más preguntas resuelve: **Defender recomienda y alerta; Azure Policy aplica y bloquea.**

---

## Estrategia el día del examen

- **Lee el enunciado dos veces antes de mirar las opciones.** Subraya mentalmente la palabra limitante.
- **Si dudas entre dos**, vuelve al requisito: casi siempre hay un *"minimizando..."* que descarta una.
- **Microsoft Learn está disponible durante el examen.** No para estudiar sobre la marcha →no da tiempo→ sino para verificar un dato concreto. Practica la navegación antes.
- **Case study:** localiza la cláusula que decide antes de leer opciones.
- **No te atasques.** Marca para revisión y sigue; en los HOTSPOT cada hueco puntúa por separado.

---

## Si vienes de fuera del ecosistema Microsoft

Yo venía de AWS y DevOps, sin ninguna certificación de Azure. El salto real **no es el temario de seguridad** →los principios son los mismos en cualquier nube→ sino **el vocabulario de productos y sus fronteras**.

Así que si estás en ese punto: no empieces por repasar seguridad. Empieza por construir el mapa de qué hace cada producto de Microsoft y, sobre todo, **dónde acaba cada uno**. Ahí es donde se gana el examen.
