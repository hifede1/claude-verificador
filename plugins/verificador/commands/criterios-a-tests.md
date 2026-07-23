---
description: "Convierte criterios de aceptación de fichas en esqueletos de test trazables (test↔criterio) con clasificación humana — lo no testeable queda registrado como verificación manual explícita"
argument-hint: "[ruta de la ficha — opcional, default docs/FICHA.md]"
---

# /criterios-a-tests — Del criterio al test trazable

> ⚠️ **ESQUELETO (v0.1.0 — S01).** El comando existe y es instalable; el parseo y la clasificación
> (S02), la generación trazable (S03) y la integración con audit-tracker (S04) todavía NO están
> construidos. Este placeholder NO genera tests ni toca archivos.

Cuando esté construido (plan S02–S04, issues #2–#4 del repo), este comando va a:

1. **Leer** los criterios de aceptación de las fichas (`docs/FICHA.md`, `PLAN.md`).
2. **Clasificar CON EL HUMANO**: ¿testeable automático, testeable con esfuerzo, o verificación
   manual? La clasificación la decide el humano con recomendación — decidir qué es «verificable
   a mano» es estructural, no se inventa. **Nada queda sin clasificar**: un criterio en el limbo
   es un bug de la corrida.
3. **Generar SIEMPRE esqueletos** — arrange/act/assert planteado + `TODO` explícito, con nombre
   trazable al criterio (`S02_C1_...`). Jamás un test completo inventado sin ver el código real:
   eso puede dar VERDE FALSO, veneno para una metodología de evidencia dura.
4. **Registrar lo no testeable** como verificación manual explícita — deuda asumida, no olvidada.

**Salvaguarda innegociable:** si el detector no reconoce el lenguaje/framework del proyecto con
confianza, NO improvisa — emite un esqueleto neutro (arrange/act/assert como comentarios) y lo
registra como verificación manual. Nunca un test roto, nunca un verde falso.

## Qué hacer hoy

Informá al usuario que el verificador está en construcción (S01 cerrada: esqueleto instalable) y
que el contrato completo vive en `docs/FICHA.md` del repo `hifede1/claude-verificador`. Si el
usuario necesita clavar criterios YA, decile que escriba los esqueletos a mano con la convención
de nombre `S<NN>_C<N>_descripcion` — la trazabilidad no espera a la herramienta.

**No generes tests ni toques archivos. Este comando aún no verifica nada — decirlo claro es parte del contrato.**
