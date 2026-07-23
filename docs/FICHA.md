# Ficha de diseño: verificador

> **Estado: VIGENTE**
> Firmado: 2026-07-23 por Fede
> *Procedencia de la firma (`claude-batuta` · `decisiones/018`): re-ratificación en bloque en la mesa de firmas del 2026-07-23 — elección explícita de Fede en sesión interactiva («En bloque, las 11») tras presentársele las 5 decisiones técnicas de esta ficha con su contenido — acto humano rastreable registrado en la conversación y la memoria del taller. Ratifica esta ficha como plano del proyecto.*
> Diseñada: 2026-07-17 (decisiones elegidas por Fede ese día; la mención «firmadas» de entonces era pre-011 — el acto de firma es el de hoy) · Próxima sesión de construcción: S01 — Esqueleto ([#1](https://github.com/hifede1/claude-verificador/issues/1))

## 1. Propósito

**Problema que resuelve:** el audit-tracker mapea el contrato criterios↔tests y llama "deuda de verificación" a todo criterio no clavado por un test — pero nadie CONVIERTE los criterios en tests. La deuda se ve, no se paga.

**Para quién:** Fede y colaboradores, después de que una ficha está firmada y antes/durante la construcción.

**Una frase:** `verificador` convierte los criterios de aceptación de las fichas en tests trazables — cada criterio queda clavado o explícitamente marcado como verificación manual.

## 2. Tipo y forma

**Plugin de Claude Code** con un comando: `/criterios-a-tests`. Enriquece al audit-tracker por el flanco de la evidencia: el mapa de tests se llena con tests que nacen del contrato.

## 3. Funciones

| Función | Entrada | Salida | Comportamiento |
|---|---|---|---|
| `/criterios-a-tests` | Ficha(s) con criterios de aceptación + el proyecto | Tests (o esqueletos) con nombre trazable al criterio, y registro de los no testeables | 1) Lee los criterios de las fichas (`docs/FICHA.md`, `PLAN.md`). 2) Clasifica con el humano: ¿testeable automáticamente, testeable con esfuerzo, o verificación manual? — la clasificación la decide el humano con recomendación. 3) Genera los tests con convención de nombre trazable (`S02_C1_...` → sesión y criterio). 4) Lo no testeable queda registrado como verificación manual explícita (visible como deuda asumida, no olvidada). Detecta el framework de test del proyecto y sigue sus convenciones. |

**Regla transversal:** un criterio sin test y sin registro de verificación manual es un bug de la corrida — nada queda en el limbo.

## 4. Decisiones técnicas

| Decisión | Elegido | Alternativas descartadas | Por qué |
|---|---|---|---|
| Trazabilidad | Convención de nombre test↔criterio (sesión + número) | Comentarios sueltos, mapping en archivo aparte | El nombre viaja con el test; los archivos paralelos driftean. |
| Framework | Detectar el del proyecto y seguir sus convenciones | Imponer uno | La herramienta visita casa ajena; se adapta. |
| Clasificación de testeabilidad | La decide el humano con recomendación | Automática | Decidir qué es "verificable a mano" es estructural — no se inventa. |
| **Forma del test** (firmada) | **Esqueletos**: molde con arrange/act/assert planteado y `TODO` explícito; el constructor lo termina con el código real a la vista | Tests completos; mixto según el caso | Un test "completo" generado sin ver el código real puede dar VERDE FALSO — veneno para una metodología de evidencia dura. El esqueleto es honesto: marca dónde va la prueba, no finge que ya prueba. |
| **Lenguajes v1** (firmada) | **Agnóstico**: detecta el lenguaje/framework del proyecto y se adapta | Solo TS/JS; TS + Go | Cubre todos los proyectos del taller sin discriminar. **Salvaguarda innegociable** (ver abajo): ante un lenguaje que no domina, degrada a esqueleto neutro + verificación manual — jamás emite un test roto. |

**Salvaguarda que hace segura la combinación firmada:** las dos decisiones se refuerzan. Como la forma es SIEMPRE esqueleto, el riesgo de "adaptarse a cualquier lenguaje" queda neutralizado: un esqueleto en un lenguaje poco conocido es un molde etiquetado (honesto), no un test que miente. La frontera dura: si el detector no reconoce el lenguaje/framework con confianza, NO improvisa un test — emite un esqueleto neutro (arrange/act/assert como comentarios) y lo registra como verificación manual. Nunca un verde falso.

## 5. Fuera de alcance (v1)

- No corre los gates ni reporta cobertura — eso es del audit-tracker.
- No escribe tests para código sin ficha (no inventa criterios).
- No E2E con infraestructura pesada (browsers, despliegues).

## 6. Criterios de aceptación

- [ ] Sobre un repo de prueba con una ficha de 3 criterios, genera tests/esqueletos trazables para los testeables y registro explícito para los manuales (inspección de salida).
- [ ] Los nombres de test permiten llegar del test al criterio y viceversa sin ambigüedad.
- [ ] Ningún criterio queda sin clasificar tras una corrida (ni testeado ni registrado = falla).
- [ ] Corrido `/audit-tracker` después, el mapa de evidencia refleja el contrato lleno sin adaptación manual.

## 7. Plan de construcción

1. **S01 — Esqueleto:** plugin instalable → `/plugin install` funciona.
2. **S02 — Lectura y clasificación:** parseo de criterios + clasificación con el humano → criterio 3.
3. **S03 — Generación trazable:** tests/esqueletos con convención de nombres → criterios 1 y 2.
4. **S04 — Integración:** pipeline con audit-tracker → criterio 4; publicación.

## 8. Referencias

- README del audit-tracker — mapa de evidencia de tests y contrato criterios↔tests.
- `fichas/PLANTILLA.md` — formato de criterios que consume.
- `~/.claude/skills/go-testing/SKILL.md` — convenciones de test Go del taller.
