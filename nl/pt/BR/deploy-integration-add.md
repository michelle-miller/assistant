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

# Incluindo integrações
{: #deploy-integration-add}

Para implementar sua qualificação, inclua-a em um assistente e, em seguida, inclua integrações no assistente que publicam seu robô nos canais em que seus clientes vão para obter ajuda.
{: shortdesc}

## Como as integrações de plataforma de central de serviço funcionam
{: #deploy-integration-service-desk-integrations}

Assista a este vídeo de 3 minutos para saber mais sobre a integração de seu assistente com uma plataforma de central de serviço, como o Intercom.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Visão geral de como as integrações de central de serviço funcionam" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para saber mais sobre como as integrações de central de serviço com seu assistente podem beneficiar seus negócios, [leia esta postagem do blog ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}.

## Incluir uma integração
{: #deploy-integration-add-task}

Siga estas etapas para criar integrações em seu assistente:

1.  Clique na guia  ** Assistentes ** .

    Se não vir a guia **Assistentes**, clique no link do menu da trilha de navegação no cabeçalho da página.
    {: note}

1.  Clique para abrir o tile para o assistente que você deseja implementar.

1.  Vá para a seção Integrações.

    **O que é a integração do Link de visualização?** Ao criar um assistente, um website de teste é provisionado automaticamente (a menos que você escolha não ativar o link de visualização). Ele tem uma interface de widget de bate-papo simples que pode ser usada para interagir com seu assistente para propósitos de teste. Também é possível compartilhar a URL para este site com a marca IBM com seus membros da equipe.

1.  Clique em **Incluir integração**.

1.  Clique no tile para o canal ao qual você deseja integrar o assistente. As opções incluem:

    - [Aplicativo customizado](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [ Facebook Messenger ](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Somente plano Plus ou Premium](images/plus.png)
    - [ Link de visualização ](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [ Slack ](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Agente de voz (telefonia) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Abre a página de visão geral do serviço **Voice Agent with Watson** no {{site.data.keyword.cloud}}.
    - [Plug-in do WordPress ![Ícone de link externon](../../icons/launch-glyph.svg "Ícone de link externo")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Abre a página de visão geral do plug-in do IBM Watson Assistant no site do WordPress.

1.  Siga as instruções fornecidas na tela para concluir o processo de integração.

Depois de integrar o assistente, teste-o no canal de destino para assegurar-se de que funcione conforme o esperado.

Para obter dicas sobre como iniciar o diálogo de uma maneira consistente em todos os tipos de integração, consulte [Iniciando o diálogo](/docs/services/assistant?topic=assistant-dialog-start).

## Limites de integração
{: #deploy-integration-add-limits}

O número de integrações que podem ser criadas em uma única instância de serviço depende de seu plano do {{site.data.keyword.conversationshort}}.

| Plano de Serviço     | Integrações por assistente |
|------------------|---------------------------:|
| Premium          |                        100 |
| Mais             |                        100 |
| Padrão         |                        100 |
| Plus Trial       |                        100 |
| Lite             |                        100 |
{: caption="Detalhes do plano de serviço" caption-side="top"}
