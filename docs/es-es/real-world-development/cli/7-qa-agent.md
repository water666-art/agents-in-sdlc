---
title: "Lección 7 - Crear y utilizar un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de control de calidad claramente definido.

En esta lección:

- explorarás cómo puede trabajar un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar ese proceso y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot, definida en un perfil de Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el perfil de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de control de calidad indica a Copilot cómo utilizar estas capacidades para comprobar los requisitos y comunicar las conclusiones. No sustituye a esas herramientas ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil de control de calidad reutilizable. El perfil definirá las comprobaciones que debe realizar y los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación sobre el filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comparar las funcionalidades con sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, utilizar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar cada requisito como superado, fallido o bloqueado y aportar pruebas que lo justifiquen. Debe pedir permiso antes de cambiar el código de implementación y no debe confirmar cambios mediante commits ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea únicamente el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de utilizar el nuevo agente, revisa su perfil para confirmar que Copilot ha capturado el flujo de control de calidad y los límites previstos. Así evitarás que un agente incompleto o demasiado amplio cambie la funcionalidad cuando solo querías verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee los metadatos. El campo `description` es obligatorio; `name` es opcional, pero permite asignar al agente un nombre visible.
3. Lee las instrucciones del perfil y confirma que el proceso de control de calidad empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que el agente aporta pruebas para sus conclusiones, pide permiso antes de cambiar el código de implementación y no confirma cambios mediante commits ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado no incluye alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, reanuda la misma conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá todavía no aparezca en la lista. El comando anterior lo activará de todas formas.

2. Pide al agente QA que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para crear una PR?
   ```

3. El agente QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y pasos siguientes

Has añadido un perfil especializado reutilizable al flujo de trabajo y has revisado su resultado. En esta lección:

- has explorado cómo puede trabajar un agente personalizado con instrucciones, habilidades y herramientas MCP.
- has creado e inspeccionado un perfil reutilizable de control de calidad.
- has seleccionado el agente QA y revisado sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil QA, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de la funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
