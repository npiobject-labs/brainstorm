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
- **Encargo de diseño**: en la planificación se espera creatividad de Claude en
  el planteamiento de la app, no una transcripción literal de los requisitos.

## Dudas abiertas

- ~~¿Claude participante o solo registro?~~ Resuelto en el prompt 3:
  participante.
- ¿Cómo se mide o se fuerza la "complementariedad"? Opciones a explorar en la
  planificación: roles asignados a Claude, turnos ciegos antes de ver lo del
  otro, detección de solapamiento entre ideas.
