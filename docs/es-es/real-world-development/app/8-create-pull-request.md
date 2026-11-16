---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, autoriza Agent Merge y confirma que se ha combinado la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias solicitudes. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionarás la solicitud completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el flujo de filtrado has utilizado Copilot para planificar, implementar y validar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la solicitud, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios cuando sea necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa aunque se reinicie la aplicación y se desactiva cuando la solicitud ya está combinada.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad, pero su capacidad para editar código y combinar todavía requiere tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Usa Agent Merge para gestionar la solicitud

Ya has creado y revisado el código. Deja que Agent Merge gestione el proceso de la solicitud.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comienza. Se encargará de:

- Crear la solicitud con un título y una descripción.
- Si la sesión se inició desde una incidencia, incluir una referencia a la incidencia relacionada en el cuerpo de la descripción.
- Reorganizar la base o resolver posibles conflictos con la rama de destino.
- Supervisar el proceso de CI para garantizar que se superen todas las comprobaciones.
- Supervisar la solicitud para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar los cambios necesarios para resolverlos.
- Combinar automáticamente la solicitud cuando todo se haya completado correctamente, si así se autoriza.

Deja que Agent Merge combine la solicitud cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la solicitud y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la solicitud:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el proceso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la operación haya terminado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varios aspectos del desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud.
- has inspeccionado la solicitud completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
