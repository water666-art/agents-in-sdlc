---
title: "Lección 7 - Planificar antes de editar"
description: "Utiliza el modo Plan en una segunda incidencia para que el agente investigue el proyecto y proponga un enfoque antes de modificar archivos."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-05
---

No todas las incidencias deben empezar con modificaciones. El modo Plan investiga el proyecto, propone un enfoque y espera tu aprobación antes de cambiar el código.

En esta lección:

- iniciarás una sesión para una segunda incidencia en el modo Plan.
- revisarás y perfeccionarás el plan propuesto por el agente.
- aprobarás el plan y elegirás cómo continúa la sesión.

## Acuerda el enfoque antes de cambiar el código

1. Abre una **segunda incidencia** desde **Issues** y selecciona **New session**.
2. En la configuración de la sesión, elige **Plan** en lugar de **Interactive** o **Autopilot**.
3. Envía el siguiente prompt y deja que el agente investigue sin modificar archivos:

   ```plaintext
   Planifica cómo implementar esta incidencia. Investiga el cuestionario actual, enumera los archivos que cambiarías, señala los riesgos para la accesibilidad y para la restricción de un único archivo, y detente antes de editar nada.
   ```

4. Lee el plan propuesto y solicita cambios si falta algo.
5. Aprueba el plan.
6. Cuando se te solicite, elige si la sesión continúa en modo **Interactive** o **Autopilot**.

> [!TIP]
> **Cuándo merece la pena el modo Plan**
>
> Utiliza el modo Plan para cualquier tarea ambigua, transversal o costosa de deshacer. Corregir un mal enfoque cuesta menos antes de la primera modificación.

## Resumen y pasos siguientes

Has acordado un enfoque con el agente antes de que escribiera código. Continúa con la [lección 8: Completar el ciclo de revisión de Copilot][next-lesson].

[next-lesson]: ../8-review-loop/
