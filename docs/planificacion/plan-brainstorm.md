# Plan de Brainstorm — v1 (2026-09-11)

Planificación del proyecto a partir de los 15 prompts recogidos en
[`ideas-brainstorm.md`](ideas-brainstorm.md), el manual del método Cherny y el
mock de Navaja (`referencias/`). Este documento **decide** donde había dudas
abiertas: cada decisión va marcada como propuesta y el usuario la confirma o la
tumba en la siguiente sesión. Lo no verificado lleva `[SUPUESTO]` y su plan B.

Mock que acompaña a este plan: [`../index.html`](../index.html), build
`BR-B1-20260911-003`.

## 1. La app en tres líneas

Brainstorm es una mesa de trabajo entre el usuario y Claude para llegar, entre
los dos, a la idea de una app. Claude participa con roles distintos y todo lo
que se aporta queda en una **mesa** estructurada, no en un chat. Cuando el
usuario lo pide, la mesa se convierte en un **prompt de tres partes** para
Fable 5.1, versionado y con el motivo de cada descarte.

Brainstorm produce prompts. Nunca produce la app.

## 2. Los cuatro conceptos que sostienen el diseño

Aquí está la parte creativa que pidió el usuario: no es una transcripción de
los requisitos, es la forma que les da.

### 2.1 La Mesa, en lugar de la transcripción

El chat es el medio, no el estado. Cada aportación que vale se convierte en
una **ficha** sobre la mesa. Una ficha tiene:

| Campo | Valores |
|---|---|
| tipo | `idea`, `duda`, `funcionalidad`, `límite`, `criterio`, `decisión` |
| autor | `usuario` o `claude` |
| rol | el rol desde el que nació (`—` si fue respuesta normal) |
| estado | `abierta`, `aceptada`, `descartada` |
| origen | mensaje de la conversación del que sale |

Esto resuelve tres requisitos a la vez:

- **Complementariedad**: Claude no lee "lo que dijo el usuario", lee la mesa. Su
  instrucción es cubrir lo que la mesa no cubre, y la mesa dice qué tipos y qué
  roles faltan.
- **Retomar días después**: se recarga la mesa (pequeña y estructurada), no la
  transcripción. Ver 2.4.
- **Generar el prompt**: el prompt sale de las fichas aceptadas, no del chat.
  Una `duda` abierta bloquea la generación o entra como límite explícito; una
  `decisión` cierra dudas.

Las fichas las crea el usuario con un toque sobre un mensaje ("a la mesa") o
las propone Claude al final de cada respuesta como bloque `Propuestas para la
mesa`, que el usuario acepta una a una. Claude nunca escribe en la mesa sin
confirmación.

### 2.2 Roles con plantilla de salida, no solo tono

Decisión propuesta para la duda "¿un rol es un tono o una plantilla?": **cada
rol lleva una lente y un formato de salida obligatorio**. Un auditor que
devuelve hallazgos con severidad es útil; uno que solo suena severo, no.

Catálogo cerrado de la v1, ampliable por fichero de configuración:

| Prefijo | Lente | Formato de salida |
|---|---|---|
| `crítico:` | Qué falla, qué sobra, qué no aguanta | Lista de objeciones, cada una con qué requisito ataca |
| `analítico:` | Descomposición, dependencias, orden | Tabla de piezas con qué depende de qué |
| `creativo:` | Alternativas que nadie ha puesto | Tres alternativas divergentes, una frase cada una, sin evaluar |
| `auditor:` | Huecos frente al método Cherny | Hallazgos con severidad (`alta`/`media`/`baja`) y evidencia (qué ficha o su ausencia) |
| `usuario:` | La voz de quien usará la app | Escenario de uso en primera persona y qué le frustra |
| `escéptico:` | Por qué no habría que construirlo | La razón más fuerte para no hacerlo y qué la desactivaría |

