# Brainstorm — recogida de ideas (en curso)

Bitácora literal de lo que va pidiendo el usuario, prompt a prompt.
No se planifica nada hasta que el usuario diga explícitamente
"con toda esta información, genérame una planificación del proyecto".

Regla de trabajo acordada: varios prompts sucesivos de entrada; Claude solo
almacena y ordena, sin proponer arquitectura ni empezar a implementar.

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
