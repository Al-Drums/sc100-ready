# EJEMPLO DE METODOLOGÍA DE ESTUDIO 

> Después de repasar unas cuantas preguntas y secciones, determiné este bloque de cierre con todos los gaps que tenía en mis estudios, sobre todo después del primer suspenso. Esto puede ayudar a como mejorar y crear un sistema de estudio que se adecúe a tu forma de estudiar. 

# BLOQUE DE CIERRE → Los 3 gaps abiertos

Verificado contra la documentación de Microsoft y GitHub el 23 de julio de 2026.
Lee primero las tres fichas, luego contesta las 10 preguntas.

---

# FICHA 1 → Azure Files: granularidad de la autorización

## El modelo de dos capas

Azure Files con autenticación basada en identidad tiene **dos niveles de permisos que se evalúan juntos**, y confundirlos es exactamente lo que el examen prueba:

| Capa | Mecanismo | Granularidad | Qué controla |
|---|---|---|---|
| **Share-level** | **Azure RBAC** (roles integrados) | **Solo el share completo** | Si el usuario puede acceder al share |
| **Directory/file-level** | **Windows ACLs (NTFS)** | Root, directorio o archivo individual | Qué operaciones puede hacer dentro |

La regla de oro: **Azure RBAC no baja de share.** Si necesitas permisos distintos por carpeta o por archivo, eso **no** se hace con RBAC → se hace con ACLs NTFS.

## Cómo se combinan

Se aplican **ambas** capas y gana **la más restrictiva**. Lectura/escritura en la ACL del archivo + solo lectura en el share = solo lectura. Y al revés también.

Traducción práctica: **el RBAC es la puerta de entrada, la ACL es lo que puedes hacer una vez dentro.** No puedes "arreglar" un RBAC restrictivo con una ACL permisiva.

## Roles RBAC de share (memorízalos)

| Rol | Permite |
|---|---|
| Storage File Data SMB Share **Reader** | Lectura |
| Storage File Data SMB Share **Contributor** | Lectura, escritura, borrado |
| Storage File Data SMB Share **Elevated Contributor** | Lo anterior **+ modificar las ACLs NTFS** |

El Elevated Contributor es el que necesitas para configurar las ACLs desde un cliente montado con identidad.

## Detalles que caen

- **Las cuentas de equipo (computer accounts) no se pueden usar con RBAC de share**, porque no tienen identidad sincronizada en Entra ID. Alternativas: default share-level permission o una cuenta de servicio.
- El **default share-level permission** concede el rol a *todas* las identidades autenticadas y sirve para escenarios donde no puedes sincronizar el AD DS on-prem con Entra. Se combina con ACLs NTFS para la granularidad real.
- Fuentes de identidad soportadas: **AD DS on-premises**, **Entra Domain Services** y **Entra Kerberos** (identidades híbridas).

## Palabras gancho

| Si ves… | Es… |
|---|---|
| "permisos distintos por carpeta o archivo" | Windows ACLs (NTFS), **no** RBAC |
| "controlar quién accede al share" | Azure RBAC con roles Storage File Data SMB |
| "modificar permisos NTFS desde el cliente" | Elevated Contributor |
| "no puedo sincronizar el AD on-prem con Entra" | Default share-level permission + ACLs |
| "computer account necesita acceso" | No es posible con RBAC → cuenta de servicio |

---

# FICHA 2 → AzureBastionSubnet como objetivo de NSG

## Lo primero: sí, admite NSG

Aquí hay una confusión muy extendida (y algún blog lo dice mal): **AzureBastionSubnet SÍ soporta NSG**. El que **no** admite NSG es **AzureFirewallSubnet**. No los mezcles.

Ahora bien, la advertencia de Microsoft es clara: **si asocias un NSG, tienes que configurar todas las reglas requeridas.** Omitir una sola impide que Bastion reciba actualizaciones de plataforma o rompe la conectividad con las VMs → y el fallo es silencioso, porque el despliegue puede tener éxito y Bastion simplemente no funcionar.

## Requisitos de la subnet

- Nombre **exactamente** `AzureBastionSubnet`. Ni `azureBastionSubnet` ni variantes.
- Tamaño mínimo **/26** (para recursos desplegados desde noviembre de 2021). Los desplegados antes con /27 siguen funcionando, pero /26 es lo requerido hoy y lo necesario para host scaling.
- **Sin route tables** y **sin delegaciones**.
- IP pública **Standard SKU con asignación estática** (salvo Bastion privado en Premium).

