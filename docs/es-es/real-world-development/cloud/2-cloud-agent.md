---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan dificultades con la deuda técnica. Puede tratarse de problemas de seguridad pendientes, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido ocasión de implementarlas. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así se pueden delegar tareas tediosas y acelerar el desarrollo, lo que permite al equipo centrarse en asuntos de mayor alcance.

En este ejercicio explorarás cómo utilizar el agente de Copilot en la nube para:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- comprender la importancia de delimitar bien las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: verás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Le gustaría que la página de detalles de cada juego mostrara otros títulos de su misma categoría para que la gente pueda seguir explorando. Todavía no necesita una implementación pulida; solo quiere algo que permita probar la experiencia de usuario. Aunque este trabajo está bloqueado, hay incidencias de mayor prioridad.

Estas tareas suelen quedar relegadas y son buenas candidatas para asignarlas al agente de GitHub Copilot en la nube. Este puede trabajar en ellas de forma asíncrona mientras la persona desarrolladora se ocupa de otras tareas y, cuando termina, revisar su trabajo y comprobar que todo está en orden.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Al igual que con una persona del equipo, puedes colaborar de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada la incidencia, Copilot creará un borrador de solicitud de incorporación de cambios para hacer un seguimiento del progreso, preparará un entorno y empezará a trabajar. Puedes examinar la sesión mientras está en curso o una vez terminada. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte la fase de análisis. Hasta las tareas que parecen sencillas pueden tener cierta complejidad, así que [delimita cuidadosamente las tareas que asignes al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a una persona que programa contigo: avanza por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa los archivos de instrucciones incluidos en este repositorio, sin modificarlos, para que después puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace, si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el campo `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la próxima sección, fíjate en los comentarios TSDoc, las convenciones de TypeScript y las cabeceras de comentarios de la solicitud de incorporación de cambios resultante. Estos elementos proceden de los archivos de instrucciones. Volveremos a señalarlos al revisar las PR en una lección posterior.

## Configura el entorno de desarrollo del agente en la nube

Desarrollar código suele requerir un entorno específico y la ejecución de scripts de configuración para dejarlo listo. Lo mismo ocurre al asignar tareas a Copilot, que las realiza de forma parecida a una persona ingeniera de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para trabajar. Puedes personalizar el entorno creando un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así se proporcionan las herramientas y dependencias necesarias. Este archivo ya está configurado para facilitar el taller y la experiencia de aprendizaje: da acceso a Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada e inicializada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para los pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo, para instalar dependencias, necesitarán el permiso `contents: read`. Si no lo clonan, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (la aplicación completa ahora utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e inicializa la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevas personas al equipo. Copilot es especialmente útil para generar documentación, así que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano para crear la documentación necesaria. En un ejercicio futuro volverás para revisar el resultado.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios TSDoc cuando resulten útiles. Lamentablemente, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente para añadir comentarios de documentación cuando ayuden a aclarar el comportamiento.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva solicitud de incorporación de cambios, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo que se ha mencionado), analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que Copilot va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios; debería aparecer una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código, analiza el estado, se detiene para pensar y decidir el plan adecuado y, después, crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente de Copilot en la nube es realizar tareas de forma asíncrona para que puedas ocuparte de otras cosas. Aprovecharemos esa función asignándole otra tarea y, mientras trabaja, nos ocuparemos de añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente de Copilot en la nube es que permite repartir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados quizá no lleve mucho tiempo, podrías dedicarlo a otras tareas. Asignemos este trabajo al agente.

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar algunas pruebas de aceptación. Estos son los requisitos:

   - Añadir en `src/lib/` una función auxiliar de acceso a datos que devuelva otros juegos de la misma categoría que un juego determinado, excluyendo ese juego.
   - Mostrar en la página de detalles una sección «Juegos relacionados» que utilice la función auxiliar.
   - Gestionar el caso en que un juego no tenga títulos relacionados.
   - Crear pruebas unitarias para la nueva función auxiliar.
   - Antes de crear la PR, comprobar que se superan todas las pruebas.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el panel derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, aparecerán unos ojos 👀 en el primer comentario de la incidencia para indicar que Copilot ya está trabajando.

![Copilot utiliza el emoji de los ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube funciona de forma parecida a una persona ingeniera de software: no hace falta que lo supervises continuamente, solo revisa el resultado cuando termine. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Copilot puede ayudarte a resolver deuda técnica, crear funcionalidades o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- garantizar que las operaciones se realicen de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, vamos a crear y utilizar agentes personalizados. [El agente de Copilot en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que hemos visto en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
