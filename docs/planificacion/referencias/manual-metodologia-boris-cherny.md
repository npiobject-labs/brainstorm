# Manual de implantación: metodología Boris Cherny para Claude Code

**Fuentes:** `docu/partida/01 Resumen entrevista Boris Cherny.md` y `docu/partida/02 - El creador de Claude Code dice que borres tus skills.md`.
**Objetivo:** convertir las ideas de Cherny en normas operativas, aplicables desde hoy en cualquier proyecto (Rust, TypeScript, Python, infra).

---

## 0. La tesis en una frase

> El cuello de botella ya no es el modelo, sino las ataduras que le ponemos. Menos instrucciones, más verificación.

Todo lo que sigue se deduce de cinco principios:

| # | Principio | Consecuencia práctica |
|---|-----------|----------------------|
| P1 | **Unhobbling**: el modelo actual resuelve nativamente lo que antes había que parchear | Cada instrucción de "cómo hacerlo" es sospechosa por defecto |
| P2 | **Ablación**: se borra todo y se reintroduce línea a línea solo lo que demuestre valor | El contexto se gana, no se hereda |
| P3 | **Tarea + guardrails + criterio de salida**: el prompt define el *qué*, nunca el *cómo* | Sobreespecificar degrada el resultado |
| P4 | **Programación empírica**: el trabajo del programador es diseñar mecanismos de verificación | Tests, linters y capturas son el verdadero "prompt" |
| P5 | **Mantenimiento autónomo**: rutinas que limpian, unifican y podan sin intervención | El repo se mantiene solo; tú diseñas las rutinas |

---

## 1. Norma de prompting: la regla de las tres partes

**Regla:** toda instrucción a Claude (chat, CLAUDE.md, rutina o workflow) se escribe con exactamente tres bloques. Si un bloque contiene pasos intermedios, se borra.

```
TAREA      → qué resultado se quiere (una o dos frases)
GUARDRAILS → qué no se puede tocar / qué restricciones inamovibles hay
SALIDA     → cómo se sabe que está terminado (verificable, no opinable)
```

**Plantilla:**

```
Tarea: <objetivo de alto nivel>.
Límites: <no tocar X>, <no cambiar la API pública>, <mantener compatibilidad con Y>.
Terminado cuando: <comando> pasa en verde, <linter> sin warnings, <métrica> cumple <umbral>.
```

**Ejemplo real (Rust):**

```
Tarea: migra el crate `l402-server` de `hyper 0.14` a `hyper 1.x`.
Límites: no cambies la firma pública de ningún handler; no añadas dependencias nuevas fuera de la familia hyper/http-body.
Terminado cuando: `cargo test --workspace` en verde, `cargo clippy -- -D warnings` limpio, `cargo audit` sin nuevos avisos.
```

**Prohibido:** "primero lee el archivo X, luego crea la estructura Y, después…". Si necesitas pasos, es que el criterio de salida está mal definido.

**Regla de oro:** si la tarea es difícil de verificar, el problema no es el prompt sino que falta la herramienta de verificación. Constrúyela antes de lanzar la tarea.

---

## 2. Norma de ablación: cómo tratar CLAUDE.md, skills y hooks

**Regla:** cada instrucción persistente debe justificar su existencia con un fallo *repetido y medible* del modelo. Sin fallo documentado, no hay instrucción.

### 2.1 Protocolo de ablación (aplicar a cualquier proyecto existente)

1. **Congela** el estado actual: `git tag pre-ablation-$(date +%F)` para poder volver.
2. **Vacía** `CLAUDE.md`, `.claude/skills/`, `.claude/hooks/` y los prompts de rutinas. Deja solo lo estrictamente factual (ver §2.3).
3. **Trabaja una semana** con el proyecto así. Anota cada fallo real del modelo en `docs/ablation-log.md` con este formato:

   ```
   | fecha | fallo observado | veces | instrucción candidata | ¿reintroducida? |
   ```
4. **Reintroduce** solo las líneas cuyo fallo se haya repetido ≥ 3 veces y se pueda medir (test que rompe, lint que falla, formato incorrecto).
5. **Repite** el ciclo completo con cada nueva generación de modelo (§7).

### 2.2 Tres preguntas antes de escribir cualquier línea en CLAUDE.md