## Reglas NSG mínimas

**Entrada:**

| Origen | Puerto | Por qué |
|---|---|---|
| Internet (o IPs públicas concretas) | 443 | Conexión del usuario desde el portal |
| **GatewayManager** | 443 | Plano de control de Azure hacia el host |
| **AzureLoadBalancer** | 443 | Sondas de estado |
| **VirtualNetwork** | **8080, 5701** | Plano de datos entre instancias de Bastion |

**Salida:**

| Destino | Puerto | Por qué |
|---|---|---|
| **VirtualNetwork** | **3389, 22** | Bastion hacia las VMs de destino |
| **AzureCloud** | 443 | Dependencias de plataforma |
| **VirtualNetwork** | 8080, 5701 | Plano de datos entre instancias |
| Internet | 80 | Validación de certificados |

## La trampa central

**Los puertos 3389 y 22 NO se abren en entrada sobre AzureBastionSubnet.** El usuario llega por 443. El RDP/SSH lo origina Bastion **hacia** la VM, así que 3389 y 22 van en **salida**, con destino VirtualNetwork.

Si en una pregunta ves una regla de entrada de 3389 desde Internet sobre AzureBastionSubnet, es un distractor: eso es precisamente lo que Bastion existe para eliminar.

## Detalles de arquitectura que caen

- Un solo Bastion **sirve a VNets peered**. Cuenta hubs regionales, no VNets.
- El NSG de la **subnet de las VMs destino** también debe permitir entrada 3389/22 **desde el service tag VirtualNetwork** (por donde llega Bastion).
- Bastion **Premium** = grabación de sesión y despliegue privado (sin IP pública propia).
- Con Premium y despliegue privado, las reglas cambian: no hay entrada desde Internet.

---

# FICHA 3 → Dependabot y los tiers de GitHub ⚠️ CORRECCIÓN

## Lo que tenías anotado ya no es correcto

Tenías apuntado que **Dependabot en repos privados requiere GitHub Enterprise Cloud**. Según la documentación actual de GitHub, **eso ya no se sostiene**: Dependabot está habilitado por defecto en todos los repositorios **públicos** y **se puede habilitar en los privados** → sin depender de un tier concreto ni de GitHub Advanced Security.

Cambiar esta creencia importa, porque si en el examen aparece "¿qué necesitas para Dependabot en repos privados?", la respuesta ya no es "subir de tier".

## Lo que SÍ diferencia los tiers

La línea de pago no está en Dependabot, sino en las funciones de **Advanced Security**:

| Función | Repos públicos | Repos privados |
|---|---|---|
| **Dependabot alerts** | Gratis, por defecto | Gratis, hay que habilitarlo |
| **Dependabot security updates** | Gratis | Gratis |
| **Dependabot version updates** | Gratis | Gratis (vía `dependabot.yml`) |
| **Dependency graph** | Gratis | Gratis |
| **Code scanning (CodeQL)** | Gratis | **Requiere Advanced Security** |
| **Secret scanning** | Gratis | **Requiere Advanced Security** |

Regla mental: **Dependabot es gratis en todas partes. CodeQL y secret scanning en repos privados son de pago.**

Nota adicional: GitHub ha separado lo que antes era el paquete único GHAS en licencias independientes (protección de secretos por un lado, seguridad de código por otro). Si el examen aún habla de "GitHub Advanced Security" como un bloque único, respóndele en sus términos.

## Los tres tipos de análisis (esto no cambia y cae siempre)

| Análisis | Qué busca | ¿De quién es el código? |
|---|---|---|
| **SAST** (CodeQL) | Fallos en tu código fuente | **Tuyo** |
| **Secret Scanning** | Claves y tokens hardcodeados | Tuyo (un descuido) |
| **SCA / Dependabot** | Vulnerabilidades en librerías | **Ajeno** |

Mnemotécnica que ya tienes: *"¿de quién es el código defectuoso? Mío = SAST, secreto olvidado = Secret Scanning, ajeno = SCA."*

## Dónde encaja cada uno en el pipeline

| Fase | Control |
|---|---|
| Plan and develop | Threat modeling (STRIDE, data flow diagram) |
| Commit / PR | SAST, secret scanning, branch policies |
| Build and test | SCA / Dependabot, pruebas de seguridad |
| Go to production | Validación de configuración, IaC scanning |
| Operate | DAST, pen testing, RASP |

---
---

# 10 PREGUNTAS

---