Reglas, tal como se decidieron en los prompts 12 a 14:

- Sin prefijo, respuesta normal.
- Varios roles en un mensaje (`crítico, auditor:`) devuelven un bloque por rol.
- **Independencia forzada** (decisión propuesta para la duda del prompt 14):
  la app genera cada bloque en una llamada separada al modelo, con la misma
  entrada, y los muestra juntos. El segundo bloque no puede alinearse con el
  primero porque no lo ha visto. En la conversación con Claude esto no se puede
  garantizar; en la app sí, y es una de sus razones de ser.
- Claude puede invocar un rol por iniciativa propia. En la app se muestra con
  una marca visible: `Claude invoca auditor: porque la mesa no tiene ningún
  criterio de salida`. La regla que dispara la iniciativa es mecánica: si un
  tipo de ficha lleva N mensajes vacío, o si un rol no se ha usado nunca en la
  sesión, Claude lo propone.
- Cada ficha guarda el rol del que nació. La generación del prompt lista qué
  lentes se aplicaron y cuáles no (**cobertura de lentes**), y lo avisa antes
  de generar.

### 2.3 El prompt: tres partes y un anexo, con linter

Decisión propuesta para "¿texto único o documento por secciones?": **documento
con tres partes obligatorias y anexos opcionales**, en este orden:

```
Tarea:            <qué app, como resultado, una o dos frases>
Límites:          <fichas de tipo límite aceptadas>
Terminado cuando: <criterios verificables, ver 2.3.1>

--- Anexos (opcionales, se pueden quitar sin romper el prompt) ---
Contexto:         <hechos no deducibles: dominio, stack elegido, decisiones>
Pantallas:        <lista de pantallas con una frase cada una>
Fuera de alcance: <lo que el prompt no pide, para que el modelo no lo añada>
```

El texto del prompt es un solo bloque copiable. Los anexos van después de una
línea de corte, así el usuario los recorta si quiere el prompt mínimo.

**Linter del prompt** (adoptado de Navaja, verificable por máquina):

- La Tarea tiene una o dos frases y ningún verbo de paso ("primero", "luego",
  "después"). Si falla, el prompt no se guarda como versión: se guarda como
  borrador con el aviso.
- Límites no está vacío.
- Terminado cuando contiene al menos un criterio ejecutable (ver abajo).
- Ninguna ficha `duda` abierta sin resolver en la mesa; si las hay, se listan y
  el usuario decide generar igual (entran como límite "no decidir X") o
  resolverlas primero.

#### 2.3.1 Criterio de salida para una app que no existe

Decisión propuesta para la duda del manual: un prompt de creación desde cero
lleva **tres criterios, en este orden de fuerza**, y el linter exige al menos
el primero:

1. **Comandos que la app creada tiene que pasar**, escritos como si la app ya
   existiera: `cargo test` en verde, `cargo clippy` sin avisos, y un test e2e
   con nombre que el prompt describe en una frase (`tests/e2e/usuario.spec.ts:
   crea una sesión, añade una ficha, recarga y sigue ahí`). El modelo que
   reciba el prompt construye primero el verificador, como manda el método.
2. **Guion de prueba en navegador**: lista de pasos "quién hace qué y qué ve",
   numerada, que un humano ejecuta en cinco minutos. No es opinable: cada paso
   tiene un resultado observable.
3. **Métrica**: si la app tiene un número de salud (tiempo de respuesta,
   tamaño del CLAUDE.md, cobertura), se fija.

Lo que no vale como criterio: "que funcione bien", "que sea intuitiva".

### 2.4 Memoria de sesión en tres capas

Decisión propuesta para "¿transcripción entera o resumen?": ninguna de las dos,
sino tres capas que se cargan de más barata a más cara:

1. **La mesa**: siempre entera. Es pequeña por construcción (decenas de fichas,
   no cientos de mensajes).
