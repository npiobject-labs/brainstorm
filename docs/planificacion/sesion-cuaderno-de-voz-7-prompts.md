# Sesión de Brainstorm en 7 diálogos — prompt de "Cuaderno de voz"

Los siete prompts que el usuario escribe, uno por diálogo, para llegar desde la
idea en bruto hasta el prompt de desarrollo de **Cuaderno de voz**. Copiables
tal cual en una sesión de claude.ai/code con este repo seleccionado, mientras
la app de Brainstorm no exista; las convenciones (prefijos de rol, propuestas
para la mesa, prompt en tres partes) son las de `plan-brainstorm.md`.

`[SUPUESTO]` Cuaderno de voz es la app de ejemplo: dictar una nota desde el
móvil y encontrarla escrita en el repositorio, sin encender el PC. Plan B: si
el caso cambia, solo se reescribe el diálogo 1; los otros seis mantienen la
estructura.

Cada diálogo lleva **lo que escribes**, **lo que tiene que devolver Claude** y
**lo que queda en la mesa** al terminar.

---

## Diálogo 1 · Abrir la sesión

```
Abrimos sesión de brainstorm para una app nueva: Cuaderno de voz. Dicto una
nota desde el móvil y aparece escrita en el repositorio del proyecto del que
hablo, sin encender el PC. El entregable de esta sesión no es la app: es el
prompt de desarrollo para Fable 5.1, en tres partes, siguiendo el método
Cherny de docs/planificacion/referencias/.

Trabajamos con las reglas de plan-brainstorm.md: eres participante, no
secretario; tu aportación cubre lo que yo no cubro; al final de cada respuesta
me propones fichas para la mesa con tipo, y no escribes ninguna sin que yo la
acepte. Mantén la mesa visible al final de cada respuesta: fichas por tipo,
dudas abiertas y qué lentes se han usado.

Empieza: qué falta en la mesa antes de poder generar nada.
```

**Devuelve**: la mesa vacía, tres o cuatro huecos nombrados, y un bloque
`Propuestas para la mesa` con la idea, los primeros límites y las dudas.

**Mesa**: las fichas que aceptes de esa primera tanda. Lentes: ninguna.

---

## Diálogo 2 · Que te lo tumben

```
crítico, escéptico: la idea tal como está en la mesa.

Un bloque por rol, sin mezclarlos. Del crítico quiero objeciones numeradas,
cada una diciendo a qué ficha ataca. Del escéptico, la razón más fuerte para
no construir esto y qué tendría que ser cierto para desactivarla.
```

**Devuelve**: dos bloques etiquetados. El escéptico tiene que llegar a que el
teclado del móvil ya dicta, y a qué hace la app que el teclado no hace.

**Mesa**: las objeciones que aceptes, y un `criterio` con lo que desactiva al
escéptico. Lentes: `crítico`, `escéptico`.

---

## Diálogo 3 · Descomponer y bajar al uso real

```
analítico, usuario: cómo se descompone esto y cómo se usa de verdad.

Del analítico quiero la tabla de piezas con qué depende de qué, y cuál es la
pieza que decide si la app vale. Del usuario, un escenario en primera persona
un día cualquiera, y qué me haría dejar de usarla a la semana.
```

**Devuelve**: la tabla de piezas (captura, transcripción, detección del
proyecto, escritura) y el escenario con la fricción que mata la app.

**Mesa**: una `funcionalidad` por pieza y el `límite` que sale de la fricción.
Lentes: más `analítico` y `usuario`.

---

## Diálogo 4 · Decidir, y cerrar las dudas

```
Decido, y esto va a la mesa como decisiones:

1. La transcripción es diferida: la nota se ve como pendiente hasta que está.
2. El audio se descarta en cuanto hay transcripción. No se guarda nunca.
3. Si el proyecto no se detecta con certeza, la nota va a un cajón de sin
   clasificar. Nunca al proyecto equivocado.
4. La nota queda en Markdown, con fecha y proyecto en el encabezado. El
   formato no lo decide el modelo.
5. Dictar y soltar: cero confirmaciones en el móvil.

Actualiza la mesa, cierra las dudas que estas decisiones resuelven y dime
cuáles siguen abiertas.
```

