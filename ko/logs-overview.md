---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# 메트릭 개요
{: #logs-overview}

개요 페이지에는 사용자와 어시스턴트 간의 상호작용에 대한 요약이 제공됩니다. 사용자 대화에서 가장 자주 인식된 인텐트 및 엔티티 뿐만 아니라 지정된 시간 동안 트래픽의 양을 볼 수 있습니다.
{: shortdesc}

메트릭을 사용하여 다음과 같은 질문에 응답할 수 있습니다.

* 지난 달에 가장 많거나 가장 적은 수의 대화가 있었던 날은 언제입니까?
* 지난 달 동안 주당 평균 대화 수는 몇 개입니까?
* 지난 주 가장 자주 나타난 인텐트는 무엇입니까?
* 2월 중에 가장 자주 인식된 엔티티 값은 무엇입니까?

메트릭 정보를 보려면 탐색줄에서 **개요**를 선택하십시오.

  ![개요 페이지](images/oview.png)

## 제어
{: #logs-overview-controls}

다음 제어를 사용하여 정보를 필터링할 수 있습니다.

- *인텐트* 및 *엔티티* 필터 - 스킬의 특정 인텐트 또는 엔티티에 대한 데이터를 표시하려면 이러한 드롭 다운 필터 중 하나를 사용하십시오.

  **중요** - 인텐트 및 엔티티 필터는 데이터 소스에 있는 내용이 아니라 ***스킬***의 인텐트 및 엔티티로 채워집니다. 스킬 이외의 [데이터 소스를 선택](/docs/services/assistant?topic=assistant-logs#logs-deploy-id)한 경우 해당 인텐트 및 엔티티도 스킬에 있지 않으면 데이터 소스 로그의 인텐트 또는 엔티티가 필터의 옵션으로 표시되지 않을 수 있습니다. 

- *데이터 새로 고치기* - 개요 페이지 통계를 즉시 새로 고칠 수 있도록 합니다. 개요 페이지에 표시된 데이터가 마지막으로 업데이트된 시기가 표시됩니다. 최신 데이터가 사용 가능한 경우 **데이터 새로 고치기**를 선택할 수 있습니다.

  통계는 어시스턴트와 상호작용한 사용자 또는 API 호출의 외부 트래픽을 표시합니다. 도구의 *시험 사용* 분할창을 통한 상호작용은 포함하지 않습니다. 

- *기간 제어* - 데이터가 표시되는 기간을 선택하려면 이 제어를 사용하십시오. 이 제어는 페이지에 표시된 모든 데이터(그래프로 표시된 대화 수 뿐만 아니라 그래프와 함께 표시된 통계, 그리고 상위 인텐트와 엔티티 목록)에 영향을 줍니다.

  통계는 대화 로그가 보유되는 기간보다 더 긴 기간을 포함할 수 있습니다.
  {: note}

  ![기간 제어](images/oview-time.png)

  하루, 한 주, 한 달 또는 한 분기 동안의 데이터를 볼 것인지 선택할 수 있습니다. 각각의 경우 그래프의 데이터 점은 해당 측정 기간에 맞게 조정됩니다. 예를 들어, 하루 동안의 그래프를 보는 경우 데이터가 시간별 값으로 제공되지만, 한 주 동안의 그래프를 보는 경우에는 데이터가 일별로 표시됩니다. 한 주는 항상 일요일부터 토요일까지입니다. 목요일로 시작해 다음 수요일로 끝나는 한 주나 1일이 아닌 다른 날짜로 시작되는 한 달과 같은 사용자 정의 기간은 작성할 수 없습니다.

  예를 들어, 하루 보기를 선택하는 경우 각 대화에 대해 표시되는 시간은 브라우저의 시간대를 반영하도록 현지화됩니다. API 호출을 통해 동일한 대화 로그를 검토하는 경우에 표시되는 시간소인과 다를 수 있습니다. API 로그 호출은 항상 UTC로 표시됩니다.

    ![기간 제어](images/oview-time2.png)

## 그래프 및 통계
{: #logs-overview-graphs}

몇 가지 통계 스코어카드가 애플리케이션에 대한 로그 데이터를 제공합니다.

* *총 대화 수* - 해당 그래프에 표시된 대로 선택한 기간 동안 활성 사용자와 애플리케이션 간의 총 대화 수입니다.

  단일 대화는 활성 사용자가 애플리케이션에 전송하는 메시지와 애플리케이션이 응답하는 메시지로 구성된 메시지 세트입니다.

  **중요**: '대화'는 애플리케이션/봇에서 전송되거나 수신된 *모든* 메시지 세트로 간주되므로 "Hi, how can I help you?"로 서비스가 시작된 후 사용자가 응답하지 않고 브라우저를 닫은 경우에도 메시지가 총 대화 수에 포함됩니다.

* *대화당 평균 메시지 수* - 해당 그래프에 표시된 대로 선택한 기간 동안 받은 총 메시지 수를 선택한 기간 동안의 총 대화 수로 나눈 값입니다.
* *최대 대화 수* - 지정된 기간 내 단일 데이터 점에 대한 최대 대화 수입니다.
* *낮은 이해도* - 이해도가 낮은 개별 메시지의 수입니다. 이러한 메시지는 인텐트로 분류되지 않으며 알려진 엔티티를 포함하지 않습니다. 잠재적 대화 문제점을 식별하는 데 유용할 수 있습니다.

자세한 그래프는 다음과 같은 추가 정보를 제공합니다.

* *총 대화 수* - 선택한 기간 동안 활성 사용자와 애플리케이션 간의 총 대화 수입니다.

  ***대화*** 그래프를 보는 동안 다음에 표시된 대로 개별 데이터 점을 클릭하여 숫자 값을 볼 수 있습니다.

  ![단일 데이터 점](images/oview-point.png)

* *대화당 평균 메시지 수* - 선택한 기간 동안 받은 총 메시지 수를 선택한 기간 동안의 총 대화 수로 나눈 값입니다.
* *총 메시지 수* - 선택한 시간 동안 활성 사용자로부터 받은 총 메시지 수입니다.
* *활성 사용자 수* - 선택한 기간 내에 애플리케이션을 사용한 고유 사용자 수입니다.
* *사용자당 평균 대화 수* - 선택한 기간 동안의 총 대화 수를 선택한 기간 동안의 총 고유 사용자 수로 나눈 값입니다.

  *활성 사용자 수* 및 *사용자당 평균 대화 수*에 대한 통계에는 고유 `user_id` 매개변수가 필요합니다. 자세한 정보는 [사용자 메트릭 사용](/docs/services/assistant?topic=assistant-logs-resources#logs-resources-user-id)을 참조하십시오.
  {: important}

## 상위 인텐트 및 상위 엔티티
{: #logs-overview-tops}

지정된 기간 동안 가장 자주 인식된 인텐트와 엔티티를 볼 수도 있습니다.

* *상위 인텐트* - 인텐트는 단순 목록에 표시됩니다. 인텐트가 인식된 횟수를 확인하는 것 이외에 인텐트를 선택하여 날짜 범위가 표시된 데이터와 일치하도록 필터링되고 인텐트가 선택한 인텐트와 일치하도록 필터링된 **사용자 대화** 페이지를 열 수 있습니다.

* *상위 엔티티*도 목록에 표시됩니다. 각 엔티티에 대해 **값** 열에서 선택하여 지정된 기간 동안 이 엔티티에 대해 식별된 가장 일반적인 값의 목록을 볼 수 있습니다. 엔티티를 선택하여 날짜 범위가 표시된 데이터와 일치하도록 필터링되고 엔티티가 선택한 엔티티와 일치하도록 필터링된 **사용자 대화** 페이지를 열 수도 있습니다.

서비스에서 인식하는 인텐트 및 엔티티를 검토하여 발견한 내용을 기반으로 인텐트 및 엔티티를 편집하는 방법에 대한 팁은 [대화에서 학습](/docs/services/assistant?topic=assistant-logs)을 참조하십시오.