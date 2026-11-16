---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has empezado por añadir una pequeña funcionalidad al proyecto, pero los cambios mayores requieren un proceso más completo. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a desarrollar lo adecuado de la manera correcta. En esta lección y en las siguientes seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad en la misma conversación y rama. Normalmente se usarían ramas o solicitudes de incorporación de cambios distintas para diferentes tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás la funcionalidad manualmente en un navegador reenviado.

Durante el trabajo también actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la solicitud de incorporación de cambios de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de trabajo no cambia los fundamentos del desarrollo; de hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios.
7. Combinar el código después de que se revise y se superen las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen un patrón parecido al anterior.

Este enfoque ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: trabajas con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la solicitud de incorporación de cambios de las valoraciones con estrellas ya se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o solicitud de incorporación de cambios de GitHub.

## Planifica la funcionalidad de filtrado

Planificar permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de asignarla a Copilot. Para cualquier trabajo complejo, conviene dedicar un tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador debajo del prompt muestre **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a elaborar el plan: primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Puede que Copilot te pregunte cómo debe funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique cualquier parte del plan que no coincida con tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, puedes implementarlo. Deja que Copilot trabaje en la tarea con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión.
2. Comprueba que el indicador debajo del prompt muestre **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al autorizarlo, la implementación puede empezar inmediatamente; revisa el plan primero. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y valida la implementación

Debes revisar el código generado antes de combinarlo, como cualquier otro código. Revisa los cambios y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Comprueba que el indicador ya no muestre **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier fallo.

## Prueba la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría, por editorial y las combinaciones que acordaste en el plan.
4. Confirma que el reinicio de filtros y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y pasos siguientes

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado la funcionalidad manualmente en un navegador reenviado.

A continuación, [aprenderás cómo las instrucciones personalizadas][next-lesson] ayudan a que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
