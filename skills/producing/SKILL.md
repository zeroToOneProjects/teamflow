---
name: producing
description: "Use when a feature has passed review and needs to be finalized, archived, and its knowledge extracted to platform specs"
---

# Producing: Finalizacion y Archivado

## Overview
Verifica goal-backward contra el spec, archiva el feature y extrae conocimiento a platform specs. La spec formal se promueve a `specs/[dominio]/` como verdad permanente.

## El Proceso

1. **Verificacion goal-backward:** Despacha `tf-verifier` via Task tool (`subagent_type: "general-purpose"`, nombre `tf-verifier`). Pasa inline los success criteria del spec y la lista de archivos modificados. El verifier aplica 3 niveles: Exists → Substantive → Wired (ver skill `teamflow:verifying` para detalle).

2. **Si verificacion falla:** Genera gap report. NO archiva. Regresa a executing con los gaps como tareas.

3. **Si verificacion pasa — Promover spec y archivar:**
   - Lee `target_domain` de `state.yaml` (definido durante specifying)
   - Copia `spec.md` a `.workflow/specs/[target_domain]/[nombre].md` — esta es la verdad permanente de reglas de negocio
   - Mueve el resto del directorio `wip/` a `_archive/`
   - Genera resumen en `history/summaries/` usando template `summary.md`

4. **Extraer conocimiento:** Revisa si el feature produjo:
   - Nuevas decisiones arquitectonicas → actualiza platform specs
   - Nuevos patrones o convenciones → agrega a platform specs
   - Platform specs son archivos en `.workflow/platform/` — NUNCA se eliminan, solo se actualizan
   - **Guidelines grandes:** Si un guideline generado o actualizado excede ~20KB, sugiere al usuario convertirlo a skill con `/create-skill` para mejor manejo de contexto.

5. **Limpiar estado:** Remueve el feature activo de `state.md`.

## Siguiente Paso
"Feature archivado y spec promovida a specs/[dominio]/. Usa /spec para el siguiente feature o /status para ver el estado."
