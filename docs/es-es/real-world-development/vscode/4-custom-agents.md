---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o áreas concretas del flujo de trabajo de desarrollo. Al definir los agentes en archivos Markdown dentro de `.github/agents`, puedes proporcionar a Copilot instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para que realice esas tareas con mayor eficacia. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique las directrices [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` en el proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un prompt en Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos suelen definirse en archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio conforme a las directrices de accesibilidad. Durante ese trabajo podría utilizar habilidades como una dedicada al flujo de solicitudes de incorporación de cambios o a la ejecución y administración de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear agentes personalizados. Como con cualquier tecnología de IA, prueba distintas opciones e itera para encontrar las que mejor se adapten al entorno y a tus situaciones.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a hacer que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para solucionar el problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, quieres implementarlo cuanto antes. Para ello, utilizarás un agente personalizado que genere la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu codespace y, en un terminal, crea una rama nueva desde `main` para el trabajo de accesibilidad. Mantendrás separada la solicitud de filtrado del ejercicio 3.

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las prácticas recomendadas para la accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores habituales y problemas que se deben evitar.

## Utiliza el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para que las instrucciones y herramientas de esa sesión se limiten a las suyas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indicará cuando sea pertinente.

1. En el menú de agentes de Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abre el menú de agentes en la parte inferior de Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía al agente de accesibilidad este prompt:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot hará cambios reales en el repositorio: editará componentes Astro, CSS y las pruebas relacionadas.

En el siguiente ejercicio revisarás y guiarás el trabajo mientras sigue en curso.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y áreas concretas. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio has aprendido:

- cómo se definen los agentes personalizados.
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios a medida que se realicen y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu funcionalidad con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Administración de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
