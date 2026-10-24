---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**](https://docs.github.com/copilot/concepts/agents/github-copilot-app) es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo con agentes en un espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía una solicitud a través de la actualización de la rama, los comentarios de revisión, las correcciones de integración continua (CI) y la combinación.

El taller sigue un único flujo de trabajo continuo en Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog inicial.
2. Realiza un cambio acotado en las valoraciones por estrellas, revísalo en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, desarrolla la funcionalidad en modo **Autopilot** y revísala en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y utiliza Agent Merge para la segunda PR.
9. Comprueba si Canvas está disponible y, después, usa Database Explorer y crea y prueba un lienzo de clasificación respaldado por el repositorio. Si no están disponibles las capacidades necesarias, registra el impedimento y utiliza la alternativa de solo lectura o continúa con el repaso.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado, junto con las actualizaciones de instrucciones y de la habilidad, el perfil QA y las pruebas. Empieza cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un worktree y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final de Canvas se mantiene en su propia sesión para que puedas centrarte en crear y probar la superficie compartida en lugar de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el valor alternativo para `null`; después, combina la primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de su incidencia, desarróllala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar una habilidad de comprobaciones de calidad][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP desde Customize y examina el comportamiento del filtrado |
| [7. Crear y usar un agente de control de calidad][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crear y combinar la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, usa Agent Merge para la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Usa Database Explorer y, después, crea y prueba un lienzo de clasificación respaldado por el repositorio |
| [10. Repaso y pasos siguientes][ex10] | Resumen | Repasa el flujo de trabajo, los elementos creados y otros recursos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de disponer de:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el ordenador

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller, por lo que permite completar todos los recorridos.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu propio equipo y no en un codespace, la [lección de requisitos previos][ex0] explica cómo instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**. Esta directiva es independiente de la de Copilot CLI.

## Primeros pasos

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
