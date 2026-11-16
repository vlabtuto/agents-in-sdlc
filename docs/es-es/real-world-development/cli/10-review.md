---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante todo el flujo de trabajo de Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado ya creada.
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios.
- has partido de la incidencia de filtrado, definido el enfoque en modo Plan, desarrollado en modo Autopilot y revisado en modo Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para probar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado todos los cambios del filtrado y autorizado a Agent Merge a combinar la solicitud.
- has explorado comandos slash para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has publicado

El taller incluye dos hitos de solicitudes de incorporación de cambios, cada uno en su propia rama creada desde `main` actualizada:

1. **Valoraciones con estrellas:** muestra en las tarjetas de juegos el valor existente de `starRating` o un estado explícito cuando no hay valoración.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde que empezaste a planificar el filtrado hasta que abriste su solicitud, utilizaste la misma conversación y rama. Combinamos todo ese trabajo en una solicitud para simplificar el taller.

## Distintos tipos de validación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el sitio en un navegador y utilizando Copilot para explorar el navegador mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y registró los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la solicitud.

Las pruebas nuevas deben cubrir carencias reales; si el análisis de QA no requiere ninguna prueba adicional, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para colaborar en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más potente para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* quieres desarrollar, *por qué* y *cómo* influye de forma importante en el resultado.

## Más opciones para explorar

Ya has cubierto el flujo de trabajo principal. Estas funciones de CLI también pueden ser útiles:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para razonar sobre un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Pasos siguientes

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que llevas años queriendo hacer. Comparte lo aprendido con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
