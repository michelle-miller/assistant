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

# 통합 추가
{: #deploy-integration-add}

스킬을 배치하려면 어시스턴트에 추가한 후 고객이 도움을 요청하는 채널에 봇을 공개하는 어시스턴트에 통합을 추가하십시오.
{: shortdesc}

## 서비스 데스크 플랫폼 통합 작업 방법
{: #deploy-integration-service-desk-integrations}

이 3분짜리 비디오는 Intercom과 같은 서비스 데스크 플랫폼으로 어시스턴트를 통합하는 방법에 대해 자세히 설명합니다.

<iframe class="embed-responsive-item" id="youtubeplayer" title="서비스 데스크 통합 작동 방법 개요" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/pJSCZLQVgCY?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

어시스턴트와의 서비스 데스크 통합이 비즈니스에 도움이 되는 방법을 알아보려면 [이 블로그 게시물![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://medium.com/ibm-watson/contact-center-post-394dff427c8){: new_window}을 읽으십시오.

## 통합 추가
{: #deploy-integration-add-task}

어시스턴트에 통합을 추가하려면 다음 단계를 수행하십시오.

1.  **어시스턴트** 탭을 클릭하십시오.

    **어시스턴트** 탭이 표시되지 않으면 페이지 헤더에서 이동 경로 메뉴 링크를 클릭하십시오. {: note}

1.  배치할 어시스턴트에 대한 타일을 클릭하여 여십시오.

1.  통합 섹션으로 이동하십시오.

    **미리보기 링크 통합이란 무엇입니까?** 어시스턴트를 작성할 때 테스트 웹 사이트가 자동으로 프로비저닝됩니다(미리보기 링크를 사용 안함으로 선택하지 않은 경우). 테스트 목적으로 어시스턴트와 상호작용하는 데 사용할 수 있는 단순 대화 위젯 인터페이스가 있습니다. 이 IBM 브랜드 사이트에 대한 URL을 팀 구성원과 공유할 수도 있습니다.

1.  **통합 추가**를 클릭하십시오.

1.  어시스턴트를 통합하려는 채널에 대한 타일을 클릭하십시오. 옵션에는 다음이 포함됩니다.

    - [사용자 정의 애플리케이션](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Plus 또는 Premium 플랜만 해당](images/plus.png)
    - [미리보기 링크](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [음성 에이전트(전화 통신) ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      {{site.data.keyword.cloud}}에서 **Voice Agent with Watson** 서비스 개요 페이지를 엽니다.
    - [WordPress 플러그인 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      WordPress 사이트에서 IBM Watson Assistant 플러그인 개요 페이지를 엽니다.

1.  화면에 제공된 지시사항에 따라 통합 프로세스를 완료하십시오.

어시스턴트를 통합한 후 대상 채널에서 이를 테스트하여 어시스턴트가 예상대로 작동하는지 확인하십시오.

모든 통합 유형에서 일관된 방식으로 대화를 시작하는 방법에 대한 팁은 [대화 시작](/docs/services/assistant?topic=assistant-dialog-start)을 참조하십시오.

## 통합 한계
{: #deploy-integration-add-limits}

단일 서비스 인스턴스에서 작성할 수 있는 통합의 수는 {{site.data.keyword.conversationshort}} 플랜에 따라 다릅니다.

| 서비스 플랜     | 어시스턴트당 통합 수 |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| 표준         |                        100 |
| Plus Trial   |                        100 |
| Lite             |                        100 |
{: caption="서비스 플랜 세부사항" caption-side="top"}
