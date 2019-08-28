---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Adición de integraciones
{: #deploy-integration-add}

Para desplegar su conocimiento, añádalo a un asistente y luego añada integraciones al asistente que publiquen su bot en los canales en los a los que los clientes acuden para obtener ayuda.
{: shortdesc}

## Cómo funcionan las integraciones de la plataforma del centro de servicio
{: #deploy-integration-service-desk-integrations}

Watson: vídeo de 3 minutos para aprender más sobre la integración de su asistente con una plataforma de servicio de atención al cliente, como Intercom.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Descripción general sobre cómo funcionan las integraciones del centro de servicio al usuario" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para obtener información sobre cómo pueden ayudar las integraciones del centro de servicio al usuario a su negocio,
[lea esta publicación de blog ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}.

## Adición de una integración
{: #deploy-integration-add-task}

Siga estos pasos para añadir integraciones al asistente:

1.  Pulse el separador **Asistentes**.

    Si no ve el separador **Asistentes**, pulse el enlace de menú de rastro de navegación en la cabecera de la página.
    {: note}

1.  Pulse para abrir el mosaico correspondiente al asistente que desea desplegar.

1.  Vaya a la sección Integraciones.

    **¿Qué es la integración Enlace de vista previa?** Cuando se crea un asistente, se proporciona automáticamente un sitio web de prueba (a menos que elija no habilitar el enlace de vista previa). Tiene una sencilla interfaz de widget de conversación que puede utilizar para interactuar con su asistente para realizar pruebas. También puede compartir el URL con este sitio de IBM con los miembros de su equipo.

1.  Pulse **Añadir integración**.

1.  Pulse el mosaico correspondiente al canal con el que desea integrar el asistente. Las opciones incluyen:

    - [Aplicación personalizada](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Solo planes Plus o Premium](images/plus.png)
    - [Enlace de vista previa](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Agente de voz (Telefonía)  ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Abre la página de visión general del servicio **Voice Agent with Watson** en {{site.data.keyword.cloud}}.
    - [Plugin de WordPress ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Abre la página de visión general del plugin IBM Watson Assistant en el sitio de WordPress.

1.  Siga las instrucciones que se proporcionan en la pantalla para completar el proceso de integración.

Después de integrar el asistente, pruébelo desde el canal de destino para asegurarse de que funciona según lo esperado.

Para ver sugerencias sobre cómo iniciar el diálogo de forma coherente en todos los tipos de integración, consulte [Inicio del diálogo](/docs/services/assistant?topic=assistant-dialog-start).

## Límites de integraciones
{: #deploy-integration-add-limits}

El número de integraciones que puede crear en una sola instancia de servicio depende del plan de {{site.data.keyword.conversationshort}}.

| Plan de servicio     | Integraciones por asistente |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Estándar         |                        100 |
| Plus Trial       |                        100 |
| Lite             |                        100 |
{: caption="Detalles del plan de servicio" caption-side="top"}
