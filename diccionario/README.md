# Diccionario de conceptos y trampas

El núcleo del método. No son definiciones: es un mapa de **qué palabra del enunciado activa qué servicio**, y dónde está la trampa entre dos opciones que suenan igual.

| Archivo | Contenido |
|---|---|
| [`01-diccionario.md`](./01-diccionario.md) | Diccionario único por bloques temáticos + chuleta de palabras gancho. Las entradas marcadas con ⚠️ corrigen o matizan una versión anterior |

## Cómo usarlo

1. **Léelo entero una vez** antes de empezar con preguntas, para tener el mapa.
2. Después úsalo solo como **referencia de corrección**: cada vez que falles una pregunta, busca el bloque y añade la trampa con tus palabras.
3. La última semana, **una relectura completa** y nada más.

## La regla de oro

Ante dos servicios que suenan parecido, pregúntate:

1. ¿El tráfico **entra o sale**? (VNet Integration = out, Private Endpoint = in)
2. ¿La clave **sale de Azure o se queda**? (BYOK entra, HYOK nunca sale)
3. ¿Protege **en reposo, en tránsito, o contra quién administra**?
4. ¿Es ataque **cloud u on-premises**?
5. ¿Es la solución **mínima suficiente** o la más exagerada? El examen premia la mínima que cumple el requisito exacto.