**Devuelve**: cinco fichas `decisión` o `límite`, las dudas tachadas y la lista
de las que quedan.

**Mesa**: sin dudas abiertas, o con las que queden nombradas una a una.

---

## Diálogo 5 · Auditar antes de generar

```
auditor: la mesa entera contra el método Cherny, antes de generar nada.

Hallazgos con severidad alta, media o baja, cada uno con su evidencia: qué
ficha lo provoca o qué ficha falta. Después dime qué criterios de salida
ejecutables tengo y cuáles me faltan, sabiendo que la app todavía no existe:
comandos que tendrá que pasar, test de extremo a extremo con nombre, y guion
de prueba en el navegador.
```

**Devuelve**: la tabla de hallazgos y la lista de criterios, separando los que
ya son ejecutables de los que todavía son opinables.

**Mesa**: los hallazgos altos y medios aceptados, y los `criterio` ejecutables.
Lentes: las seis.

---

## Diálogo 6 · Generar v1

```
Genera el prompt, versión 1.

Tres partes: Tarea en una o dos frases y como resultado, nunca como pasos;
Límites con las fichas de límite aceptadas; Terminado cuando con los criterios
ejecutables. Después de una línea de corte, los anexos recortables: contexto,
pantallas y fuera de alcance.

Antes de dármelo, pásale el linter y enséñame el resultado: si la Tarea lleva
verbos de paso, si Límites está vacío, si hay algún criterio ejecutable y si
queda alguna duda abierta. Si no pasa, no lo llames versión: dámelo como
borrador con el aviso.
```

**Devuelve**: el prompt en un bloque copiable, el informe del linter y la
cobertura de lentes.

**Mesa**: v1, o un borrador si el linter la para.

---

## Diálogo 7 · Descartar con motivo y cerrar

```
Descarto la v1. El motivo va a la mesa como criterio, con mis palabras, y la
v2 tiene que nacer de él.

Motivo: <lo que falle cuando lo leas>.

Genera la v2, enséñame el diff contra la v1 en texto y en fichas, y si la
acepto cierra la sesión: resumen en docs/planificacion/sesiones/, entrada nueva
en docs/bitacora/, y el paquete para Navaja (idea, límites con sus claves,
criterios de salida y contexto) en un bloque que pueda pegar en su fase Idea.
```

**Devuelve**: la ficha `criterio` nueva, la v2, el diff y el paquete.

**Mesa**: v1 descartada con motivo, v2 aceptada, sesión en estado `prompt
entregado`.

---

## Lo que tiene que salir al final

Forma esperada del entregable, para reconocer si la sesión ha ido bien. El
texto exacto lo decide la mesa, no este documento.

```
Tarea: una app que recibe una nota dictada desde el móvil y deja su
transcripción en Markdown en el proyecto correcto del repositorio, sin
intervención.
Límites: nada del flujo requiere el PC encendido; el audio se descarta en
cuanto hay transcripción; si el proyecto no se detecta con certeza la nota va
a sin clasificar y nunca al proyecto equivocado; dictar y soltar, sin
confirmaciones; el formato de salida lo fija el prompt, no el modelo; ninguna
clave en docs/; no tocar /salud, /holamundo ni los workflows.
Terminado cuando: `cargo test` en verde con el test de audio a fichero;
`notas.spec.ts` pasa de extremo a extremo; el guion de cinco pasos en el móvil
se ejecuta entero sin tocar el PC.

--- Anexos ---
Contexto: <stack, dónde corre la transcripción, cómo se listan los proyectos>
Pantallas: <dictar, pendientes, sin clasificar>
Fuera de alcance: <multiusuario, edición de la nota, búsqueda>
```

## Si la sesión se alarga

Los siete diálogos son el camino corto. Si un diálogo abre más de lo que
cierra, el que sigue no es el número siguiente: es repetir el 4, decidir, hasta
que la mesa no tenga dudas abiertas. Generar con dudas abiertas produce
versiones que se descartan por lo mismo dos veces.
