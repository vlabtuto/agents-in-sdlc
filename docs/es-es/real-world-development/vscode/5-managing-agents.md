---
title: "Ejercicio 5 - Supervisión y administración de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando utilizas el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que ocurren. Puedes revisar cada diff, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios de archivos propuestos.
- pedirás al agente durante la sesión que añada un selector de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un modo claro para las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación, orientarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat junto a un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramienta, lectura y edición de archivos, así como los comandos de terminal que solicita ejecutar.
3. Cuando proponga una edición, el diff aparecerá en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramienta requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba únicamente los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Pide al agente que añada un modo claro

Ya está implementado el modo de alto contraste. Amplía la misma conversación con un selector de modo claro. Como continuarás en la misma sesión, el agente conserva el contexto del trabajo: los componentes modificados, el enfoque de estilo y el patrón de almacenamiento local.

1. Envía este prompt de seguimiento desde Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará los cambios realizados y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada edición nueva a medida que se aplique, como hiciste con el modo de alto contraste. Usa **Keep** o **Undo** para cada diff.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, si intenta refactorizar la interfaz de los selectores de una forma que no te convence—, deténlo con el botón **Stop** junto a la entrada de chat y envía un mensaje de seguimiento que aclare el enfoque.

> [!TIP]
> Es más rápido orientar una sesión activa que empezar de nuevo. Utiliza prompts de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en vez de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de ajustes existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has utilizado el modo de agente de Copilot Chat para supervisar y orientar una sesión activa: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En este ejercicio has aprendido a:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar ediciones concretas.
- guiar una sesión activa con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios del agente local que has utilizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
