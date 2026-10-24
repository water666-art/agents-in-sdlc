---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora los revisarás en conjunto y abrirás una solicitud de incorporación de cambios. Tú combinaste la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones, la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una misma conversación y rama, y lo incluirás en una sola PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás el cambio completo y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

Durante el desarrollo del filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, sin quitar a la persona desarrolladora el control de la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo necesario para incorporar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los impedimentos de la PR —corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y actualiza la rama cuando hace falta— y activa la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Utiliza Agent Merge para gestionar la PR

Ya has terminado de crear el código. Vamos a revisarlo, crear la PR y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y, cuando el repositorio lo permita, completa manualmente la combinación tras revisar el trabajo.

## Resumen y pasos siguientes

Has automatizado varias partes del desarrollo: generar código, probarlo y validarlo, y ahora gestionar una solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación.
- has inspeccionado el cambio completo y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge solo después de revisar el trabajo y has confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y el uso compartido, así como la delegación opcional al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