2. **El destilado**: un texto de menos de 40 líneas que Claude reescribe al
   cerrar cada jornada, con lo que se decidió, lo que quedó abierto y el tono de
   la sesión. Se muestra al retomar como tarjeta "Han pasado N días. Esto es lo
   que hay".
3. **Los últimos K mensajes** de la conversación, con K fijado por presupuesto
   de tokens, más búsqueda en el resto de la transcripción bajo demanda.

La transcripción completa se guarda siempre; nunca se recarga entera.

## 3. Versiones del prompt y aprendizaje del descarte

Cada generación crea una versión `v1, v2, …` con:

- el texto del prompt,
- la **foto de la mesa** en ese momento (ids de las fichas que entraron),
- el resultado del linter,
- la cobertura de lentes,
- y, si se descarta, el **motivo** en palabras del usuario.

Cuando el usuario descarta con motivo, el motivo se convierte en una ficha de
tipo `criterio` sobre la mesa ("un prompt bueno no hace X"). Así la versión
siguiente arranca de lo aprendido sin que nadie tenga que recordarlo. Entre dos
versiones se puede ver el diff de texto y el diff de fichas.

Una versión aceptada cierra la sesión con estado `prompt entregado`; la sesión
puede reabrirse.

## 4. Alcance: Brainstorm y Navaja

Decisión propuesta: **dos apps separadas, con un testigo explícito**.

- Brainstorm cubre lo anterior a la idea formada: llegar a ella entre dos, con
  roles, mesa y versiones. Genera ya el prompt en sus tres partes, porque no
  tiene sentido entregar algo que no pase el linter del método.
- Navaja cubre lo posterior: CLAUDE.md mínimo, verificadores, herramientas,
  rutinas, ablación.
- El testigo es un fichero `paquete.json` que Brainstorm exporta y Navaja
  importa: idea, límites con sus claves, criterios de salida, contexto. Navaja
  no se toca en este proyecto; el import es trabajo suyo, y mientras no exista
  el paquete se pega a mano en su fase Idea.

Ablación sobre el propio Brainstorm: **sí, a los prompts de sistema de los
roles**. Cada rol arranca con la lente y el formato de 2.2 y nada más. Solo se
añade una instrucción a un rol cuando el mismo fallo se ha visto tres veces y
está apuntado en `docs/planificacion/experimentos.md`. El `CLAUDE.md` del repo
es el de la plantilla y no entra en la ablación.

Una sesión equivale a una app. Si dos sesiones convergen, la v2 de la app
permitirá importar fichas de otra sesión; en la v1 no.

## 5. Arquitectura

Encaja en lo que ya hay en el repo: backend Rust en `app/`, estáticos en
`docs/`, Pages y Fly por workflow.

| Pieza | Decisión | Por qué |
|---|---|---|
| Backend | Rust, axum + tokio (ya en `app/`) | Es lo que la plantilla despliega y verifica |
| Persistencia | SQLite en un volumen de Fly montado en `/data` | Un usuario, un fichero, copia trivial |
| Modelo | `claude-fable-5-1` vía API desde el backend | El prompt se redacta para ese modelo; que lo redacte él |
| Frontend | HTML, CSS y JS sin build, servido desde Pages, llamando a Fly con CORS | Igual que `holamundo.html`; funciona desde el móvil sin instalar nada |
| Acceso | Una clave de acceso en cabecera, guardada como secreto de Fly | Equipo de dos; el multiusuario real llega si hace falta |

`[SUPUESTO]` Fly permite un volumen en la app `brainstorm-npiobject-labs` con
el plan actual. Plan B: SQLite en el disco efímero con volcado diario a un
fichero JSON en el repo de datos privado, o Postgres gestionado de Fly.

`[SUPUESTO]` La clave de la API de Anthropic no llega heredada de la
organización como `FLY_API_TOKEN`; hay que hacer `fly secrets set
ANTHROPIC_API_KEY=… --app brainstorm-npiobject-labs` una vez desde el PC. Plan
B: un secreto de repositorio que `deploy.yml` pase con `--build-secret`; menos
limpio y queda anotado como deuda.

