---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como este trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra todas las llamadas a herramientas y las ediciones de archivos a medida que ocurren. Puedes revisar los diffs en línea, aceptar o rechazar cambios individuales y guiar la conversación con prompts adicionales sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivo propuestas.
- guiarás al agente durante la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un control para el modo claro, pensado para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de empezar una conversación desde cero, guiarás la sesión existente para ampliar la tarea.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso que da el agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada cambio aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat y observa las llamadas a herramientas. Verás lecturas y ediciones de archivos, así como los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, aparecerá el diff en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo para ejecutar un comando de terminal, el agente se detendrá y mostrará un aviso **Allow**. Autoriza solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al finalizar, debería mostrar un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ya se ha implementado el modo de alto contraste. Ahora ampliarás la misma conversación para añadir un control de modo claro. Como continúas en el mismo chat, el agente conserva el contexto de su trabajo: los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control para el modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. Las personas usuarias deben poder activar el alto contraste y el modo claro de forma independiente.
    ```

2. El agente continuará desde el punto en que lo dejó, explorará los cambios existentes y propondrá los ajustes necesarios para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diff.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta refactorizar la interfaz de los controles de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviar una indicación más precisa.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts adicionales para pedir cambios («simplifica el CSS; utiliza variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa el conjunto de cambios

Antes de confirmar los cambios, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y las pruebas correspondientes.
3. Abre varios archivos modificados y revisa los diffs. Confirma que se siguen las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts adicionales para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado en tiempo real el trabajo del agente.
- has aceptado y rechazado ediciones individuales.
- has guiado una sesión en curso con prompts adicionales.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que el agente local ha generado durante este taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
