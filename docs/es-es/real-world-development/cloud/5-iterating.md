---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades: has pedido a Copilot que documente el código, añada una sección de juegos relacionados y implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios y, si hace falta, darás indicaciones para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir los procesos habituales de DevOps. Antes de activar la revisión para el resto del equipo, revisarás las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión, por lo que existen medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno temporal e independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios generada por el agente: añadir documentación al código. Utilizarás la interfaz habitual de GitHub.com.

> [!NOTE]
> Es posible que la solicitud muestre una advertencia de que GitHub Copilot está bloqueado por un firewall. Es **normal**: Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud con un título parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba que incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado todo, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es un proceso unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios en línea del código, igual que harías con otra persona del equipo. Copilot leerá los comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay una única solicitud recomendada. Por ejemplo, podrías pedirle que:

- añada al principio de cada archivo de código un comentario breve que explique su función.
- añada comentarios TSDoc a archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**. Puedes utilizar una idea de la lista o pedir otra mejora documental.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Iniciará una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la solicitud.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la solicitud como lista para revisión y combinarla en la rama principal.

![Marcar la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la solicitud que creó Copilot para mostrar juegos relacionados en la página de detalles.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título se parezca a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca un indicador de flujos de trabajo pendientes de aprobación.
7. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos aparecerán en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace y probar manualmente la funcionalidad. Abre un terminal en el Codespace y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ya ha creado la sección de juegos relacionados. Puedes seguir iterando y pedir cambios mediante comentarios en la pestaña **Conversation**. Por ejemplo, puedes modificar el número de juegos que se muestran o pedir que añada encabezados de comentarios y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. Esta solicitud debe incluir el modo de alto contraste del ejercicio 3 y el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Abre el repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la solicitud con un título parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la solicitud tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación, prestando atención a:
   - Los componentes de interfaz para cambiar de modo.
   - El uso del almacenamiento local para guardar las preferencias.
   - Los cambios de CSS o estilo para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como etiquetas ARIA y navegación por teclado.
   - El código JavaScript o TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca un indicador de flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos aparecerán en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de alto contraste y modo claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar las funciones conforme a las prácticas recomendadas de accesibilidad. Si encuentras algún problema o mejora, puedes mencionar a **@copilot** en un comentario y pedirle que actualice los cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle incidencias reales del backlog y ocuparte de otra tarea. Para practicarlo, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario de interés para mecenas en la página de detalles del juego.
- Implementar paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el agente.
- **Has asignado incidencias al agente de GitHub Copilot en la nube** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para accesibilidad, con selectores de alto contraste y modo claro.
- **Has utilizado la página de agentes de Copilot como centro de operaciones** para supervisar y orientar la sesión de accesibilidad.
- **Has revisado las solicitudes de Copilot y has iterado sobre ellas**, mencionando a `@copilot` para pedir cambios y aprobando los flujos de trabajo.

## Repaso y pasos siguientes

Has terminado el itinerario del agente en la nube. Para seguir explorando, puedes probar los otros itinerarios:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, también puedes:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crear un agente personalizado para otra área.
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Administración de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