**1.** Un share de Azure Files contiene las carpetas `\RRHH` y `\Finanzas`. Los usuarios de RRHH deben poder leer y escribir en `\RRHH` pero no ver `\Finanzas`. Se usa autenticación con AD DS.

¿Qué deberías recomendar?

- A. Asignar el rol Storage File Data SMB Share Contributor solo sobre la carpeta `\RRHH`
- B. Asignar el rol a nivel de share y configurar Windows ACLs en cada carpeta
- C. Crear un share independiente por departamento y asignar RBAC a cada uno
- D. Usar Conditional Access con condición de grupo sobre cada carpeta

*(Puede haber más de una técnicamente válida → elige la que refleja el modelo de permisos de Azure Files.)*

---

**2.** Un usuario tiene el rol Storage File Data SMB Share Reader sobre un share, y la ACL NTFS de un archivo concreto le concede permisos de lectura y escritura.

¿Qué puede hacer el usuario con ese archivo?

- A. Leer y escribir: la ACL más específica prevalece
- B. Solo leer: se aplica el permiso más restrictivo de las dos capas
- C. Nada: los permisos contradictorios deniegan el acceso
- D. Leer y escribir, pero solo montando con la clave de la cuenta

---

**3.** Necesitas que un administrador pueda modificar los permisos NTFS de directorios dentro de un share de Azure Files montándolo con su identidad.

¿Qué rol necesita?

- A. Storage File Data SMB Share Reader
- B. Storage File Data SMB Share Contributor
- C. Storage File Data SMB Share Elevated Contributor
- D. Storage Account Contributor

---

**4.** Tu organización no puede sincronizar su AD DS on-premises con Microsoft Entra ID, pero necesita usar autenticación basada en identidad en Azure Files con control granular por carpeta.

¿Qué deberías recomendar?

- A. Es imposible sin sincronización
- B. Un default share-level permission combinado con Windows ACLs
- C. SAS tokens por carpeta
- D. Migrar a Blob Storage con RBAC por container

---

**5.** Aplicas un NSG a AzureBastionSubnet. ¿Cuál de estas reglas de **entrada** NO es necesaria?

- A. Puerto 443 desde el service tag Internet
- B. Puerto 443 desde el service tag GatewayManager
- C. Puertos 3389 y 22 desde el service tag Internet
- D. Puertos 8080 y 5701 desde el service tag VirtualNetwork

---

**6.** ¿Cuál de estas afirmaciones sobre subredes reservadas es correcta?

- A. Ni AzureBastionSubnet ni AzureFirewallSubnet admiten NSG
- B. AzureBastionSubnet admite NSG; AzureFirewallSubnet no
- C. AzureFirewallSubnet admite NSG; AzureBastionSubnet no
- D. Ambas admiten NSG sin restricciones

---

**7.** Estás desplegando Azure Bastion en una VNet nueva. ¿Cuál es el tamaño mínimo de AzureBastionSubnet y qué otras restricciones aplican?

- A. /27, sin route tables ni delegaciones
- B. /26, sin route tables ni delegaciones
- C. /26, con una route table apuntando al Firewall del hub
- D. /24, con delegación al servicio Microsoft.Network/bastionHosts

---

**8.** Tras aplicar un NSG a AzureBastionSubnet, los usuarios se conectan al portal pero no consiguen abrir sesión RDP contra las VMs.

¿Qué regla falta con más probabilidad?

- A. Entrada 443 desde GatewayManager
- B. Salida 3389 y 22 hacia el service tag VirtualNetwork
- C. Entrada 3389 desde Internet
- D. Salida 443 hacia el service tag Internet

---

**9.** Tu equipo trabaja en repositorios **privados** en GitHub y necesita recibir alertas de vulnerabilidades en dependencias de terceros y pull requests automáticos que las actualicen.

¿Qué necesitas?

- A. Habilitar Dependabot en los repositorios; no requiere licencia adicional
- B. Actualizar a GitHub Enterprise Cloud
- C. Adquirir GitHub Advanced Security
- D. Desplegar Microsoft Defender for DevOps

---

**10.** Sobre esos mismos repositorios privados, ahora necesitas además **análisis estático del código propio con CodeQL** y **detección de secretos hardcodeados**.

¿Qué necesitas?

- A. Nada adicional: van incluidos como Dependabot
- B. Advanced Security (code scanning y secret scanning en repos privados)
- C. Solo habilitar el dependency graph
- D. Migrar los repos a públicos

---
---

# SOLUCIONARIO

---