1. ¿El modelo lo falla de forma repetida **sin** esta línea? Si no lo has probado, no la escribas.
2. ¿Es un hecho del proyecto o una instrucción de procedimiento? Solo pasan los hechos.
3. ¿Se puede sustituir por una verificación automática (test, linter, hook de CI)? Si sí, escribe la verificación, no la instrucción.

### 2.3 Qué SÍ va en CLAUDE.md (hechos, no procedimientos)

- Comandos canónicos: build, test, lint, run (una línea cada uno).
- Decisiones arquitectónicas no deducibles del código ("no custodial: las claves nunca salen del cliente").
- Restricciones legales/fiscales/de dominio que el código no expresa.
- Qué está fuera de alcance del repo.

**Tamaño objetivo:** < 40 líneas. Si crece, algo se está convirtiendo en procedimiento.

### 2.4 Qué NO va nunca

- Estilo de código que un formateador/linter ya impone.
- Explicaciones de cómo funciona el lenguaje o el framework.
- Secuencias de pasos ("primero…, luego…").
- Advertencias genéricas ("ten cuidado con…", "asegúrate de…").

---

## 3. Norma de skills: repetibilidad sí, rendimiento no

**Regla de decisión:** una skill solo se conserva si su propósito es producir **el mismo resultado idéntico cada vez**. Si el objetivo es "la mejor solución posible", la skill estorba y se borra.

| Tipo de tarea | ¿Skill? | Motivo |
|---------------|---------|--------|
| Facturas, propuestas, informes con formato de marca | **Sí** | Se busca uniformidad, no creatividad |
| Plantillas de release notes, changelog, ADR | **Sí** | Formato fijo |
| Diseñar, refactorizar, depurar, migrar código | **No** | El modelo encuentra rutas mejores solo |
| Arquitectura, elección de tecnología | **No** | La receta limita el razonamiento |
| Configurar entornos con pasos reproducibles exactos | **Sí, si es idéntico siempre** | Si varía por contexto → No |

### 3.1 Auditoría trimestral de `.claude/skills/`

Para cada skill, responde en una tabla:

```
| skill | ¿resultado idéntico cada vez? | ¿usada en los últimos 30 días? | ¿el modelo lo hace igual sin ella? | decisión |
```

Se borra cualquier skill con un "No" en la primera columna o un "Sí" en la tercera. **Se borra, no se archiva**: si hace falta, está en git.

### 3.2 Mismo criterio para hooks y MCP servers

Un hook o un MCP que no ejecute una **verificación** (test, lint, comprobación de formato) o no dé acceso a un **dato externo** que el modelo no tiene, se elimina.

---

## 4. Norma de verificación: programación empírica

**Regla:** antes de lanzar una tarea, existe un mecanismo automático que dice "sí/no" sobre el resultado. Sin ese mecanismo, la tarea no se lanza.

### 4.1 Los tres verificadores mínimos por proyecto

1. **Tests ejecutables en un comando** (`cargo test`, `bun test`, `pytest`). Cobertura de los invariantes de negocio, no del 100 % de líneas.
2. **Linter/analizador estático estricto** (`clippy -D warnings`, `eslint --max-warnings 0`, `ruff`). Configurado para fallar, no para avisar.
3. **Verificación de contrato externo** cuando aplique: OpenAPI diff, snapshot de UI, captura comparada píxel a píxel, comparación de salida contra fixture.

### 4.2 Método científico aplicado

```
Hipótesis   → "el modelo puede hacer X solo"
Experimento → lanzar la tarea con las tres partes (§1) y sin instrucciones de proceso
Medición    → ¿pasan los verificadores? ¿cuántas iteraciones necesitó?
Ajuste      → si falla repetidamente: primero mejora el verificador; solo después añade una línea de contexto
```

### 4.3 Bucle de autocorrección

El criterio de salida debe estar formulado de modo que el modelo pueda **iterar solo hasta cumplirlo**: "hasta que `cargo test` pase en verde" y no "haz que los tests pasen". La diferencia es que el primero habilita ejecución desatendida durante horas o días.

### 4.4 Caducidad de las evaluaciones

Los evals propios (suites de prompts + resultado esperado) se saturan en 2–3 generaciones de modelo. Marca cada eval con la generación en que se creó y renuévalo cuando todos los modelos lo pasen al 100 %: ya no discrimina nada.

---

## 5. Norma de mantenimiento autónomo: rutinas

**Regla:** el repo tiene rutinas programadas que lo mantienen sin intervención. Cada rutina es una frase (tres partes, §1), nunca un procedimiento.

