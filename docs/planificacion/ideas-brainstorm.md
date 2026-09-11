# Brainstorm — recogida de ideas (en curso)

Bitácora literal de lo que va pidiendo el usuario, prompt a prompt.
No se planifica nada hasta que el usuario diga explícitamente
"con toda esta información, genérame una planificación del proyecto".

Regla de trabajo acordada: varios prompts sucesivos de entrada; Claude solo
almacena y ordena, sin proponer arquitectura ni empezar a implementar.

## Estado consolidado a 2026-09-10

Qué es la app, en una frase: una herramienta para conducir sesiones de
brainstorming entre el usuario y Claude cuyo entregable es un prompt de
desarrollo para Claude Fable 5.1, refinado por versiones hasta dar con el que
convence.

Piezas confirmadas hasta ahora:

1. **Sesión de brainstorming en equipo**, con el caso real de dos
   participantes: el usuario y Claude.
2. **Claude participa**, no transcribe. Aporta ideas propias y saca dudas y
   funcionalidades a la mesa por iniciativa suya.
3. **Complementariedad obligatoria**: lo que aporta Claude debe cubrir lo que
   el usuario no cubre. El sistema necesita saber qué hay ya sobre la mesa.
4. **Persistencia entre días**: una sesión se retoma pasados días sin perder
   contexto.
5. **Generación bajo demanda**: en cualquier momento, con lo recogido hasta
   ese punto, se pide un prompt. La app produce el prompt, nunca la app.
6. **Iteración versionada**: varias generaciones por sesión, cada una guardada
   como versión, con el motivo de descarte anotado cuando el usuario la
   rechaza.

Hueco principal pendiente: los criterios de "lo que busco", que definen cuándo
un prompt es bueno. El usuario los aportará en sesiones siguientes.

Nada de esto está planificado ni implementado todavía, por decisión expresa
del usuario. La planificación arranca cuando él la pida.

## Prompt 1 — 2026-09-10

> Vamos a crear la aplicación de Brainstorm. Yo te voy a ir diciendo cosas, y
> tú solamente lo que tienes que hacer es, de momento, ir almacenándolas. Las
> guardas y te voy a enviar varios prompts, no sé cuántos, hasta que en un
> momento determinado te diga, con toda esta información, genérame una
> planificación del proyecto.

Contenido de producto: ninguno todavía. Solo el método de trabajo.

## Prompt 2 — 2026-09-10

> La app es para gestionar sesiones de brainstorming en equipo, aunque
> generalmente seremos tus y yo.

## Prompt 3 — 2026-09-10

> Sí, tú y yo, participando en la sesión. es importante que tu aportación sea
> complementaria a lo que yo pueda aportar. Y también es importante que con tu
> amplia experiencia seas creativo en el planteamiento de esta APP.

## Prompt 4 — 2026-09-10

> Las sesiones deben poder retomarse días después sin perder contexto. Y
> también es importante lo que me has contado referente a que todas las dudas
> que tengas me las plantees así como sugerencias oh funcionalidades que puedan
> ser interesantes. Como te puedes imaginar el objetivo es generar el mejor
> promt posible para el desarrollo de una determinada app

## Prompt 5 — 2026-09-10

> La idea es que en cualquier momento, con toda la información recolectada
> hasta ese punto, hasta ese momento, te pueda pedir generar un prompt, no una
> APP, sino un prompt para desarrollar esa APP en fable. Evidentemente te puedo
> pedir generar varios promts en distintos momentos y puede ser que ninguno me
> convenza y sigamos con el brainstorm para refinar el prompt

## Prompt 6 — 2026-09-10

> Sí, Fable 5.1, y cuando descarte un prompt te diré por qué se ha descartado y
> lo vas anotando en distintas versiones como propones eso es una buena idea
> recolectar los distintos proms en versiones es un proceso iterativo hasta
> acercarse al promt que más se acerque a lo que voy buscando y esto que voy
> buscando te lo pasaré en siguientes conversaciones

## Prompt 7 — 2026-09-10

> De momento, hoy dejamos aquí la información. Mañana continuaré hablándote
> sobre esta aplicación de Brainstorm. ahora lo que puedes hacer es un resumen
> de toda la información que tienes hasta el momento y almacenarla de forma
> segura