**1 → B.** RBAC a nivel de share + Windows ACLs por carpeta.
🔑 **Azure RBAC no baja de share.** No existe forma de asignar un rol RBAC "sobre la carpeta `\RRHH`", así que A es imposible tal como está redactada.
C funciona técnicamente y a veces es la respuesta cuando el enunciado pide aislamiento total o cuotas separadas, pero **no es el modelo de permisos de Azure Files** y multiplica los shares innecesariamente. Cuando la pregunta es "permisos distintos dentro del mismo share", la respuesta es siempre RBAC + ACLs.
❌ D: Conditional Access controla el acceso a aplicaciones, no permisos de sistema de archivos.

**2 → B.** Solo leer.
🔑 **Se aplican ambas capas y gana la más restrictiva.** El share actúa de puerta: si ahí solo tiene lectura, la ACL no puede ampliarlo.
⚠️ Esto funciona igual en el sentido inverso: Contributor en el share + solo lectura en la ACL = solo lectura.

**3 → C.** Elevated Contributor.
Es el único de los tres roles SMB que añade la capacidad de **modificar las ACLs NTFS**.
❌ D: Storage Account Contributor es un rol del plano de gestión (gestiona la cuenta), no concede acceso al plano de datos SMB.

**4 → B.** Default share-level permission + Windows ACLs.
El default share-level permission concede el rol a todas las identidades autenticadas, esquivando el requisito de identidad híbrida sincronizada. La granularidad real la ponen después las ACLs.
🔑 Palabra gancho: **"no puedo sincronizar el AD DS con Entra"**.

**5 → C.** Puertos 3389 y 22 desde Internet.
🔑 **La trampa central del bloque.** El usuario llega por **443**. El 3389/22 lo origina Bastion **hacia** la VM, así que va en **salida** con destino VirtualNetwork, nunca en entrada desde Internet.
Las otras tres sí son necesarias: 443 desde Internet (usuario), 443 desde GatewayManager (plano de control) y 8080/5701 desde VirtualNetwork (plano de datos entre instancias).

**6 → B.** Bastion sí, Firewall no.
AzureBastionSubnet **soporta NSG** → de hecho Microsoft lo recomienda para aplicar mínimo privilegio, siempre que configures **todas** las reglas requeridas. AzureFirewallSubnet **no admite NSG**, porque el propio Firewall gestiona toda la política de tráfico.

**7 → B.** /26, sin route tables ni delegaciones.
Desde noviembre de 2021 el mínimo es **/26**. Los desplegados antes con /27 siguen funcionando pero no escalan.
❌ C: las route tables en AzureBastionSubnet están explícitamente desaconsejadas y rompen el servicio.
⚠️ Recuerda también: nombre exacto `AzureBastionSubnet` e IP pública Standard estática.

**8 → B.** Falta la salida 3389/22 hacia VirtualNetwork.
El síntoma lo dice todo: **el portal carga** (luego la entrada 443 funciona) pero **la sesión no abre** (luego falla el tramo Bastion → VM).
🔑 Aprende a leer el síntoma: si falla la conexión al portal, mira las reglas de entrada; si falla la sesión contra la VM, mira las de salida → y también el NSG de la subnet de la VM, que debe permitir 3389/22 desde el service tag VirtualNetwork.

**9 → A.** Habilitar Dependabot; sin licencia adicional.
⚠️ **Esta es la corrección respecto a lo que tenías anotado.** Dependabot alerts, security updates y version updates están disponibles en repos privados sin requerir un tier concreto ni Advanced Security. En públicos viene activado por defecto; en privados hay que activarlo.
❌ D: Defender for DevOps agrega hallazgos de seguridad en Defender for Cloud, pero no es lo que habilita Dependabot.

**10 → B.** Advanced Security.
🔑 **Aquí sí está la línea de pago.** Code scanning con CodeQL y secret scanning **en repos privados** requieren Advanced Security; en repos públicos son gratis.
Contraste con la 9: **Dependabot gratis en todas partes; CodeQL y secret scanning de pago en privados.**

---

## RESUMEN EN TRES LÍNEAS

1. **Azure Files:** RBAC llega hasta el share; para carpeta o archivo, ACLs NTFS. Gana el más restrictivo.
2. **AzureBastionSubnet:** admite NSG (Firewall no), /26 mínimo, y 3389/22 van en **salida**, no en entrada.
3. **GitHub:** Dependabot es gratis también en repos privados; lo de pago en privados es CodeQL y secret scanning.

Si aciertas las 10, los tres gaps están cerrados y no queda nada abierto de cara al 29.
