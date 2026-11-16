---
title: "Lección 8 - Crea y combina la solicitud de incorporación de cambios"
description: "Revisa conjuntamente el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una solicitud de incorporación de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás todo en conjunto y abrirás una solicitud de incorporación de cambios. Tú combinaste la solicitud de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias solicitudes. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta solicitud.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la solicitud de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que la solicitud se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado has utilizado Copilot para planificar, implementar y validar una funcionalidad. Tailspin Toys quiere automatizar el trabajo pendiente de la solicitud, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar los cambios cuando sea necesario. Después, activa la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y decide si está listo antes de activarlo.

## Usa Agent Merge para gestionar la solicitud

Ya has creado todo el código. Revísalo, crea la solicitud y deja que Agent Merge se encargue del resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agentes con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud. Cuando todo se complete correctamente, la combinará.
7. Comprueba que Copilot indique algo parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación que ya has revisado.

## Resumen y pasos siguientes

Has automatizado varios aspectos del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado la solicitud de filtrado.
- has activado Agent Merge después de la revisión y confirmado que la solicitud se ha combinado.

A continuación, [explorarás otros comandos slash de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
