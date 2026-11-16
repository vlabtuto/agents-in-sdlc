---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás la publicación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero aún hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y subir código. Revisa la configuración de aprobaciones y el repositorio de destino antes de enviar este prompt. Si está seleccionada la opción **Approve all**, no esperes otro aviso de confirmación.

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, sus desencadenadores y permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que aún no se haya combinado.
4. Solicita las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias antes de autorizar la combinación.

## Autoriza la combinación y la publicación

> [!WARNING]
> El siguiente prompt autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Envíalo solo después de revisar los cambios y confirmar el repositorio y la solicitud. Según la configuración de aprobaciones, estas acciones podrían realizarse sin otro aviso.

Sustituye `<PR_NUMBER>` por el número de la solicitud que has revisado y envía este prompt por separado:

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama en tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Comprueba la publicación

1. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
2. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages ni combinar la solicitud, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que la publicación se haya completado y el sitio esté disponible.

## Resumen y pasos siguientes

Space Quiz ya está disponible como sitio web y has comprobado la publicación en lugar de fiarte únicamente del informe del agente. Continúa con la [lección 13: Repaso y pasos siguientes][next-lesson].

[next-lesson]: ../13-review/