Cierre de sesión. Continúa mañana.

## Prompt 8 — 2026-09-10

> Mañana te paso el id correcto de Drive

Pendiente para la próxima sesión: id de la carpeta de Drive. Hasta entonces las
copias de `docs/planificacion/` no se suben; el repo sigue siendo la única
fuente de verdad, así que no se pierde nada.

## Prompt 9 — 2026-09-11

> [captura de Drive] puedes obtener de ahí el ID he creado la carpeta nueva

Carpeta `Mi unidad > brainstotm`, id `1FithJGx3HLgPd9H-1VEgwxEfe90Yn1q6`.
Verificada con acceso de escritura y anotada en `CLAUDE.md`.

## Requisitos recogidos

- **Propósito**: gestionar sesiones de brainstorming en equipo.
- **Escala real de uso**: equipo pequeño; el caso habitual son dos
  participantes, el usuario y Claude. El multiusuario existe, pero el caso
  típico es de dos.
- **Claude es participante, no secretario**: aporta ideas dentro de la sesión,
  no solo la registra ni la modera.
- **Complementariedad**: la aportación de Claude debe cubrir ángulos que el
  usuario no cubre, no repetir ni reformular lo suyo. Implica que el sistema
  tiene que saber qué ha aportado ya el usuario para diferenciarse de ello.
- **Objetivo final de una sesión**: producir el mejor prompt posible para el
  desarrollo de una app concreta. El entregable de la sesión no son las ideas
  sueltas, es un prompt de desarrollo.
- **Generación bajo demanda**: en cualquier momento, con lo recogido hasta ese
  instante, el usuario pide "genera el prompt". La app nunca genera la app en
  sí, solo el prompt que la desarrollará.
- **Destino del prompt**: Claude Fable 5.1 (confirmado en el prompt 6). El
  prompt se redacta para que lo consuma ese modelo.
- **Varias generaciones por sesión**: se pueden pedir N prompts en momentos
  distintos. Ninguno tiene por qué convencer; el brainstorm continúa después
  de una generación para refinar. Implica versionado de prompts generados,
  conservar los descartados y poder comparar entre versiones.
- **Versionado con motivo de descarte** (confirmado): cada prompt generado se
  guarda como versión. Al descartar uno, el usuario dice por qué, y ese motivo
  queda anotado junto a la versión. El proceso es iterativo: cada versión
  arranca de lo aprendido en las anteriores y de sus motivos de rechazo.
- **Continuidad**: una sesión se puede retomar días después sin perder
  contexto. Implica persistencia del estado de la sesión y una reconstrucción
  del contexto que quepa en la ventana del modelo.
- **Claude plantea dudas y sugerencias**: durante la sesión, no solo responde.
  Saca a la mesa dudas abiertas y funcionalidades que podrían ser interesantes,
  como comportamiento del producto (no solo como estilo de conversación).
- **Encargo de diseño**: en la planificación se espera creatividad de Claude en
  el planteamiento de la app, no una transcripción literal de los requisitos.

## Dudas abiertas

- ~~¿Claude participante o solo registro?~~ Resuelto en el prompt 3:
  participante.
- ¿Cómo se mide o se fuerza la "complementariedad"? Opciones a explorar en la
  planificación: roles asignados a Claude, turnos ciegos antes de ver lo del
  otro, detección de solapamiento entre ideas.
- Retomar días después: ¿se recarga la transcripción entera o un resumen
  destilado? Con sesiones largas la transcripción íntegra no cabrá.
- ¿El prompt generado es un texto único o un documento estructurado por
  secciones (objetivo, alcance, stack, pantallas, criterios de aceptación)?
- ¿Una sesión equivale a una app, o varias sesiones pueden converger en el
  mismo prompt?
- ~~¿Se anota el motivo de descarte? ¿Se versionan los prompts?~~ Resuelto en
  el prompt 6: sí a las dos.
- Pendiente del usuario: los criterios de "lo que voy buscando", es decir,
  contra qué se juzga si un prompt se acerca o no. Sin eso no hay criterio de
  aceptación del entregable.
