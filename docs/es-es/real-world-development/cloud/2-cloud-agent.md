---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: incidencias de seguridad pendientes, código heredado que hay que actualizar o solicitudes de funcionalidad relegadas en el backlog por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios para que lo revise una persona. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dedicar más tiempo a los asuntos importantes.

Con el agente de Copilot en la nube:

- personalizarás el entorno de generación de código.
- comprobarás cómo se realizan las operaciones de forma segura.
- aprenderás la importancia de delimitar claramente las incidencias.
- asignarás incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. El primer paso es añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que los visitantes sigan explorando. No necesita una implementación refinada; solo una versión que permita hacer pruebas de aceptación de la experiencia. Este trabajo es un bloqueo, aunque hay otras incidencias con mayor prioridad.

Ambas son tareas que suelen posponerse y que conviene asignar al agente de GitHub Copilot en la nube. El agente las resuelve de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, cuando termina, puede revisar el resultado.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Puedes iniciar el trabajo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Después de la asignación, Copilot creará un borrador de solicitud para registrar su progreso, preparará un entorno y empezará a trabajar. Puedes inspeccionar la sesión mientras está en curso o una vez finalizada. Cuando la solución esté lista para revisión, te mencionará en la solicitud.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar en el problema. Incluso las operaciones que parecen sencillas pueden ser complejas. Por eso, [delimita con cuidado las tareas que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un programador colaborativo con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones, habrás visto `.github/copilot-instructions.md` en funcionamiento; si no, este es un buen momento para revisarlo. Antes de asignar trabajo al agente, lee los archivos de instrucciones que ya incluye este repositorio para poder reconocer su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, fíjate en si la solicitud resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Esas indicaciones proceden de los archivos de instrucciones y volverán a aparecer cuando revises las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente

La creación de código suele requerir un entorno específico y scripts de configuración para garantizar que todo esté preparado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo creando un [flujo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias de desarrollo necesarias. El taller ya incluye la configuración para facilitar el ejercicio: proporciona Node.js, dependencias, binarios de navegador para pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para los pasos. Copilot utilizará su propio token para las operaciones.
    permissions:
      # Si estos pasos clonan el repositorio, por ejemplo para instalar dependencias, se necesita el permiso `contents: read`. Si no se clona, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (toda la aplicación ahora utiliza Astro y Drizzle/libSQL).
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

      # Migra y carga los datos iniciales de la base de datos SQLite local para que la compilación y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto sirve para probarlo sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esa deuda técnica reduce la productividad, dificulta el mantenimiento del código e impide que los nuevos desarrolladores se incorporen al equipo con facilidad. Copilot puede ayudar a crear documentación; por eso, esta es una buena incidencia para el agente de la nube. Trabajará en segundo plano y volverás más adelante para revisar el resultado.

1. En una nueva pestaña del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud nueva, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana.
12. Después de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando su entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo resolver la tarea.

13. Revisa la lista y las tareas previstas.
14. Desplázate hacia abajo por la cronología de la solicitud y busca un mensaje que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot explora el código, analiza el contexto, decide el plan adecuado y crea código.

El trabajo puede tardar varios minutos. Uno de los objetivos principales del agente en la nube es realizar tareas asíncronamente para que puedas ocuparte de otros asuntos. Aprovecharás esa capacidad asignándole otra tarea y, mientras tanto, escribirás código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a los detalles del juego

Una de las ventajas del agente en la nube es que divide el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar demasiado tiempo, podrías dedicar ese tiempo a otro trabajo. Asigna también esta tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nuevas incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo verás una serie de 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en esta segunda solicitud. El agente en la nube funciona de manera parecida a un ingeniero de software: no tienes que supervisarlo constantemente, sino revisar el resultado cuando termine. Ahora crearás y utilizarás agentes personalizados.

## Resumen y pasos siguientes

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona, por ejemplo, para atender deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has aprendido a:

- personalizar el entorno de generación de código.
- comprobar cómo se realizan las operaciones de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y utilizar agentes personalizados. El [agente en la nube también puede utilizar servidores MCP][cloud-agent-mcp] y puede acceder a instrucciones personalizadas, que ya has explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

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
