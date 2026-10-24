---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de una persona desarrolladora consiste en localizar todos los archivos necesarios, realizar cambios, ejecutar pruebas, depurar, descubrir qué archivo falta, actualizarlo y repetir el proceso.

El modo agente de Copilot puede ayudar con este trabajo.

El modo agente de Copilot funciona de forma más autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que detecta. Veamos cómo utilizarlo para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite iterar y recibir comentarios rápidamente. El agente en la nube está pensado para trabajar de forma asíncrona, como otra persona del equipo, y colaborar mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos con GitHub Copilot.
- revisar los cambios y las pruebas generados por el modo agente antes de incorporarlos al código.

## Escenario

El catálogo de juegos está creciendo y quieres permitir que se filtren por categoría y editorial. En el ejercicio anterior creaste una función auxiliar para editoriales; ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de cambiar nada, explora el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar juegos y ver sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local en su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade mediante un `<script>` Astro pequeño y acotado que utiliza las API estándar del DOM.

### Inicia el sitio web

Para facilitar el inicio del sitio, hay una tarea `npm run dev` que ejecuta el único servidor de desarrollo de Astro. Puedes iniciarlo en el codespace de GitHub:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un aviso parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en una ventana del navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Si utilizas un codespace, al seleccionar el enlace a la URL localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio

Cuando el sitio esté en marcha, podrás explorar sus funciones principales:

- **Página principal**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página de detalles con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. La selección de modo y modelo varía según el ejercicio; cada paso indicará lo que debes elegir.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que lo muestre.

1. Selecciona **Agent** en el menú de agentes de la vista Chat. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras de este taller no recomiendan un modelo concreto. Durante su creación utilizaron Claude Sonnet 4.5 y lo incluyeron para intentar mantener la coherencia de las sugerencias y ofrecer una experiencia adecuada. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden ser distintas de los ejemplos. Es completamente normal.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje grandes, Copilot puede utilizar un comando MCP diferente y completar la tarea igualmente.

3. Envía a Copilot este prompt para consultar el backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según cuáles sean más útiles para las personas usuarias.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según las que considera más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto sobre su trabajo. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo añadir funcionalidades al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidades, entre ellas:
   - Una descripción de la arquitectura.
   - Principios para el diseño de componentes, las pruebas y la accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en archivos más pequeños y enlazar a ejemplos y plantillas.

## Implementa la funcionalidad de filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios diferentes en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas del comportamiento del filtrado.
- Actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo agente de Copilot puede realizar estas tareas.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de una selección de las instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que sigue seleccionado **Claude Sonnet 4.5** como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del backlog:

   ```plaintext
   Actualiza el sitio para permitir filtrar por editorial y categoría según los requisitos de la incidencia correspondiente del backlog de GitHub. La función auxiliar de editoriales ya existe desde el ejercicio anterior; consérvala y mejórala si es necesario mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en marcha, así que no tienes que iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la funcionalidad. Encontrará las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante ese proceso, quizá aparezcan los controles **Keep** y **Undo** en el editor. Cuando termine, podrás conservar o deshacer todos los cambios; no hace falta seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alternará entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de una sesión de Chat completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y comprueba si hay posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio en marcha y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! En este ejercicio has explorado cómo utilizar el modo agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- utilizar el modo agente para implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados por el modo agente antes de incorporarlos al código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si la persona usuaria podrá elegir el tamaño de página o si será fijo.
- Al redactar el prompt, proporciona a Copilot las directrices necesarias para obtener la implementación deseada.
- Quizá tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
