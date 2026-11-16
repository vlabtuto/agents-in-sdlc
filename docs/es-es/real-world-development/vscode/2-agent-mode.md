---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El proceso habitual consiste en localizar los archivos necesarios, aplicar los cambios, ejecutar las pruebas, depurar, encontrar el archivo que faltaba y volver a modificarlo. La lista puede ser interminable.

Para eso sirve el modo de agente de Copilot.

El modo de agente puede trabajar de forma más autónoma en el IDE. Se comporta como un desarrollador: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. A continuación, utilizarás este modo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- utilizar el modo de agente de Copilot para explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente antes de combinarlos.

## Escenario

A medida que crece la lista de juegos, quieres que se puedan filtrar por categoría y editorial. En el ejercicio anterior ya creaste un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, explora el sitio de Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM en su frontmatter; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/` y la interactividad se implementa con scripts `<script>` pequeños y específicos de Astro que usan las API DOM estándar.

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro.

1. Vuelve a tu codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, seleccionar un enlace `localhost` en el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor web que ahora se ejecuta en el codespace.

### Explora el sitio

Una vez iniciado el sitio, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea pertinente.

El sitio funciona, pero hay varias mejoras posibles. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla se generó automáticamente un backlog de incidencias; pide a GitHub Copilot que te muestre esos elementos.

1. En el menú de agentes de Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores no expresan una preferencia por un modelo concreto. Durante la elaboración del taller se utilizó Claude Sonnet 4.5 para que las sugerencias fueran relativamente uniformes y la experiencia resultara predecible. Como los modelos de lenguaje no son deterministas, es normal que las sugerencias varíen.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje, Copilot podría utilizar un comando MCP diferente, pero debería completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las directrices generales, entre ellas:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para varios tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado hay que hacer al menos tres cambios en la aplicación:

- Añadir o mejorar la lógica de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de la interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú de agentes.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad descrita en la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes para la funcionalidad, como los helpers de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código base. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar habrá una opción **Keep** o **Undo** para cada cambio; no hace falta seleccionarlos mientras Copilot siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas e incluso que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Sesión completa de Copilot en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

Has utilizado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio de Tailspin Toys. En este ejercicio has aprendido a:

- utilizar el modo de agente para implementar funcionalidades en la interfaz y la capa de datos.
- explorar el proyecto, localizar archivos pertinentes y coordinar cambios.
- revisar los cambios y las pruebas generados antes de combinarlos.

A continuación, [probarás la funcionalidad con Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza lo que has aprendido para pedir a Copilot que la implemente. Ten en cuenta lo siguiente:

- Sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- Decide cómo debe funcionar la paginación y si el usuario podrá seleccionar el tamaño de página o será fijo.
- En el prompt, proporciona a Copilot las directrices necesarias para implementar la solución que quieres.
- Puede que tengas que iterar con Copilot, pedir cambios y proporcionar contexto adicional. Es el proceso habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
