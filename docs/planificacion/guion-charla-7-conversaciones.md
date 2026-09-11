# Guion de una charla en Brainstorm — 7 conversaciones

Guion de demostración de una sesión completa de Brainstorm entre el usuario y
Claude, en siete intercambios. No es una transcripción real: es el **guion de
prueba en navegador** que el plan pide como criterio de salida de tipo 2
(`plan-brainstorm.md`, 2.3.1), y a la vez la comprobación de que las nueve
propuestas de la sección 8 se sostienen cuando se usan de verdad.

`[SUPUESTO]` La app objetivo del ejemplo es **Cuaderno de voz** (dictar notas
desde el móvil y que acaben transcritas en un repo). Se elige por ser un caso
con límites y criterios verificables reales. Plan B: sustituir el caso por
cualquier otro sin tocar la estructura de las siete conversaciones.

Convenios del guion: **U** es el usuario, **C** es Claude. Tras cada
conversación se anota el estado de la mesa, que es lo que el mock enseña en la
pestaña **Mesa**. Cada conversación declara **qué demuestra**, para que al
ejecutarlo se sepa qué se está mirando.

---

## 1 · Abrir la mesa

**Demuestra**: la mesa como estado, las propuestas de ficha y que Claude nunca
escribe sin confirmación.

> **U** — Quiero una app para dictar notas de voz desde el móvil y que acaben
> escritas en un repositorio, sin abrir el portátil.

> **C** — La mesa está vacía, así que empiezo por lo que hay que dejar fijo
> antes de nada. Dos cosas me chirrían ya: "sin abrir el portátil" es un
> límite, no una funcionalidad, y "acaben escritas" no dice quién transcribe
> ni cuándo.
>
> **Propuestas para la mesa**
>
> | # | tipo | texto |
> |---|---|---|
> | 1 | idea | Dictar una nota desde el móvil y encontrarla escrita en el repo |
> | 2 | límite | Nada del flujo requiere el PC encendido |
> | 3 | duda | ¿La transcripción es inmediata o diferida? |
> | 4 | duda | ¿La nota original en audio se conserva o se descarta tras transcribir? |

> **U** — A la mesa la 1, la 2 y la 3. La 4 la reformulo yo: el audio se
> descarta siempre, eso no es duda, es límite.

**Mesa**: 3 fichas aceptadas (1 `idea`, 2 `límite`, 1 `duda` abierta) y 1
propuesta descartada. Cobertura de lentes: ninguna todavía.

---

## 2 · Dos roles a la vez

**Demuestra**: prefijo de rol encadenado, un bloque por rol e **independencia
forzada**: cada bloque sale de una llamada distinta con la misma entrada, así
que el segundo no puede alinearse con el primero.

> **U** — crítico, escéptico: la idea de arriba.

> **C** —
>
> **`crítico:`**
> 1. "Sin PC encendido" ataca a la ficha 2, pero la transcripción tiene que
>    correr en algún sitio: o es un servicio de pago o es una máquina tuya, y
>    entonces el límite es falso.
> 2. La ficha 1 no dice *dónde* en el repo. Una nota sin ruta es una nota que
>    no se encuentra a los tres días.
> 3. No hay ninguna ficha de tipo `criterio`. Tal como está, cualquier prompt
>    que salga de esta mesa no pasa el linter.
>
> **`escéptico:`**
> La razón más fuerte para no construirlo: el móvil ya dicta a texto en el
> teclado del sistema, y un `git commit` desde una app de notas resuelve el
> 80 % sin escribir una línea. Lo que desactivaría esta objeción es que la
> nota necesite algo que el teclado no hace: clasificarla, enlazarla con lo
> que ya existe en el repo o dispararla sin desbloquear la pantalla.

> **U** — A la mesa las tres objeciones del crítico. Y del escéptico, a la
> mesa lo que lo desactiva, como `criterio`.

