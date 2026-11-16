---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que ese contexto esté disponible. Hay distintas formas de compartir información específica con Copilot; una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos de rutas.
- generarás la primera parte de la capa de datos del filtrado (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.
- volverás a enviar el mismo prompt y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios de instrucciones y la parte de filtrado en `main` para que Copilot utilice las instrucciones actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no cumplir algunos de los estándares que hayas definido. Copilot no es determinista. El objetivo es observar la tendencia de los resultados después de actualizar las instrucciones, no obtener exactamente el mismo código.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys sigue directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe utilizar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Mediante archivos de instrucciones, proporcionarás a Copilot la información necesaria para que realice las tareas conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y requisitos de programación. Puedes especificar convenciones de código, bibliotecas y los tipos de comentarios que quieres incluir. También puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto general, como la tecnología utilizada, una descripción del proyecto, prácticas recomendadas y otras directrices globales.
- `.github/instructions/*.instructions.md`, archivos que proporcionan directrices para lenguajes, tipos de archivos o tareas concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan metadatos `applyTo`.
>
> Copilot también [admite otros estándares de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

El proceso completo para crear archivos de instrucciones queda fuera del alcance de este ejercicio, pero el proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en la orientación general del proyecto: qué se está desarrollando, cómo está estructurado y cuáles son los estándares globales.
- Utiliza archivos `*.instructions.md` para dar directrices específicas sobre tipos de archivos (pruebas unitarias, componentes de Astro o capa de datos) o tareas concretas.
- Escribe en lenguaje natural y presenta las directrices con claridad. Incluye ejemplos de cómo debe y no debe ser el código.

No existe una única forma de crear archivos de instrucciones, del mismo modo que no hay una única manera de utilizar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para el proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían contar con un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás archivos para tareas como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece opciones distintas; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo explicará cuando sea relevante.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y selecciona una rama:

   ```bash
   git checkout -b custom-instructions
   ```

## Utiliza Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea pertinente.

Para ver el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú de agentes de Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al archivo del helper como a sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esta estructura procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el comentario inicial del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia después de actualizar las instrucciones.

## Añade un estándar para todo el repositorio

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade un estándar para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares del proyecto, pero aún no contiene directrices para la documentación en el código.
3. Añade estas líneas de Markdown justo después de los estándares existentes para pedir a Copilot que incluya comentarios iniciales y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear instrucciones para todo el repositorio en `.github/copilot-instructions.md` o instrucciones `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares globales, como la regla sobre los comentarios de documentación.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar la conversación.
2. Selecciona `src/lib/publishers.ts` para centrar el contexto de Copilot en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de guiar a Copilot para que siga un estándar nuevo y lo aplique a código real que utilizarás en el ejercicio siguiente.

## Confirma las instrucciones y sube la rama

Los archivos de instrucciones, como cualquier otro archivo del repositorio, se gestionan mediante el control de código fuente.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que se han aplicado los cambios de instrucciones y del helper:

   ```bash
   git status
   ```

3. Prepara y confirma los cambios en las instrucciones y todos los archivos que Copilot haya modificado para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Ya has subido la rama y ahora debes abrir una solicitud de incorporación de cambios asociada a la incidencia sobre los estándares de documentación del backlog. Copilot puede buscar la incidencia, crear una solicitud que la cierre y, después, combinarla.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub, así que no hace falta preparar nada. La primera vez que Copilot utilice una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las indicaciones. En un ejercicio posterior aprenderás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una solicitud desde la rama actual. Indícale que describa tanto las instrucciones como el nuevo helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud de incorporación de cambios.
5. Cuando Copilot te pida permiso para realizar acciones en GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la solicitud, pídele que la combine y vuelva a la rama principal:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones en GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** si corresponde.

Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar global a `.github/copilot-instructions.md`.
- volver a enviar el mismo prompt y observar cómo el código incorpora el estándar.
- confirmar los cambios de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda utilizarla.

A continuación, [utilizarás el modo de agente][next-lesson] para que Copilot añada una funcionalidad en distintas partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
