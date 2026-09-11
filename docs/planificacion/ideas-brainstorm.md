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

## Prompt 10 — 2026-09-11

> los criterios que hacen bueno a un prompt, los tengo ya desarrollados en otra
> app que ya tienes. La app se llama Boris Cherny. lo que viene a decir es que
> un prompt consta de tres partes fundamentales, que son el objetivo, el
> guardarailes y el stop, pero todo eso te lo voy a dar de cierta forma más
> detallada más adelante. te adjunto un MOCK de la aplicación BorisCherny

Adjunta una captura del mock de esa app (no se guarda en el repo: `docs/` es
público y la captura muestra una ruta local del PC).

**Anatomía del prompt, según el método**: tres partes fundamentales, objetivo,
guardarraíles y stop. El detalle llega más adelante.

Lo que se lee en el mock, como referencia de vocabulario y de método:

- La app se llama **Navaja**, con el subtítulo "método Cherny". La pantalla
  mostrada es el **Asistente de prompt**: "de la idea al prompt", siete fases
  guiadas, en cada una se propone un borrador y se hacen las preguntas del
  método; el usuario confirma o corrige.
- Las siete fases: **Idea, Tarea, Límites, Verificación, Contexto,
  Herramientas, Resultado**.
- El "cómo funciona" que describe el propio mock: escribes la idea; se propone
  la tarea **como resultado, nunca como pasos**; eliges los límites, o sea qué
  no puede tocar; eliges cómo se verifica (tests, lint, prueba como usuario en
  navegador); se propone el **CLAUDE.md mínimo**; se recomiendan solo las
  herramientas que pasan la **regla de Cherny**; y obtienes prompt, CLAUDE.md,
  herramientas y comando de arranque.
- El formulario de la fase Idea pide "¿Qué quieres conseguir?", escrito como
  resultado y con las palabras del usuario, más un proyecto (existente o
  nuevo) y un campo de stack y dominio, que sirve para proponer límites,
  verificadores y herramientas.
- Otras secciones del menú lateral: Panel, Forja de prompts, CLAUDE.md,
  Verificadores, Ablación, Skills y conectores, Rutinas, Experimentos,
  Workflows, Renovación, Arranque y prueba, Exportar, Proyectos. Aparece
  también un "índice de esbeltez" con valor 95/100 y un "mayor lastre".

## Prompt 11 — 2026-09-11

> [adjunta `Manual_metodologia_Boris_Cherny.md` y `Navaja.html`] Te adjunto
> documentación de el método borischerny y el mock para que los tengas en
> cuenta como metodología para desarrollar los prompt en brinstorm.

Ambos ficheros quedan guardados en `docs/planificacion/referencias/`
(`manual-metodologia-boris-cherny.md` y `navaja-mock.html`). Revisados: no
contienen claves ni datos personales.

### Lo que fija el manual, y que Brainstorm hereda

La tesis: *el cuello de botella ya no es el modelo, sino las ataduras que le
ponemos. Menos instrucciones, más verificación.*

**Las tres partes de todo prompt** (lo que el usuario llamó objetivo,
guardarraíles y stop):

```
Tarea:            <qué resultado se quiere, una o dos frases>
Límites:          <qué no se puede tocar, restricciones inamovibles>
Terminado cuando: <comando o métrica verificable, no opinable>
```

Prohibido incluir pasos intermedios. Si hacen falta pasos, es que el criterio
de salida está mal definido. Y si la tarea es difícil de verificar, el problema
no es el prompt: falta el verificador, y se construye antes de lanzar la tarea.

Las diez normas del manual, resumidas: tres partes y nunca pasos; ninguna
instrucción persistente sin tres fallos medidos; CLAUDE.md de hechos, no de
procedimientos, en menos de 40 líneas; una skill solo vive si produce el mismo
resultado siempre; un hook o MCP solo vive si verifica algo o aporta un dato
externo; verificador automático antes de lanzar; ante fallo repetido se mejora
primero el verificador; rutinas de una frase que entregan PR; más de veinte
ficheros o goal drift dispara un workflow; cada generación de modelo dispara
ablación completa.

### Lo que aporta el mock de Navaja

- Fases del asistente: `Idea, Tarea, Límites, Verificación, Contexto,
  Herramientas, Resultado`.
- Catálogo de límites reutilizables, cada uno con su clave: no cambiar la API
  pública, sin dependencias nuevas sin nota explícita, ningún secreto ni clave
  en el repo, no tocar la configuración de CI, compatibilidad con los datos ya
  persistidos, mantener la MSRV declarada, no custodial, importes en msat como
  enteros, sin cambios visuales fuera del flujo pedido.
