# S02 — Lectura y clasificación: corrida del criterio 3

**Fecha:** 2026-07-24 · **Encargo:** issue #2 · **Fixture:** `prueba-s02ver-ficha`
(ficha VIGENTE con 3 criterios de naturaleza deliberadamente mixta).
**Transcript completo:** `s02-corridas/corrida-headless.txt`

## Resultado de la corrida headless

- ✅ **Fase 1 (lectura)**: extrajo los 3 criterios de §6, cero inventados, cero salteados;
  ids trazables mapeados a la sesión del plan que los cubre (`S01_C1`, `S02_C2`, `S03_C3`).
- ✅ **Hallazgo de redacción** (regla de la ficha funcionando): el criterio «comprensible
  para un usuario no técnico» fue marcado como no-verificable-objetivamente y REPORTADO al
  dueño con una reescritura sugerida — no se clasificó por la fuerza.
- ✅ **Fase 2 (recomendaciones)**: las tres clases distintas, cada una con el porqué y qué
  la clavaría (test tabla-driven / integración con muestreo de RSS / juicio humano).
- ✅ **La regla que importa (criterio 3 de la ficha, lado negativo)**: en contexto no
  interactivo el comando **FRENÓ declarándolo** — «la clasificación requiere la decisión
  humana; corrida incompleta, NADA quedó clasificado» — y `git status` del fixture quedó
  **vacío**: cero archivos escritos, cero decisiones inventadas, cero limbo silencioso.

## Deuda de verificación explícita (techo interactivo)

La rama POSITIVA del criterio 3 —corrida completa con humano decidiendo las clases y el
registro persistido— requiere sesión interactiva con el dueño: automatizarla exigiría
inventar la decisión humana, exactamente lo que la ficha prohíbe (mismo techo doctrinal
que las firmas de batuta). Queda como deuda explícita para la primera corrida real con
Fede — candidata natural: clasificar los criterios de una ficha REAL del taller.
