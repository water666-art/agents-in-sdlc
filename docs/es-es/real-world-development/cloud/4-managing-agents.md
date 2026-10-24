---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de GitHub Copilot en la nube que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar esas tareas.

Hay varias herramientas para gestionar el trabajo asignado al agente, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para solicitar otra funcionalidad.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Ya que se estaba actualizando el estilo del sitio y se iba a añadir una opción para cambiar de modo, tenía sentido incluir esa funcionalidad. Quieres guiar el trabajo del agente para asegurarte de que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de GitHub Copilot en la nube

Vamos a consultar el estado actual de todas las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas, tanto en el panel principal como en el de la izquierda. Deberían aparecer las tareas que has asignado a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso. Revisa las operaciones que ha realizado Copilot. Entre ellas puede haber:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo de Copilot.
   - Configurar servidores MCP.
   - Realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios, que aparece a la derecha. Permite consultar la PR y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que añada la opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para cambiar a ese modo y que las personas puedan elegir el modo de visualización que prefieran.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibir tu mensaje.

El proceso también tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y guiar las tareas del agente de GitHub Copilot en la nube. Puedes hacer un seguimiento de todas las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para cambiar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar sus tareas.
- redirigir una sesión en curso para solicitar otra funcionalidad.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
