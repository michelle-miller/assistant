---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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
{:table: .aria-labeledby="caption"}

# 대화 빌드
{: #dialog-build}

대화는 고객이 원하는 것을 기반으로 어시스턴트가 고객에 대해 제공하는 응답을 정의합니다.
{: shortdesc}

## 대화 빌드
{: #dialog-build-task}

대화를 작성하려면 다음 단계를 완료하십시오.

1.  **대화** 탭을 클릭한 후 **대화 작성**을 클릭하십시오.

    처음에 대화 편집기를 열 때 다음 노드가 작성됩니다.

    - **Welcome**: 첫 번째 노드입니다. 처음 어시스턴트를 사용할 때 사용자에게 표시되는 인사가 포함됩니다. 인사를 편집할 수 있습니다.

    이 노드는 사용자가 시작한 대화 플로우에서 트리거되지 않습니다. 예를 들어, `welcome` 특별 조건을 사용하는 Facebook 또는 Slack 건너뛰기 노드와 같은 채널과의 통합에 사용된 대화입니다. 자세한 내용은 [대화 초기화](/docs/services/assistant?topic=assistant-dialog-start)를 참조하십시오.
    {: note}

    - **Anything else**: 최종 노드입니다. 입력이 인식되지 않을 때 사용자에게 응답하는 데 사용되는 구가 포함됩니다. 제공된 응답을 바꾸거나 유사한 의미의 응답을 추가하여 대화에 다양성을 추가할 수 있습니다. 또한 어시스턴트가 정의된 각 응답을 차례로 리턴할지 아니면 임의 순서로 리턴할지를 선택할 수 있습니다.
1.  대화 트리에 노드를 추가하려면 **Welcome** 노드에서 **추가**(![추가 아이콘](images/kabob.png)) 아이콘을 클릭한 다음 **아래에 노드 추가**를 선택하십시오.
1.  **If assistant recognizes** 필드에 충족될 때 어시스턴트가 노드를 처리하도록 트리거하는 조건을 입력하십시오. 

    시작하려면 일반적으로 인텐트를 조건으로 추가해야 합니다. 예를 들어, 여기에 `#open_account`를 추가하면 사용자 입력이 계정을 열겠다는 것을 나타내는 경우, 이 노드에서 지정할 응답을 사용자에게 리턴할 것임을 의미합니다. 

    조건 정의를 시작하면 옵션을 표시하는 상자가 표시됩니다. 다음 문자 중 하나를 입력한 다음 표시되는 옵션 목록에서 값을 선택할 수 있습니다.

    <table>
    <caption>조건 빌더 구문</caption>
    <tr>
      <th>문자</th>
      <th>다음 아티팩트 유형의 정의된 값 나열</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>인텐트</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>엔티티</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>{entity-name} 값</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>대화의 다른 위치에서 정의하거나 참조한 컨텍스트 변수</td>
    </tr>
    </table>

    이러한 옵션을 사용하는 새 조건을 정의하여 인텐트, 엔티티, 엔티티 값 또는 컨텍스트 변수를 새로 작성할 수 있습니다. 이러한 방식으로 아티팩트를 작성하는 경우 아티팩트를 완전하게 작성하는 데 필요한 다른 단계로 돌아가 이를 완료해야 합니다(예: 인텐트를 위한 샘플 발화(utterance) 정의).

    둘 이상의 조건에 따라 트리거되는 노드를 정의하려면 하나의 조건을 입력한 다음 그 옆에 있는 더하기 부호(+) 아이콘을 클릭하십시오. 다중 조건에 `AND` 대신 `OR` 연산자를 적용하려면 필드 사이에 표시되는 `and`를 클릭하여 연산자 유형을 변경하십시오. AND 연산은 OR 연산 앞에서 실행되지만 소괄호를 사용하여 순서를 변경할 수 있습니다. 예를 들어, `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`입니다.

    정의하는 조건의 길이는 2,048자 미만이어야 합니다.

    조건에서 값을 테스트하는 방법에 대한 자세한 정보는 [조건](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-conditions)을 참조하십시오.
1.  **선택사항**: 이 노드에서 사용자로부터 여러 정보를 수집하려면 **사용자 정의**를 클릭하고 **슬롯**을 사용하십시오. 세부사항은 [슬롯을 사용하여 정보 수집](/docs/services/assistant?topic=assistant-dialog-slots)을 참조하십시오.
1.  응답을 입력하십시오.
    - 어시스턴트가 사용자에게 응답으로 표시할 텍스트 또는 멀티미디어 요소를 추가하십시오.
    - 특정 조건에 따라 다른 응답을 정의하려면, **사용자 정의**를 클릭하고 **다중 응답**을 사용하십시오.
    - 조건부 응답, 서식이 있는 응답 또는 응답에 다양성을 추가하는 방법에 관한 정보는 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)을 참조하십시오.

1.  현재 노드가 처리된 후 수행할 작업을 지정하십시오. 다음 옵션 중에서 선택할 수 있습니다.

    - **사용자 입력 대기**: 사용자가 새 입력을 제공할 때까지 어시스턴트를 일시정지합니다.
    - **사용자 입력 건너뛰기**: 어시스턴트가 첫 번째 하위 노드로 직접 점프합니다. 이 옵션은 현재 노드가 적어도 하나의 하위 노드를 가질 경우에만 사용 가능합니다.
    - **점프**: 어시스턴트는 사용자가 지정한 노드를 처리하여 대화를 계속 진행합니다. 어시스턴트가 대상 노드의 조건을 평가할지 또는 대상 노드의 응답으로 직접 건너 뛸지 여부를 선택할 수 있습니다. 세부사항은 [조치로 점프 구성](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to-config)을 참조하십시오.

1.  **선택사항**: 런타임 시 노드 선택사항 세트를 표시할 때 이 노드를 고려하고 이 노드가 목표에 가장 적합한 항목을 선택하도록 요청하는 경우, 이 노드가 처리하는 사용자 목표의 간단한 설명을 **외부 노드 이름** 필드에 추가하십시오. 예를 들어 *계정 열기*가 있습니다.

    ![Plus 또는 Premium 플랜만 해당](images/plus.png) *외부 노드 이름* 필드는 Plus 또는 Premium 플랜 사용자에게만 표시됩니다. 자세한 내용은 [모호성 제거](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)를 참조하십시오.

1.  **선택사항**: 노드의 이름을 지정하십시오.

    대화 노드 이름에는 문자(유니코드), 숫자, 공백, 밑줄, 하이픈 및 마침표가 포함될 수 있습니다.

    노드 이름을 지정하면 쉽게 용도를 기억할 수 있고 최소화되었을 때도 노드를 찾을 수 있습니다. 이름을 제공하지 않으면 노드 조건이 이름으로 사용됩니다.

1.  노드를 추가하려면 트리에서 노드를 선택한 다음 **추가**(![추가 아이콘](images/kabob.png)) 아이콘을 클릭하십시오.
    - 기존 노드의 조건이 충족되지 않는 경우 다음에 검사하는 피어 노드를 작성하려면 **아래에 노드 추가**를 선택하십시오.
    - 기존 노드의 조건을 검사하기 전에 검사하는 피어 노드를 작성하려면 **위에 노드 추가**를 선택하십시오.
    - 선택한 노드에 하위 노드를 작성하려면 **하위 노드 추가**를 선택하십시오. 하위 노드는 상위 노드 다음에 처리됩니다.
    - 현재 노드를 복사하려면, **복제**를 선택하십시오.

    대화 노드가 처리되는 순서에 대한 자세한 정보는 [대화 개요](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow)를 참조하십시오.
1.  빌드할 때 대화를 테스트하십시오.
   자세한 정보는 [대화 테스트](#dialog-build-test)를 참조하십시오.

## 대화 테스트
{: #dialog-build-test}

대화를 변경하면 언제든지 이 대화를 테스트하여 대화가 입력에 응답하는 방식을 볼 수 있습니다.

"시험 사용" 분할창을 통해 제출하는 조회는 `/message` API 호출을 생성하지만, 로그되지 않으며, 요금이 부과되지 않습니다.

1.  대화 탭에서 ![연습](images/ask_watson.png) 아이콘을 클릭하십시오.
1.  대화 분할창에서 몇 가지 텍스트를 입력한 다음 Enter를 누르십시오.

    시스템이 대화 테스트를 시작하기 전에 최신 변경사항에 대한 훈련을 완료했는지 확인하십시오. 시스템이 여전히 훈련 중인 경우 *시험 사용* 분할창에 메시지가 표시됩니다.
    {: tip}

    ![훈련 메시지 화면 캡처](images/training.png)
1.  대화가 올바르게 입력을 해석했고 적절한 응답을 선택했는지 보려면 응답을 확인하십시오.

    대화 창은 입력에서 인식된 인텐트와 엔티티를 표시합니다.

    ![테스트 대화 출력의 화면 캡처](images/test_dialog_output.png)

    입력에서 인식되는 엔티티를 편집하려면 엔티티 이름을 클릭하여 엔티티 페이지에서 여십시오. 잘못된 인텐트가 인식되면, 인텐트 이름 옆에 있는 화살표를 클릭하여 이를 정정하거나 주제를 관련 없음으로 표시할 수 있습니다. 자세한 정보는 [훈련 데이터 개선](/docs/services/assistant?topic=assistant-logs#logs-fix-data)을 참조하십시오.

1.  대화 트리의 어느 노드가 응답을 트리거했는지 알고 싶은 경우, 그 옆에 있는 **위치** ![위치](images/location.png) 아이콘을 클릭하십시오. 대화 탭에 아직 없는 경우, 대화 탭을 여십시오.

    소스 노드에 초점이 맞춰지고 어시스턴트가 트리를 통과하여 이동한 경로가 강조 표시됩니다. 새 테스트 사항을 입력하는 것과 같은 다른 조치를 수행할 때까지 강조 표시됩니다.

1.  컨텍스트 변수 값을 검사하거나 설정하려면 **컨텍스트 관리** 링크를 클릭하십시오.

    대화에서 정의한 컨텍스트 변수가 표시됩니다.

    또한 `$timezone` 컨텍스트 변수가 나열됩니다. *시험 사용* 분할창 사용자 인터페이스는 웹 브라우저에서 사용자 로케일 정보를 가져오고 이 정보를 사용하여 `$timezone` 컨텍스트 변수를 설정합니다. 이 컨텍스트 변수를 사용하면 테스트 대화 교환에서 시간 참조를 쉽게 처리할 수 있습니다. 사용자 애플리케이션에서 유사한 작업을 고려하십시오. 지정되지 않은 경우 그리니치 평균시(GMT)가 사용됩니다.

    변수를 추가하고 해당 값을 설정하여 다음 테스트 대화 턴(turn)에서 대화가 응답하는 방식을 볼 수 있습니다. 이 기능은 예를 들어, 사용자가 제공한 컨텍스트 변수값에 따라 다른 응답을 표시하도록 대화를 설정하는 경우에 유용합니다.

    1.  컨텍스트 변수를 추가하려면 변수 이름을 지정하고 **Enter**를 누르십시오.
    1.  컨텍스트 변수의 기본값을 정의하려면 목록에 추가한 컨텍스트 변수를 찾은 다음 이 변수에 맞는 값을 지정하십시오.

    자세한 정보는 [컨텍스트 변수](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context)를 참조하십시오.

1.  대화와 계속 상호작용하여 대화가 이를 통해 플로우되는 방법을 확인하십시오.
    - 테스트 발화를 찾아 다시 제출하기 위해 위로 키를 눌러 최근 입력을 모두 볼 수 있습니다.
    - 대화 분할창에서 이전 테스트 발화를 제거하고 다시 시작하려면 **지우기** 링크를 클릭하십시오. 테스트 발화 및 응답이 제거될 뿐만 아니라 이 조치가 대화와 상호작용의 결과로 설정된 컨텍스트 변수값을 모두 지웁니다.

### 다음에 수행할 작업
{: #dialog-build-next-steps}

잘못된 인텐트 또는 엔티티를 인식하고 있는지 판별하는 경우 인텐트 또는 엔티티 정의를 수정해야 합니다.

올바른 인텐트와 엔티티가 인식되지만 대화에서 잘못된 노드가 트리거되는 경우 조건이 올바르게 작성되었는지 확인하십시오.

시작할 때 도움이 되는 팁은 [대화 빌드 팁](/docs/services/assistant?topic=assistant-dialog-tips)을 참조하십시오.

대화를 작업에 사용할 준비가 된 경우, 메시징 플랫폼 또는 사용자 정의 애플리케이션과 어시스턴트를 통합하십시오. [통합 추가](/docs/services/assistant?topic=assistant-deploy-integration-add)를 참조하십시오.

## 대화 노드 한계
{: #dialog-build-node-limits}

스킬당 작성할 수 있는 대화 노드의 수는 플랜 유형에 따라 다릅니다.

| 플랜     | 스킬당 대화 노드 수     |
|------------------|---------------------------:|
| Premium          |                    100,000 |
| Plus             |                    100,000 |
| 표준         |                    100,000 |
| Plus Trial   |             25,000 |
| Lite             |                     100`*` |
{: caption="플랜 세부사항" caption-side="top"}

트리에서 미리 채워져 있는 welcome 및 anything_else 대화 노드는 총계 계산 시 포함됩니다.

트리 깊이 한계: 대화는 2,000개의 대화 노드 하위를 지원합니다. 대화는 20개 이하에서 가장 잘 작동합니다.

`*` 한계가 2018년 12월 1일 25,000에서 100으로 변경되었습니다. 제한이 변경되기 전에 작성된 서비스 인스턴스의 사용자는 2019년 6월 1일까지 플랜을 업그레이드하거나 기존 서비스 인스턴스의 스킬에 있는 대화를 편집하여 새 한계 요구사항을 충족시켜야 합니다.

대화 스킬에서 대화 노드 수를 보려면 다음 중 하나를 수행하십시오.

- 어시스턴트와 이미 연관되지 않은 경우에는 대화 스킬을 어시스턴트에 추가한 후 어시스턴트의 기본 페이지에서 해당 스킬 타일을 확인하십시오. *훈련된 데이터* 섹션에는 대화 노드 수가 나열됩니다.
- GET 요청을 /dialog_nodes API 엔드포인트에 보낸 후 `include_count=true` 매개변수를 포함합니다. 예:

  ```curl
  curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
  ```

  응답에서, `pagination` 오브젝트의 `total` 속성에는 대화 노드 수가 포함됩니다.

총계가 예상보다 많은 것 같은 경우, 애플리케이션에서 빌드하는 대화가 JSON 오브젝트로 변환되기 때문일 수 있습니다. 단일 노드의 일부로 표시되는 일부 필드는 실제로는 기본 JSON 오브젝트에서 별도의 대화 노드로 구성됩니다.

  - 각 노드 및 폴더는 자체 노드로 표시됩니다.
  - 단일 대화 노드와 연관된 각 조건부 응답은 개별 노드로 표시됩니다.
  - 슬롯이 있는 노드의 경우 각 슬롯, 슬롯 찾음 응답, 슬롯 찾을 수 없음 응답, 슬롯 핸들러 및 "모든 정보 프롬프트" 응답(설정된 경우)이 개별 노드입니다. 사실상 세 개의 슬롯이 있는 하나의 노드는 11개의 대화 노드와 동일할 수 있습니다.

## 대화 검색
{: #dialog-build-search}

대화를 검색하여 지정된 단어 또는 구문을 멘션하는 하나 이상의 대화 노드를 찾을 수 있습니다.

1.  검색 아이콘 ![검색 아이콘](images/search_icon.png)을 선택하십시오.

1.  검색어 또는 구를 입력하십시오.

    처음 검색할 때 인덱스가 작성됩니다. 대화 노드의 텍스트가 인덱싱되는 동안 대기하라는 메시지가 표시될 수 있습니다.
    {: note}

검색어를 포함한 노드가 해당 예제와 함께 표시됩니다. 편집하기 위해 열려면 결과를 선택하십시오.

  ![인텐트 검색 리턴](images/search_dialog.png)

## 해당 노드 ID로 대화 노드 찾기
{: #dialog-build-get-node-id}

노드 ID로 대화 노드를 검색할 수 있습니다. 전체 노드 ID를 검색 필드에 입력하십시오. 다음 이유로 알려진 노드 ID와 연관된 대화 노드를 찾을 수 있습니다.

- 로그를 검토하며, 로그는 해당 노드 ID로 대화 섹션을 참조합니다.
- 대화 트리에서 볼 수 있는 노드에 API 메시지 출력의 `nodes_visited` 특성에 나열된 노드 ID를 맵핑하려고 합니다.
- 대화 런타임 오류 메시지가 구문 오류에 대해 알리며, 노드 ID를 사용하여 수정해야 하는 노드를 식별합니다.

노드 ID를 기반으로 하여 노드를 발견하는 또 다른 방법은 다음과 같은 단계를 따릅니다.

1.  대화 탭을 사용하여 대화 트리에서 노드를 선택하십시오.
1.  현재 노드에 대해 열려 있는 경우 편집 보기를 닫으십시오.
1.  웹 브라우저의 위치 필드에 다음 구문이 있는 URL이 표시됩니다.

    `https://assistant-location.watsonplatform.net/location/instance-id/workspaces/workspace-id/build/dialog#node=node-id`

1.  현재 `node-id` 값을 찾을 노드의 ID로 바꾸고 URL을 편집한 다음 새 URL을 제출하십시오.
1.  필요한 경우 편집된 URL을 다시 강조표시하고 다시 제출하십시오.

페이지가 새로 고쳐지고 대화 노드에 대한 초점을 지정한 노드 ID로 바꿉니다. 노드 ID가 슬롯, 찾음 또는 찾을 수 없음 슬롯 조건, 슬롯 핸들러, 슬롯 핸들러 또는 조건부 응답인 경우, 슬롯 또는 조건부 응답이 정의된 노드에 초점이 맞춰지고 해당 모달이 표시됩니다.

계속 노드를 찾을 수 없는 경우, 대화 스킬을 내보내고 JSON 편집기를 사용하여 스킬 JSON 파일을 검색할 수 있습니다.
{: tip}

## 대화 노드 복사
{: #dialog-build-copy-node}

노드를 복제하여 대화 트리 바로 아래에 피어 노드로 정확히 해당 노드의 복사본을 생성할 수 있습니다. 복사된 노드 자체에는 원래 노드와 동일한 이름이 부여되지만, 여기에 `- copy`*`n`*이 추가됩니다. 여기서 *`n`*은 1부터 시작하는 숫자입니다. 동일한 노드를 두 번 이상 복제하면 이름의 *`n`*이 각 사본마다 하나씩 증가하여 사본을 서로 구별하는 데 도움이 됩니다. 노드에 이름이 없는 경우, `copy`*`n`*이라는 이름이 부여됩니다.

사용자가 하위 노드를 포함하는 노드를 복제하는 경우 하위 노드도 함께 복제됩니다. 복사된 하위 노드는 원래 하위 노드와 정확히 동일한 이름을 가집니다. 원래의 하위 노드로부터 복사된 하위 노드를 구별하는 유일한 방법은 상위 노드 이름의 `copy` 참조입니다.

1.  복사하려는 노드에서, **추가** ![추가 아이콘](images/kabob.png) 아이콘을 클릭한 다음 **복제**를 선택하십시오.
1.  복사된 노드의 이름을 바꾸거나 조건을 편집하여 다른 노드와 구분하십시오.

## 대화 노드 이동
{: #dialog-build-move-node}

작성하는 각 노드를 대화 트리의 다른 위치로 이동할 수 있습니다.

이전에 작성된 노드를 플로우의 다른 영역으로 이동하여 대화를 변경할 수 있습니다. 다른 분기의 동위 또는 피어가 되도록 노드를 이동할 수 있습니다.

1.  이동할 노드에서 **추가**(![추가 아이콘](images/kabob.png)) 아이콘을 클릭한 다음 **이동**을 선택하십시오.
1.  이 노드를 이동할 위치에서 가까운 트리에 있는 대상 노드를 선택하십시오. 이 노드를 대상 노드 앞이나 뒤에 배치할지 또는 대상 노드의 하위 노드로 만들지 선택하십시오.

## 폴더가 있는 대화 구성
{: #dialog-build-folders}

대화 노드를 폴더에 추가하여 함께 그룹화할 수 있습니다. 노드를 그룹화해야 하는 다양한 이유는 다음과 같습니다.

- 유사한 주제를 처리하는 노드를 함께 유지하면 쉽게 찾을 수 있습니다. 예를 들어, *사용자 계정* 폴더의 사용자 계정에 대한 질문을 처리하는 노드와 *지불* 폴더의 지불 관련 쿼리를 처리하는 노드를 그룹화할 수 있습니다.
- 특정 조건이 충족되는 경우에만 대화에서 처리할 노드 세트를 함께 그룹화합니다. 예를 들어, `$isPlatinumMember`와 같은 조건을 사용하면 현재 사용자가 추가 서비스를 받을 자격이 있는 경우에만 처리해야 하는 추가 서비스를 제공하는 노드를 그룹화할 수 있습니다.
- 작업 중에 런타임에서 노드를 숨기려면, 조건이 `false`인 폴더에 노드를 추가하여 노드가 처리되지 않도록 할 수 있습니다.

폴더의 다음 특성은 폴더의 노드가 처리되는 방식에 영향을 미칩니다.

- 조건: 조건이 지정되지 않은 경우 어시스턴트에서 폴더 내의 노드를 직접 처리합니다. 조건이 지정된 경우, 어시스턴트는 먼저 서비스 내의 노드를 처리할지 여부를 판별하기 위해 폴더 조건을 평가합니다.
- 사용자 정의: 폴더에 적용하는 모든 구성 설정은 폴더의 노드에 상속됩니다. 예를 들어, 폴더의 다이그레션(digression) 설정을 변경하면, 폴더의 모든 노드가 변경 내용을 상속합니다.
- 트리 계층: 폴더의 노드는 폴더가 루트 또는 하위 레벨의 대화 트리에 추가되는지 여부에 따라 루트 또는 하위 노드로 간주됩니다. 루트 레벨 폴더에 추가하는 루트 레벨 노드는 계속 루트 노드로 작동합니다. 예를 들어 폴더의 하위 노드가 되지 않습니다. 그러나 루트 레벨 노드를 다른 노드의 하위 폴더로 이동하는 경우 루트 노드가 다른 노드의 하위 노드가 됩니다.

폴더는 노드가 평가되는 순서에 영향을 주지 않습니다. 노드는 처음부터 마지막까지 계속 처리됩니다. 어시스턴트가 트리를 따라 이동할 때 폴더를 발견하는 경우 폴더 조건이 없거나 조건이 true이면 폴더의 첫 번째 노드를 즉시 처리하고 거기서부터 순서대로 계속 처리합니다. 폴더에 폴더 조건이 없는 경우, 폴더는 어시스턴트에 대해 투명하며 폴더의 각 노드는 트리의 다른 개별 노드처럼 처리됩니다.

### 폴더 추가
{: #dialog-build-folders-add}

대화 트리에 폴더를 추가하려면 다음 단계를 완료하십시오.

1.  **대화** 탭의 트리 보기에서, **폴더 추가**를 클릭하십시오.

    폴더는 **Anything else** 노드 바로 앞에 있는 대화 트리 끝에 추가됩니다. 트리의 기존 노드를 선택하지 않은 경우에는 선택한 노드 아래에 추가됩니다.

    트리의 다른 위치에 폴더를 추가하려면 노드를 추가하려는 지점 위에서 **추가**![추가 아이콘](images/kabob.png) 아이콘을 클릭한 다음 **폴더 추가**를 선택하십시오.

    기존 대화 분기 내의 하위 노드 아래에 폴더를 추가할 수 있습니다. 이를 수행하려면, 하위 노드에서 **추가** ![추가 아이콘](images/kabob.png) 아이콘을 클릭하고 **폴더 추가**를 선택하십시오.

    폴더가 편집 보기에서 열립니다.

1.  **선택사항**: 폴더 이름을 지정하십시오.

1.  **선택사항**: 폴더에 대한 조건을 정의하십시오.

    조건을 지정하지 않은 경우, `true`가 사용되고 폴더의 노드가 항상 처리됨을 의미합니다.

1.  폴더에 대화 노드를 추가하십시오.

    - 폴더에 기존 대화 노드를 추가하려면, 한번에 하나씩 폴더로 이동해야 합니다.

      이동하려는 노드에서 **추가** ![추가 아이콘](images/kabob.png) 아이콘을 클릭하고 **이동**을 선택한 다음 폴더를 클릭하십시오. 이동 대상으로 **대상 폴더**를 선택하십시오.

      노드를 이동하면 폴더 내의 트리 시작 부분에 추가됩니다. 따라서, 예를 들어, 연속적인 루트 대화 노드의 순서를 유지하려면 마지막 노드부터 먼저 시작하여 이동하십시오.
      {: tip}

    - 폴더에 새 대화 노드를 추가하려면, 폴더에서 **추가** ![추가 아이콘](images/kabob.png) 아이콘을 클릭한 다음, **폴더에 노드 추가**를 선택하십시오.

      대화 노드는 폴더 내에서 대화 트리의 끝에 추가됩니다.

### 폴더 삭제
{: #dialog-build-folders-delete}

폴더만 단독으로 삭제하거나 폴더와 폴더 내의 모든 대화 노드를 삭제할 수 있습니다.

폴더를 삭제하려면 다음 단계를 완료하십시오.

1.  **대화** 탭의 트리 보기에서, 삭제하려는 폴더를 찾으십시오.

1.  폴더에서 **추가**![추가 아이콘](images/kabob.png) 아이콘을 클릭한 다음 **삭제**를 선택하십시오.

1.  다음 중 하나를 수행하십시오.

    - 폴더만 삭제하고 폴더에 있는 대화 노드를 유지하려면 **폴더 내의 노드 삭제** 확인란의 선택을 해제한 다음, **예, 삭제하십시오**를 클릭하십시오.
    - 폴더와 폴더에 있는 모든 대화 노드를 삭제하려면, **예, 삭제하십시오**를 클릭하십시오.

폴더만 삭제한 경우 폴더에 있던 노드가 대화 트리에서 폴더가 위치했던 지점에 표시됩니다.
