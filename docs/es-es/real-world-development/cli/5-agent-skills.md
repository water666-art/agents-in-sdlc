---
title: "Lección 5 - Personalizar y utilizar una habilidad de comprobaciones de calidad"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y utilizado archivos de instrucciones para que respete los estándares. ¿Y las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a saber cómo ejecutar operaciones de este tipo.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. Una habilidad es, en esencia, una carpeta que contiene un archivo Markdown llamado `SKILL.md`. El Markdown incluye metadatos con un nombre y una descripción para identificar la habilidad, una descripción general de su función e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita más archivos ni carpetas. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden guardarse en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta de habilidades de usuario `~/.copilot/skills`.

## Explora la habilidad

Exploremos la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al codespace y, en el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee los campos `name` y `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo invocar la habilidad.
3. Lee las instrucciones y observa cómo orientan el proceso de pruebas y lint.
4. Fíjate en que la habilidad todavía no incluye una sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Las habilidades pueden invocarse directamente desde Copilot CLI o mediante lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han comunicado las herramientas. Actualicemos la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o se ha bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta los comunica. Indica el siguiente paso para cada fallo o bloqueo y nunca presentes como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ya has actualizado la habilidad; veamos el cambio en acción. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y comprobaciones de tipos.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutado la habilidad.

Este cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