**Mesa**: 7 fichas. Aparece la primera `criterio` ("la nota tiene que hacer
algo que el teclado del móvil no hace"). Cobertura: `crítico`, `escéptico`.

---

## 3 · Claude invoca un rol por su cuenta

**Demuestra**: iniciativa propia con marca visible y regla mecánica de disparo
(un tipo de ficha lleva N mensajes vacío).

> **U** — Lo que el teclado no hace es enlazar. Quiero que la nota se pegue
> sola al proyecto del que habla.

> **C** — *Claude invoca `analítico:` porque la mesa no tiene ninguna ficha de
> tipo `funcionalidad` en tres mensajes.*
>
> **`analítico:`** Piezas y dependencias de "se pega sola al proyecto":
>
> | Pieza | Depende de |
> |---|---|
> | Captura de audio en el móvil | Nada |
> | Transcripción | Captura |
> | Detección del proyecto | Transcripción + lista de proyectos del repo |
> | Escritura del fichero | Detección + permiso de escritura |
>
> La detección es la única pieza que puede fallar de forma silenciosa: si
> acierta el 70 % de las veces, cada nota hay que revisarla, y entonces el
> ahorro desaparece. Es la pieza que decide si la app vale.
>
> **Propuestas para la mesa**: 4 `funcionalidad`, una por pieza, y 1 `criterio`
> sobre el acierto de la detección.

> **U** — Todas a la mesa. El criterio lo fijo yo: si no acierta el proyecto,
> la nota va a un cajón de sin clasificar, nunca al proyecto equivocado.

**Mesa**: 12 fichas, 1 `duda` abierta (la 3). Cobertura: `crítico`,
`escéptico`, `analítico`.

---

## 4 · Primera generación, y el linter la para

**Demuestra**: que un prompt que no pasa el linter **no se guarda como
versión**, sino como borrador con el aviso.

> **U** — Genera el prompt.

> **C** — No sale como versión. El linter devuelve dos fallos y un aviso:
>
> - **Duda abierta sin resolver**: "¿la transcripción es inmediata o
>   diferida?". O la resuelves o entra como límite "no decidir el momento de
>   la transcripción".
> - **Terminado cuando no ejecutable**: lo único que tengo de la mesa es "que
>   la detección acierte", que no es un comando.
> - **Cobertura**: faltan las lentes `creativo` y `usuario`. Nadie ha puesto
>   alternativas sobre la mesa y nadie ha contado cómo se usa esto un martes
>   a las ocho de la mañana.
>
> Queda guardado como **borrador**, no como v1. Puedes forzar la generación y
> las dudas entrarán como límites explícitos.

> **U** — No fuerces. Lo resolvemos.

**Mesa**: 12 fichas. Versiones: 0. Un borrador con su informe de linter.

---

## 5 · Cerrar las dudas con las lentes que faltaban

**Demuestra**: las lentes `usuario` y `creativo` con su formato de salida
obligatorio, y las fichas de tipo `decisión` cerrando dudas.

> **U** — usuario, creativo: cómo se usa esto de verdad.

> **C** —
>
> **`usuario:`** Son las ocho y diez, voy conduciendo. Digo "nota para
> Brainstorm: el linter tendría que avisar antes de generar, no después".
> Llego, abro el repo y está ahí, en el proyecto correcto, con la fecha. Lo
> que me frustraría: tener que abrir la app, elegir el proyecto y confirmar.
> Si hay tres toques, dicto en el bloc de notas y me olvido de esto.
>
> **`creativo:`** Tres alternativas divergentes, sin evaluar:
> 1. La nota no va a un fichero, va a la bitácora que ya existe.
> 2. No hay app: un número de teléfono al que dejas un mensaje de voz.
> 3. La nota entra como issue del repo, y el proyecto lo decide una etiqueta
>    que dices al principio.

> **U** — La 3 no, la 1 sí. Y decido la duda: transcripción diferida, con la
> nota visible como pendiente mientras tanto.

> **C** — **Propuestas para la mesa**: 1 `decisión` (transcripción diferida,
> cierra la duda 3), 1 `decisión` (destino, la bitácora), 1 `límite` (cero
> toques: dictar y soltar), y 3 `criterio` ejecutables:
>
> - `cargo test` en verde con un test de "audio entra, fichero sale".
> - Un test de extremo a extremo llamado `notas.spec.ts`: dicta, espera,
>   recarga y la nota está en el proyecto correcto.
> - Guion de navegador de cinco pasos, ejecutable en cinco minutos.

> **U** — Todas a la mesa.

**Mesa**: 19 fichas, 0 dudas abiertas. Cobertura: las seis lentes menos
`auditor`.

---

## 6 · v1, descarte con motivo, y el motivo se vuelve criterio

**Demuestra**: el corazón del producto. Una versión descartada enseña algo a la
siguiente sin que nadie tenga que acordarse.

> **U** — Ahora sí, genera.

> **C** — **v1** generada, linter en verde. Aviso de cobertura: la lente
> `auditor` no se usó.
>
> ```
> Tarea: una app que recibe una nota dictada desde el móvil y deja su
> transcripción en el proyecto correcto del repo, sin intervención.
> Límites: nada del flujo requiere el PC encendido; el audio se descarta tras
> transcribir; si el proyecto no se detecta con certeza, la nota va a sin
> clasificar y nunca al proyecto equivocado; dictar y soltar, sin
> confirmaciones.
> Terminado cuando: `cargo test` en verde con el test de audio a fichero;
> `notas.spec.ts` pasa; el guion de cinco pasos se ejecuta en el móvil.
> ```
>
> Anexos recortables: contexto, pantallas, fuera de alcance.

> **U** — La descarto. Motivo: el prompt no dice en qué formato queda la nota,
> y quien lo lea se inventará un JSON. Un prompt bueno no deja el formato de
> salida al criterio del modelo.

> **C** — Descartada. El motivo entra en la mesa como ficha `criterio`: *un
> prompt bueno no deja el formato de salida al criterio del modelo*. Esa ficha
> ya está en la foto de la próxima generación.

**Mesa**: 20 fichas. Versiones: v1 `descartada` con motivo.

---

## 7 · Nueve días después: destilado, v2 y testigo

**Demuestra**: memoria en tres capas al retomar, el diff entre versiones y el
cierre de la sesión con el paquete para Navaja.

> **C** — *Han pasado 9 días. Esto es lo que hay.* Destilado de 18 líneas:
> qué se decidió (transcripción diferida, destino la bitácora, cajón de sin
> clasificar), qué quedó abierto (ninguna duda; falta la lente `auditor`) y
> por qué se descartó la v1.

> **U** — auditor: la mesa entera antes de generar.

> **C** — **`auditor:`** Hallazgos frente al método:
>
> | Severidad | Hallazgo | Evidencia |
> |---|---|---|
> | alta | Ningún límite prohíbe tocar `/salud` ni `deploy.yml` | ausencia de ficha `límite` sobre CI |
> | media | El criterio del acierto de detección no tiene número | ficha 13, "con certeza" |
> | baja | Las pantallas del anexo no están en la mesa | anexo de v1 |

> **U** — El alto y el medio a la mesa. Genera v2.

> **C** — **v2**, linter en verde, cobertura completa. Diff contra v1: dos
> límites nuevos, el formato de salida fijado en Markdown con encabezado de
> fecha y proyecto, y el umbral de la detección como número.

> **U** — Aceptada.

> **C** — Sesión en estado `prompt entregado`. El paquete está en
> `GET /sesiones/:id/paquete`: idea, límites con sus claves, criterios de
> salida y contexto, listo para la fase Idea de Navaja. La sesión se puede
> reabrir.

**Mesa final**: 22 fichas, 0 dudas abiertas, 6 lentes de 6. Versiones: v1
descartada con motivo, v2 aceptada.

---

## Qué valida este guion

| Propuesta de la sección 8 del plan | Dónde se ve |
|---|---|
| 1 · Seis roles con formato de salida | 2, 3, 5, 7 |
| 2 · Independencia forzada de los bloques | 2 |
| 3 · Prompt de tres partes con anexos | 6 |
| 4 · Criterios de salida para una app nueva | 5 |
| 5 · Memoria en tres capas | 7 |
| 6 · Testigo a Navaja | 7 |
| 7 · Ablación de los prompts de rol | ninguna conversación necesitó instrucción extra en un rol |
| 8 · Persistencia | 7, al retomar nueve días después |
| 9 · F1 con el prompt de la sección 7 | 1 y 3: mesa y fichas es lo primero que se usa |

Lo que este guion deja al descubierto, y que el plan no cubría: en la
conversación 4 el usuario descubre el fallo del linter **después** de pedir la
generación. La propia ficha dictada en la conversación 5 lo dice ("el linter
tendría que avisar antes de generar"). Candidato a ficha de la sesión real:
mostrar en la pestaña Mesa, de forma permanente, qué le falta a la mesa para
que una generación pase el linter.