### 5.1 Las cuatro rutinas de base (adaptadas de las que usa Anthropic)

| Rutina | Prompt (completo) | Frecuencia |
|--------|-------------------|------------|
| **Dead code** | Tarea: elimina código no referenciado. Límites: no toques APIs públicas ni feature flags activos. Terminado cuando: tests y lint en verde y el PR lista lo eliminado. | Diaria / semanal |
| **Abstraction police** | Tarea: unifica abstracciones duplicadas. Límites: sin cambios de comportamiento observable. Terminado cuando: tests en verde y el PR explica cada fusión. | Semanal |
| **Poda de tests** | Tarea: borra tests redundantes o que no fallan ante ningún cambio realista; añade tests para invariantes sin cobertura. Límites: cobertura de invariantes de negocio no baja. Terminado cuando: suite en verde y tiempo de ejecución ≤ anterior. | Semanal |
| **Dependencias** | Tarea: actualiza dependencias con avisos de seguridad. Límites: sin cambios de major sin nota explícita. Terminado cuando: `cargo audit`/`npm audit` limpio y tests en verde. | Semanal |

### 5.2 Reglas de las rutinas

- Cada rutina entrega un **PR**, nunca escribe en `main`.
- Si una rutina falla dos veces seguidas, se revisa el **verificador**, no el prompt.
- Ninguna rutina tiene más de 4 líneas de prompt. Si crece, se divide en dos rutinas.

---

## 6. Norma de escala: Dynamic Workflows

**Regla:** cuando una tarea toca más de ~20 archivos, o cuando una sola conversación empieza a saturar contexto o a perder el objetivo (*goal drift*), no se insiste en el chat: se lanza un workflow.

### 6.1 Cuándo

- Migraciones completas (lenguaje, framework, versión mayor).
- Auditorías de todo el repo (seguridad, deuda técnica, cobertura).
- Refactorizaciones transversales (renombrado de abstracciones, cambio de patrón de errores).

### 6.2 Patrón canónico: Fan-Out & Synthesize

```
Fase 1  Descomposición   → 1 agente divide el trabajo en fragmentos independientes (devuelve JSON)
Fase 2  Paralelización   → N agentes, uno por fragmento, aislados (read-only o worktree)
Fase 3  Maker-Checker    → agentes verificadores independientes ejecutan tests/linters sobre cada resultado
Fase 4  Consolidación    → 1 agente sintetiza: PR unificado + informe
```

### 6.3 Elección de primitiva

| Necesidad | Primitiva |
|-----------|-----------|
| La siguiente fase necesita **todos** los resultados (consolidar métricas, decidir plan) | `parallel()` — barrera síncrona |
| Cada elemento es **independiente** (refactorizar → probar, archivo a archivo) | `pipeline()` — streaming entre etapas |
| El agente modifica disco en paralelo con otros | `isolation: "worktree"` |
| El agente solo analiza | `isolation: "read-only"` |

### 6.4 Esqueleto reutilizable

```javascript
export const meta = {
  name: 'audit-fix',
  description: 'Auditar, corregir y verificar módulos',
  phases: [{ title: 'Scan' }, { title: 'Audit' }, { title: 'Fix+Verify' }, { title: 'Synthesize' }],
}

// 1. Descomposición
const modules = await agent(
  'Tarea: lista los módulos de src/ con baja cobertura o deps obsoletas. Devuelve solo un array JSON de rutas.',
  { phase: 'Scan', schema: { type: 'array', items: { type: 'string' } } })

// 2. Auditoría en paralelo (barrera)
const reports = await parallel(modules.map(m => () =>
  agent(`Tarea: audita "${m}". Límites: solo lectura. Terminado cuando: devuelves hallazgos marcados CRÍTICO/MENOR.`,
        { phase: 'Audit', isolation: 'read-only', label: `audit:${m}` })))

const targets = modules.map((m, i) => ({ m, r: reports[i] })).filter(t => /CRÍTICO/.test(t.r))

// 3. Corrección → verificación en streaming
const patches = await pipeline(
  targets,
  t => agent(`Tarea: corrige los hallazgos CRÍTICO de "${t.m}". Informe: ${t.r}. Límites: sin cambios de API pública.`,
             { phase: 'Fix+Verify', isolation: 'worktree', label: `fix:${t.m}` }).then(fix => ({ ...t, fix })),
  r => agent(`Tarea: verifica "${r.m}". Terminado cuando: tests y lint del módulo en verde; si fallan, corrige hasta que pasen.`,
             { phase: 'Fix+Verify', label: `verify:${r.m}` }).then(v => ({ ...r, v })))

// 4. Síntesis
return agent(`Tarea: resume las correcciones verificadas y prepara el PR. Datos: ${JSON.stringify(patches)}`,
             { phase: 'Synthesize' })
```

