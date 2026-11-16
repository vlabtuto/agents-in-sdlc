---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Has empezado por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y garantizar que se desarrolle lo adecuado de la forma correcta. En esta lección y en las siguientes seguirás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de la funcionalidad. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero en este taller simplificarás el proceso para centrarte en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva desde una incidencia de GitHub.
- definirás los requisitos en modo **Plan**.
- implementarás la funcionalidad en modo **Autopilot**.
- revisarás el código.
- validarás manualmente la funcionalidad en un lienzo de navegador.

Durante el trabajo también actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan acotar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de programar. Utilizarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los fundamentos. De hecho, adquieren aún más importancia. La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Abrir una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y se supere el proceso de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen un patrón parecido al anterior.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** determina el nivel de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo de prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu rama `main` local está actualizada.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, en la navegación superior del repositorio, selecciona **Issues**. Abre **Allow users to filter games by category and publisher**. Es una incidencia que ya incluye la plantilla; no hace falta crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en tu terminal, pero no en la sesión, comprueba la variable `PATH` o la configuración del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones; no reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Solicita cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de suponer que la aprobación lo cambia. Confirma que el indicador muestra **Autopilot**.

Copilot comenzará a implementar la funcionalidad. Verás cómo sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan primero. Si Copilot informa de dependencias ausentes o de un conflicto de puerto, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y valida la implementación

Debes revisar el código antes de combinarlo, como cualquier otro código. Revisa los cambios y ejecuta el sitio para comprobar que todo funciona.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Inspecciona los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local. Asegúrate de que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Pide a Copilot que inicie la aplicación y abra la página en el lienzo:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados coinciden con ella.
4. Combina filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer toda la lista.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo iniciado para esta sesión:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

No crees ni combines aún la PR del filtrado. Conserva esta sesión, su árbol de trabajo y su rama para las lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código de filtrado y añadirás mejoras de habilidades y QA antes de crear y combinar la PR conjunta en la lección 8. Después de detener el servidor, puedes dejar la sesión inactiva y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una sesión de agente nueva desde una incidencia de GitHub.
- has definido los requisitos en modo **Plan**.
- has implementado la funcionalidad en modo **Autopilot**.
- has revisado el código.
- has validado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
