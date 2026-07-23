---
description: "Lee los criterios de aceptación de la ficha, recomienda una clasificación por criterio (testeable automático / con esfuerzo / verificación manual) y la decide CON el humano — nada queda sin clasificar; la generación de esqueletos (S03) aún no existe"
argument-hint: "[ruta de la ficha — opcional, default docs/FICHA.md]"
---

# /criterios-a-tests — Del criterio al test trazable

Convertís criterios de aceptación en un contrato de verificación. En esta versión (S02) hacés
las fases de **lectura y clasificación**; la generación de esqueletos es S03 y la integración
con audit-tracker es S04 — NO están construidas: no generás tests ni escribís archivos de test.

Ficha a leer: $ARGUMENTS — sin argumento, `docs/FICHA.md` del repo actual (y `PLAN.md` si
existe). Sin ficha → frenás: no inventás criterios (regla §5 de tu propia ficha).

## Fase 1 — Lectura

1. Extraé TODOS los criterios de aceptación (checkboxes de §6 o sección equivalente). Cada
   uno recibe un id trazable `S<NN>_C<N>` (sesión del plan que lo cubre + número de orden).
2. Si un texto es ambiguo o no es verificable como está escrito, lo marcás: «criterio no
   verificable tal como está redactado» ES un hallazgo para el dueño, no un criterio a
   clasificar por la fuerza.

## Fase 2 — Clasificación (la decide EL HUMANO)

3. Para cada criterio, RECOMENDÁ una clase con el porqué y qué lo clavaría:
   - **Testeable automático**: un test lo clava sin infraestructura pesada (función pura,
     parser, contrato de API con mock). Decí qué test.
   - **Testeable con esfuerzo**: clavable pero exige fixture/entorno real (proceso vivo,
     E2E, red). Decí qué haría falta.
   - **Verificación manual**: requiere juicio humano o acto humano (una firma, un criterio
     estético, una validación de dueño). Decí cómo se verifica a mano y cuándo.
4. **La clasificación la decide el humano** — presentá las recomendaciones con
   AskUserQuestion (agrupá de a 4 por llamada; opciones: las tres clases, recomendada
   primera). Tu recomendación informa; su elección manda.
5. **Contexto no interactivo** (corrida headless, sin humano que responda): emitís la tabla
   de recomendaciones completa y **FRENÁS ahí, declarándolo**: «la clasificación requiere la
   decisión humana — corrida incompleta, nada quedó clasificado». No escribís NINGÚN archivo,
   no clasificás por default, no marcás nada como decidido. Una corrida sin humano NO puede
   terminar «clasificada» — eso sería inventar la decisión estructural que la ficha reserva
   al humano.

## Fase 3 — Registro (solo con la clasificación humana completa)

6. Con TODAS las decisiones tomadas, emití la tabla final: id · criterio · clase (decidida
   por el humano) · qué lo clava (test futuro de S03) o cómo se verifica a mano. Ofrecé
   persistirla en `docs/verificacion/criterios-clasificados.md` del repo visitado — solo
   con confirmación explícita; es el contrato que S03 consume.
7. **Regla transversal (criterio 3 de tu ficha): NADA queda en el limbo.** Al cerrar una
   corrida completa, cada criterio está clasificado-y-registrado o la corrida FALLA
   diciéndolo. El limbo es un bug tuyo, no del proyecto visitado.

## Reglas de oro

- No generás tests ni esqueletos (S03) ni tocás el tracker (S04) — decilo si te lo piden.
- No inventás criterios para código sin ficha (§5).
- La herramienta visita casa ajena: leés su ficha, no la reescribís; los hallazgos de
  redacción se REPORTAN al dueño.
- Jamás un verde falso: recomendación ≠ decisión; sin humano no hay clasificación.
