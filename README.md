# SC-100 sin experiencia previa en Azure: cómo aprobé y como suspendí el primer intento.

Aprobé el **SC-100: Microsoft Cybersecurity Architect** con **721/1000**, sin tener ninguna certificación previa de Azure y viniendo del mundo AWS.

No fue a la primera. Suspendí con un **679** (no veas como dolió), cambié de método, y aprobé en el segundo intento. Este repo documenta **el proceso completo**: qué falló, qué cambié, y todo el material que construí por el camino, por si le sirve a alguien que está en el mismo punto en el que yo estaba.

> ⚠️ **Aviso:** esto NO es un dump de preguntas ni un atajo. Es un método de estudio y un mapa de conceptos. El SC-100 es un examen de criterio: premia entender *por qué* una solución encaja, no memorizar respuestas, por eso tienens que saber como funciona cada pequeña parte, porque en ese detalle esta la respuesta correcta.

---

## 📌 Contexto: de dónde partía

- **Base:** ciberseguridad y DevOps sobre AWS. Cero certificaciones de Azure.
- **Reto:** el SC-100 es de nivel *architect* y asume familiaridad con el ecosistema Microsoft (Entra, Purview, Defender, Sentinel, Azure Policy…).
- **Conclusión:** se puede, pero el salto no es el temario de seguridad eso. ya lo tenía, sino **el vocabulario de productos de Microsoft y sus fronteras**. Ahí es donde se gana o se pierde el examen.

---

## ❌ Primer intento: 679 (suspenso)

El desglose oficial señaló dos dominios flojos:

- **Cifrado** (Storage, SQL, Synapse, Key Vault / Managed HSM)
- **Respuesta a ransomware y backup**

El error de fondo no fue de conocimiento, sino de **método**: estudié por definiciones. Y el SC-100 no pregunta definiciones, pregunta cuál de dos servicios que *suenan igual* es el correcto según una palabra concreta del enunciado. 

Por otro lado, mi método de estudio, basado en videos de Udemy y tests creados con Notebook, me dejaron un conocimiento superficial y que no podía enseñarme lo que yo mismo no era capaz de ver. Las áreas sobre las que no se hacía hincapié, eran áreas que se iba a quedar en la sombra.

---

## ✅ Qué cambié (el método que funcionó)

### 1. Escenarios antes que teoría 
Por cada concepto flojo:
1. Partía de un **escenario concreto** ("una App Service que debe quedar inaccesible desde internet").
2. Lo **razonaba en voz alta** hasta atascarme.
3. Solo entonces iba a **Microsoft Learn** a validar.
4. Cerraba con **variantes** ("¿y si además hay que llegar desde on-prem?").

Aprender *por qué* VNet Integration no aísla nada por sí sola. No memoricé la descripción, entendí las limitaciones.

### 2. Un diccionario de "palabras gancho"
El núcleo del método. Una chuleta que mapea **qué término del enunciado activa qué servicio**. Ejemplos:

| Si el enunciado dice… | Es… |
|---|---|
| SSH / RDP / SQL hacia on-prem sin VPN | Entra Private Access (no Application Proxy) |
| "shared Teams channels" | B2B Direct Connect (no B2B Collaboration) |
| Synapse **Serverless** + cifrado | CMK en el Storage Account (no TDE) |
| "compromised admin cannot delete backups" | MUA con Resource Guard |
| Kerberos / DCSync / on-prem | Defender for Identity (no Entra ID Protection) |

El diccionario completo está en [`/diccionario`](./diccionario).

### 3. Simulacros con corrección razonada
~150 preguntas de práctica, pero con una regla: **no marcar y pasar**. Por cada pregunta, escribir *por qué* la correcta es correcta y *por qué* caen los distractores. Ahí es donde se consolida el criterio.

### 4. Cuestionar la fuente
Uno de los aprendizajes más útiles: los bancos de preguntas gratuitos **tienen respuestas mal argumentadas**. Aprender a detectar cuáles y por qué me obligó a razonar de verdad, que es exactamente lo que hace un arquitecto de seguridad: no aceptar la primera respuesta que suena bien.

---

## 📂 Qué hay en este repo

| Carpeta | Contenido |
|---|---|
| [`/diccionario`](./diccionario) | El diccionario de conceptos y trampas (palabras gancho por producto) |
| [`/simulacros`](./simulacros) | Simulacros propios con solucionario razonado |
| [`/casos-estudio`](./casos-estudio) | Casos de estudio tipo examen, trabajados paso a paso |
| [`/metodo`](./metodo) | El proceso de estudio en detalle |

---

## 🧠 Los 5 bloques donde se decide el examen

1. **Sentinel vs Defender XDR vs Defender for Cloud**  Quién ingiere qué, quién correlaciona qué.
2. **Las 3 detecciones de identidad**  Entra ID Protection (login cloud) vs Defender for Identity (AD on-prem) vs Defender for Cloud Apps (sesión SaaS).
3. **Aislamiento de red**  Dirección del tráfico (inbound vs outbound) lo resuelve casi todo.
4. **Cifrado** ¿La clave sale de Azure o se queda? ¿protege contra quién?
5. **Gobernanza** CAF (toda la organización) vs WAF (una workload); Azure Policy aplica y bloquea, Defender recomienda y alerta.

---

## 🗺️ Qué viene después

El SC-100 es un paso dentro de un plan más amplio hacia **seguridad de IA y arquitectura multi-cloud**. Siguiente parada: **SC-500** (seguridad de IA: prompt injection, guardarraíles, Defender for AI) en cuanto el examen esté disponible.

---

## 🤝 Si estás preparándolo

Si vienes de AWS/GCP o de seguridad "clásica" y el ecosistema Microsoft te abruma: **se puede**, y más rápido de lo que crees si atacas el vocabulario de productos en vez del temario de seguridad. Abre un issue o escríbeme si te atascas, sobre todo en cifrado o ransomware, que son los que me costaron a mí.

---

*Este material es de elaboración propia con fines educativos. No reproduce contenido con copyright de Microsoft ni preguntas reales del examen. Las referencias apuntan siempre a Microsoft Learn.*
