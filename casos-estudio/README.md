# Casos de estudio

Escenarios largos de elaboración propia, con el formato de las *case study* del examen: contexto extenso, requisitos numerados, restricciones y preguntas que se responden **releyendo los requisitos**.

| Archivo | Escenario |
|---|---|
| [`caso-fabrikam.md`](./caso-fabrikam.md) | Fabricante multinacional: identidad híbrida, multi-cloud, ransomware |
| [`caso-northwind.md`](./caso-northwind.md) | Distribuidora: APIM, managed identities, Purview, backup |
| [`caso-wingtip.md`](./caso-wingtip.md) | Financiera con M&A: multi-tenant, landing zones, residencia de datos |

## El método para las case study

Casi todas las respuestas las decide **una cláusula concreta del enunciado**. Antes de mirar las opciones, localiza la palabra limitante:

- *"sin cuentas de invitado"* → Azure Lighthouse / B2B Direct Connect
- *"on-premises"* → Azure Arc, o Defender for Identity si es un ataque
- *"por región / residencia de datos"* → segmentar, nunca centralizar
- *"minimizar el esfuerzo administrativo"* → descarta la opción que exige configuración manual repetida
- *"secure score"* → Defender for Cloud

Cada caso termina con una tabla que mapea cada pregunta con la restricción que la resuelve. Ese es el músculo que se entrena.
