---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 配置 {{site.data.keyword.conversationshort}} 工作區

{{site.data.keyword.conversationshort}} 服務的自然語言處理發生於*工作區* 內部，這是適用於定義應用程式交談流程之所有構件的容器。
{: shortdesc}

單一 {{site.data.keyword.conversationshort}} 服務實例可以包含多個工作區。工作區包含下列類型的構件：

- [**目的**](intents.html)：*目的* 代表使用者輸入的用途，例如關於公司位置或帳單付款的問題。您可以為您想要應用程式支援的每一種使用者要求類型定義一個目的。在工具中，目的名稱前面一律會加上 `#` 字元。若要訓練工作區辨識您的目的，請提供許多使用者輸入範例，並指出它們所對映的目的。
- [**實體**](entities.html)：*實體* 代表與目的相關並且為目的提供特定環境定義的術語或物件。例如，實體可能代表使用者要尋找公司位置的城市，或帳單付款金額。在工具中，實體名稱前面一律會加上 `@` 字元。若要訓練工作區辨識您的實體，請列出使用者可能輸入之每一個實體及同義字的可能值。
- [**對話**](dialog-build.html)：*對話* 是一個分支交談流程，定義應用程式在辨識出已定義的目的及實體時如何回應。您可以使用此工具中的對話建置器來建立與使用者的交談，並根據您在其輸入中辨識的目的及實體來提供回應。

在新增資訊時，工作區會自行訓練，因此您不需要執行任何動作即可起始訓練。

## 工作區限制
{: #workspace-limits}

您可以在單一服務實例中建立的工作區數目，取決於您的 {{site.data.keyword.conversationshort}} 服務方案。除非編輯或複製範例，否則此範例工作區不會計入工作區限制：

| 服務方案         | 每個服務實例的工作區            |
|------------------|--------------------------------:|
| 標準/進階        |                              20 |
| 精簡*            |                               5 |

*30 天未作用之後，可能會刪除未用的「精簡」工作區，以釋出空間。

## 建立工作區
{: #creating-workspaces}

您可以從頭開始建立工作區、使用提供的範例工作區，或從 JSON 檔案匯入工作區。您也可以複製相同服務實例內的現有工作區。

您可以使用 {{site.data.keyword.conversationshort}} 工具來建立工作區。請遵循下列步驟來建立工作區：

1.  如果尚未開啟「服務詳細資料」頁面，請從主控台中按一下 {{site.data.keyword.conversationshort}} 服務實例。（在建立服務實例時，會顯示「服務詳細資料」頁面。）

1.  按一下**啟動工具**。

1.  執行下列其中一項作業：
    - 若要從頭開始建立工作區，請按一下**建立**。
    - 若要使用此範例作為您自己的工作區的起始點，請編輯範例工作區。如果您要將它用於多個工作區，請改為複製它。
    - 若要從 JSON 檔案匯入工作區，請按一下 ![匯入工作區](images/workspace_import.png) 圖示，然後選取您要從中匯入的 JSON 檔案。

        **重要事項：**

        - 匯入的 JSON 檔案必須使用 UTF-8 編碼。
        - 工作區 JSON 檔案的大小上限是 10MB。如果您需要匯入較大的工作區，請考慮在匯入工作區之後個別匯入目的及實體（您也可以使用 REST API 來匯入較大的工作區。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}。）
        - JSON 不能包含定位點、換行或回車字元。

        指定您要包含的資料：

        - 如果您要匯入已匯出工作區的完整副本（包括對話），請選取**全部（目的、實體及對話）**。
        - 如果您要使用已匯出工作區中的目的及實體，但計劃要建置新的對話，請選取**目的及實體**。

1.  指定新工作區的詳細資料：
    - **名稱**：名稱的長度不超過 64 個字元。這是必要值。
    - **說明**：說明的長度不超過 128 個字元。
    - **語言**：要訓練工作區瞭解之使用者輸入的語言。

在建立工作區之後，它會顯示為「工作區」頁面上的磚。

## 匯出及複製工作區
{: #exporting-and-copying-workspaces}

您可以使用「工作區」頁面來匯出工作區，並且將工作區複製到新的工作區。

匯出工作區時，會將所有工作區資料的副本儲存在 JSON 檔案。如果您要將工作區匯入至不同的服務實例，或儲存工作區資料的離線副本，請使用此選項。在匯入工作區時，您可以選擇只匯入目的及實體，這在您要使用相同的訓練資料來建置新的對話時十分有用。

複製工作區時，會在相同的服務實例內建立工作區的完整副本。如果您要調整現有工作區，同時保留原始版本，請使用此選項。

- 若要將現有工作區匯出至 JSON 檔案，請按一下工作區磚中的功能表圖示，然後選取**下載為 JSON**。
- 若要建立相同服務實例內現有工作區的副本，請按一下工作區磚中的功能表圖示，然後選取**複製**。

    指定新工作區的名稱、說明及語言。所有資料（包含目的、實體及對話）都會包含在副本中。

## 與團隊成員共用工作區
{: #invite-others}

建立服務實例之後，就可以讓其他人存取它。您也可以定義訓練資料，並建置對話。

**重要事項**：一次只有一個人可以編輯目的、實體或對話節點。如果多人同時使用相同的項目，則最後儲存變更的人所做的變更會是唯一套用的變更。不會保留其他人在相同時間範圍所做的變更以及較早儲存的變更。請與團隊成員協調您計劃進行的更新，以避免任何人遺失工作。

若要與其他人員共用工作區，您必須提供他們對於管理工作區之服務實例的開發人員存取權。如果您共用的實例會管理您不想讓其他人編輯的其他工作區，則請務必讓您邀請的所有人都清楚知道這一點。

1.  移至 {{site.data.keyword.watson}} Developer Console [專案 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.{DomainName}/developer/watson/projects) 頁面，然後登入。按一下功能表中的**管理 > 帳戶 > 使用者**。
1.  按一下**邀請使用者**，然後輸入您要授與存取權之團隊人員的電子郵件位址。
1.  在 *Cloud Foundry 存取* 區段中，從*組織* 清單中選擇組織。

    *組織角色* 欄位會自動填入*審核員*。您可以保留欄位中的預設值。
1.  選擇性地限制授與單一地區及空間中之工作區的存取權。
1.  在*空間角色* 欄位中，選擇**開發人員**。

    如需角色的相關資訊，請參閱 [Cloud Foundry 角色 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.bluemix.net/docs/iam/cfaccess.html#cfroles)。
1.  按一下**邀請使用者**。

當您邀請的人員下一次登入 {{site.data.keyword.cloud_notm}} 時，您的組織將會包含在其帳戶清單中。

隨著更多人參與工作區開發，可能會發生未計劃性的變更（包括工作區刪除）。請考慮定期建立工作區備份，以便您可以在必要時回復至舊版本。若要建立備份，只需要將工作區匯出為 JSON 檔案。