**Nota:** el runtime limita la concurrencia (≈16 agentes simultáneos) y encola el resto; no hace falta gestionar cuotas a mano.

---

## 7. Norma de renovación: qué hacer con cada nueva generación de modelo

**Regla:** un cambio de generación de modelo dispara obligatoriamente el ciclo de ablación completo. Sin excepciones.

Checklist al aparecer un modelo nuevo:

- [ ] `git tag pre-ablation-<modelo>`
- [ ] Vaciar CLAUDE.md a los hechos de §2.3
- [ ] Borrar todas las skills que no cumplan §3
- [ ] Borrar hooks que no verifiquen algo
- [ ] Reducir cada prompt de rutina a las tres partes
- [ ] Trabajar 1 semana con `ablation-log.md` abierto
- [ ] Reintroducir solo lo que tenga ≥ 3 fallos medidos
- [ ] Revisar evals: retirar los saturados, crear más difíciles

---

## 8. Plan de implantación en un proyecto

### Día 1 (2–3 horas)

1. Asegurar los tres verificadores de §4.1 en un solo comando cada uno.
2. Ejecutar el protocolo de ablación §2.1 (pasos 1 y 2).
3. Escribir el CLAUDE.md mínimo (plantilla en §9.1).
4. Crear `docs/ablation-log.md`.

### Semana 1

5. Toda tarea se pide con las tres partes (§1). Sin excepciones, aunque cueste.
6. Registrar fallos en el log. No reintroducir nada todavía.
7. Configurar la rutina de *dead code* (§5.1) entregando PR.

### Semana 2–4

8. Reintroducir en CLAUDE.md solo lo que cumpla el umbral de 3 fallos.
9. Activar el resto de rutinas de §5.1.
10. Primer workflow (§6) sobre una tarea real de > 20 archivos.

### Mensual / trimestral

11. Auditoría de skills (§3.1) y hooks (§3.2).
12. Revisión de evals (§4.4).
13. Con cada modelo nuevo: §7.

---

## 9. Plantillas

### 9.1 CLAUDE.md mínimo

```markdown
# <proyecto>

## Comandos
- build: `cargo build --workspace`
- test:  `cargo test --workspace`
- lint:  `cargo clippy --workspace -- -D warnings`
- run:   `cargo run -p <bin>`

## Hechos no deducibles del código
- <decisión arquitectónica 1>
- <restricción de dominio/legal 1>

## Fuera de alcance
- <lo que este repo no hace>
```

### 9.2 Prompt de tarea

```
Tarea: …
Límites: …
Terminado cuando: … (comando/métrica verificable)
```

### 9.3 Registro de ablación (`docs/ablation-log.md`)

```markdown
| fecha | fallo observado | veces | instrucción candidata | reintroducida |
|-------|-----------------|-------|-----------------------|---------------|
```

### 9.4 Auditoría de skills

```markdown
| skill | idéntico cada vez | usada 30d | modelo lo hace sin ella | decisión |
|-------|-------------------|-----------|-------------------------|----------|
```

---

## 10. Resumen: las 10 normas

1. Todo prompt tiene tres partes: tarea, límites, criterio de salida. Nunca pasos.
2. Ninguna instrucción persistente sin ≥ 3 fallos medidos que la justifiquen.
3. CLAUDE.md contiene hechos, no procedimientos, y cabe en 40 líneas.
4. Una skill solo vive si su fin es un resultado idéntico cada vez.
5. Un hook o MCP solo vive si verifica algo o aporta un dato externo.
6. Antes de lanzar una tarea existe un verificador automático que responde sí/no.
7. Si el modelo falla repetidamente, primero se mejora el verificador; después el contexto.
8. El repo tiene rutinas de una frase que entregan PRs: dead code, abstraction police, tests, dependencias.
9. Más de ~20 archivos o síntomas de goal drift → workflow con Fan-Out & Synthesize.
10. Cada nueva generación de modelo dispara ablación completa y renovación de evals.