`[SUPUESTO]` Las llamadas al modelo caben en el tiempo de respuesta HTTP de Fly
(60 s por defecto). Plan B: respuesta en streaming por SSE, que además mejora
la experiencia en móvil.

### 5.1 Modelo de datos

```
sesion    (id, nombre, app_objetivo, estado, creada, ultima_actividad, destilado)
mensaje   (id, sesion, autor, roles[], texto, bloque_rol, creado)
ficha     (id, sesion, tipo, autor, rol, texto, estado, mensaje_origen, creada)
version   (id, sesion, numero, prompt, fichas[], linter, cobertura, estado,
           motivo_descarte, creada)
```

### 5.2 Rutas

Todas con `Access-Control-Allow-Origin: *`, como manda `CLAUDE.md`, y con la
clave de acceso en cabecera salvo `/salud` y `/holamundo`, que se quedan como
están.

```
GET    /sesiones                      lista
POST   /sesiones                      crea
GET    /sesiones/:id                  mesa + destilado + últimos mensajes
POST   /sesiones/:id/mensajes         mensaje del usuario → respuesta (un bloque por rol)
POST   /sesiones/:id/fichas           crea ficha (del usuario o aceptando una propuesta)
PATCH  /sesiones/:id/fichas/:fid      cambia estado o texto
POST   /sesiones/:id/versiones        genera prompt → versión nueva con linter
PATCH  /sesiones/:id/versiones/:n     acepta o descarta con motivo
POST   /sesiones/:id/cerrar-jornada   reescribe el destilado
GET    /sesiones/:id/paquete          paquete.json para Navaja
```

### 5.3 Pantallas (las del mock)

1. **Sesiones**: lista con estado, días desde la última actividad, número de
   fichas y de versiones. Botón "retomar".
2. **Sesión**, con tres pestañas que caben en un móvil:
   - **Mesa**: fichas agrupadas por tipo, con autor y rol; cobertura de lentes
     arriba; dudas abiertas destacadas.
   - **Conversación**: chat con prefijos de rol, bloques por rol, marca de rol
     invocado por Claude, botón "a la mesa" en cada párrafo y tarjeta de
     destilado al retomar.
   - **Prompts**: versiones con estado, motivo de descarte, linter y el texto
     de la versión actual en tres partes; botones generar, aceptar, descartar.

## 6. Fases

Cada fase se escribe como se escribirá su prompt de desarrollo: tarea, límites
y terminado cuando. Las fases se lanzan una a una, cada una en su sesión, y
cada una termina en `main` verificada por workflow.

### F0 · Planificación y mock — esta sesión

- **Tarea**: este documento y el mock 1.
- **Terminado cuando**: `plan-brainstorm.md` en `main`, mock publicado en Pages
  con build `BR-B1-20260911-003` y run de `pages.yml` en verde.

### F1 · La mesa persistente

- **Tarea**: sesiones y fichas con persistencia en SQLite sobre volumen de Fly,
  y frontend con las pantallas Sesiones y Mesa, sin Claude todavía.
- **Límites**: no tocar `/salud` ni `/holamundo`; sin dependencias fuera de
  axum, tokio, serde y sqlx; ninguna clave en `docs/`; `deploy.yml` sigue
  verificando `/salud`.
- **Terminado cuando**: `cargo test` en verde con tests de las rutas de
  sesiones y fichas; tras un `fly machine restart` una ficha creada antes sigue
  en `GET /sesiones/:id`; desde Pages en el móvil se crea una sesión y una
  ficha y se ven al recargar.

### F2 · Claude participa con roles

- **Tarea**: mensajes con prefijo de rol, un bloque por rol generado en
  llamadas independientes, propuestas para la mesa al final de cada respuesta,
  iniciativa propia con marca visible.
