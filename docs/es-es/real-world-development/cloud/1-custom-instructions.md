---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información adicional, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda no solo *qué* debe hacer, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos específicos de rutas.
- añadirás un estándar global al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, en este no ejecutarás un prompt antes y después. El agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar sus efectos comparando ambos resultados en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere más adelante.

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
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece opciones distintas; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El ejercicio correspondiente lo explicará cuando sea pertinente.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizados del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Dentro de **Code standards**, observa las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript como `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de código del proyecto, pero aún no exige documentación dentro del código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

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

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube podrá utilizar estas instrucciones cuando trabaje en incidencias asignadas en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main`, puedes omitir los pasos de la rama y confirmar los cambios directamente en esa rama. El flujo con una rama permite practicar un proceso parecido al de un proyecto real.

## Resumen y pasos siguientes

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar global a `.github/copilot-instructions.md`. Verás cómo se aplica cuando revises las solicitudes de incorporación de cambios que genere el agente durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
