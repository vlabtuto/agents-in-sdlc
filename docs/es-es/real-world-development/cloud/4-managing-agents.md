---
title: "Ejercicio 4 - Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has asignado tres tareas distintas al agente de Copilot en la nube para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, también es importante supervisar estas tareas.

Hay varias herramientas para administrar las tareas asignadas, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes consultar las tareas de agentes que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas e incluso orientar las sesiones en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo selectores, parecía lógico incluir esta función. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Consulta el estado de las tareas asignadas al agente.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas asignadas a Copilot, entre ellas:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa lo que ha realizado Copilot, por ejemplo:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo.
   - Configurar servidores MCP.
   - Realizar varias operaciones para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Allí puedes consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que ha terminado de prepararse y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro para guiar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot reconoce la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la solicitud nueva cuando termine el paso que estaba realizando al recibir el mensaje.

El trabajo tardará varios minutos. Es un buen momento para hacer una pausa y repasar lo que has aprendido.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de operaciones para supervisar y guiar tareas del agente en la nube. Puedes realizar un seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

Has aprendido a:

- utilizar mission control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