- **Límites**: los prompts de sistema de los roles solo llevan lente y formato
  (ablación desde el principio); Claude nunca crea fichas sin confirmación;
  la clave de la API solo vive en secretos de Fly.
- **Terminado cuando**: test que envía `crítico, creativo: …` y comprueba dos
  bloques con etiquetas distintas; test que fuerza una mesa sin criterios y
  comprueba que la respuesta trae la marca de rol invocado; guion de prueba en
  navegador de cinco pasos pasado en el móvil.

### F3 · Generar, versionar, descartar

- **Tarea**: generación del prompt en tres partes y anexos, linter, versiones
  con foto de la mesa, descarte con motivo que crea ficha `criterio`, diff
  entre versiones, pantalla Prompts.
- **Límites**: un prompt que no pasa el linter no se guarda como versión; el
  texto del prompt es copiable en un solo bloque.
- **Terminado cuando**: test que genera con una duda abierta y recibe el aviso;
  test que descarta v1 con motivo y comprueba la ficha `criterio` nueva y que
  v2 la incluye; el prompt generado pasa el linter de Navaja pegado en su
  Forja.

### F4 · Retomar días después

- **Tarea**: destilado al cerrar jornada, tarjeta al retomar, carga por capas
  con presupuesto de tokens, búsqueda en la transcripción.
- **Límites**: la transcripción nunca se recarga entera; el destilado no
  supera 40 líneas.
- **Terminado cuando**: test con una sesión de 300 mensajes sintéticos que
  retoma con menos de N tokens de contexto (N fijado en F2) y responde
  citando una decisión de los primeros mensajes.

### F5 · Testigo a Navaja y cierre

- **Tarea**: `GET /sesiones/:id/paquete`, exportar y copiar, sesión en estado
  `prompt entregado`, reapertura.
- **Terminado cuando**: el paquete de una sesión real se pega en la fase Idea
  de Navaja y Navaja genera prompt y CLAUDE.md sin editar nada a mano.

Fuera de la v1: multiusuario real, importar fichas entre sesiones, streaming
si el `[SUPUESTO]` de tiempos aguanta, y cualquier cambio en Navaja.

## 7. Primer prompt de desarrollo, ya en el formato

Como muestra de lo que Brainstorm producirá, y como lo que se lanzará para F1:

```
Tarea: en app/ (axum + tokio), añade sesiones y fichas de brainstorming
persistidas en SQLite sobre /data, y en docs/ las pantallas Sesiones y Mesa
del mock BR-B1-20260911-003 conectadas a esas rutas.
Límites: no cambies /salud ni /holamundo; sin dependencias fuera de axum,
tokio, serde y sqlx; ninguna clave ni endpoint interno en docs/; deploy.yml
sigue verificando /salud con el SHA; app/fly.toml sin clave app.
Terminado cuando: `cargo test` en verde con tests de GET/POST /sesiones y
POST/PATCH fichas; tras reiniciar la máquina de Fly una ficha creada antes
sigue en GET /sesiones/:id; desde Pages en móvil se crea una sesión y una
ficha y siguen ahí al recargar.
```

## 8. Lo que el usuario tiene que confirmar

1. El catálogo de seis roles y que cada uno lleve formato de salida (2.2).
2. Independencia forzada de los bloques de roles en la app (2.2).
3. Prompt de tres partes más anexos recortables (2.3).
4. Los tres tipos de criterio de salida para apps nuevas (2.3.1).
5. Memoria en tres capas y destilado de 40 líneas (2.4).
6. Dos apps con testigo `paquete.json`; Navaja no se toca (4).
7. Ablación solo sobre los prompts de los roles (4).
8. SQLite en volumen de Fly y clave de acceso simple (5), con sus supuestos.
9. Que F1 arranque con el prompt de la sección 7.