- Navaja ya resuelve la fase de estructurar un prompt a partir de una idea
  formada. Brainstorm cubre lo de antes: llegar a esa idea entre dos.

## Prompt 12 — 2026-09-11

> Quiero que tú cuando participes en el brainstorm adoptes distintos roles.
> Estos roles pueden ser de tipo crítico, analítico, puede ser creativo, puede
> ser, pues, otra serie de roles. ¿Cómo lo vamos a hacer? Pues, cuando yo ponga
> delante de mi prompt, por ejemplo, crítico:, lo que te pregunte o lo que
> lance en ese prompt, tú vas a contestar de forma crítica. Si pongo auditor: y
> un comentario, tú vas a adoptar el rol de auditor. Dime si lo has entendido
> y, si te parece buena idea.

**Mecanismo**: prefijo `rol:` al principio del prompt del usuario. Lo que sigue
se responde desde ese rol. Roles nombrados por el usuario: crítico, analítico,
creativo, auditor; la lista queda abierta.

Vale como convención de la conversación desde ya, y como funcionalidad de la
app cuando se construya.

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
- **Criterios de calidad de un prompt**: los fija el método Cherny, entregado
  en el prompt 11 y guardado en `docs/planificacion/referencias/`. Un prompt
  son tres partes, tarea, límites y criterio de salida, sin pasos intermedios.
  Brainstorm no reinventa el criterio: lo adopta como metodología de
  generación.
- **Un prompt es verificable o no vale**: el criterio de salida se expresa como
  comando o métrica. Un prompt generado por Brainstorm sin criterio de salida
  ejecutable está incompleto por definición, y eso da un test objetivo de
  calidad, no una impresión.
- **Roles por prefijo**: el usuario antepone `rol:` a su mensaje y Claude
  responde desde ese rol. Roles citados: crítico, analítico, creativo,
  auditor, lista abierta. Es el mecanismo concreto con el que se fuerza la
  complementariedad, que hasta ahora era un principio sin implementación.
- **El rol queda asociado a lo aportado**: si cada aportación sabe desde qué
  rol nació, la generación del prompt puede comprobar qué lentes se han
  aplicado y cuáles faltan.
- **Encargo de diseño**: en la planificación se espera creatividad de Claude en
  el planteamiento de la app, no una transcripción literal de los requisitos.

## Dudas abiertas

- ~~¿Claude participante o solo registro?~~ Resuelto en el prompt 3:
  participante.
- ~~¿Cómo se fuerza la complementariedad?~~ Resuelto en el prompt 12: roles
  invocados por prefijo. Queda por decidir el catálogo cerrado y el
  comportamiento por defecto sin prefijo.
- Sin prefijo, ¿qué rol aplica? Propuesta: ninguno, respuesta normal.
- ¿Puede Claude invocar un rol por iniciativa propia, avisando de que lo hace?
- ¿Se pueden encadenar roles en un mismo turno (`crítico+auditor:`)?
- ¿Un rol es solo un tono, o lleva asociada una plantilla de salida propia?
  Un auditor que siempre devuelve hallazgos con severidad es más útil que un
  auditor que solo suena severo.
- Retomar días después: ¿se recarga la transcripción entera o un resumen
  destilado? Con sesiones largas la transcripción íntegra no cabrá.
- ¿El prompt generado es un texto único o un documento estructurado por
  secciones (objetivo, alcance, stack, pantallas, criterios de aceptación)?
- ¿Una sesión equivale a una app, o varias sesiones pueden converger en el
  mismo prompt?
- ~~¿Se anota el motivo de descarte? ¿Se versionan los prompts?~~ Resuelto en
  el prompt 6: sí a las dos.
- ~~Detalle del método Cherny~~: entregado en el prompt 11.
- ¿Brainstorm y Navaja son dos apps separadas que se pasan el testigo, o
  Brainstorm absorbe el método y genera ya el prompt en sus tres partes? Sigue
  abierta, y el manual la agudiza: Navaja ya cubre de la idea al prompt, así
  que el hueco propio de Brainstorm es lo anterior, llegar a la idea entre dos.
- El manual exige que el criterio de salida sea verificable. Para una app que
  todavía no existe no hay comando que ejecutar. ¿Qué cuenta como criterio de
  salida en un prompt de creación desde cero: los tests que la app deberá
  pasar, una captura, un guion de prueba manual?
- El propio método pide ablación: ¿se aplica a Brainstorm consigo misma, es
  decir, arrancar con el CLAUDE.md mínimo y añadir solo lo que falle tres
  veces?
- Si el prompt generado sigue el método, ¿el entregable incluye también el
  CLAUDE.md mínimo, los verificadores y el comando de arranque, como en
  Navaja, o solo el texto del prompt?
