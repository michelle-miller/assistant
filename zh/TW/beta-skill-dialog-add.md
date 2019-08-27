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

# 建置對話技能
{: #beta-skill-dialog-add}

{{site.data.keyword.conversationshort}} 服務的自然語言處理定義於*對話技能*，這是適用於定義交談流程之所有構件的容器。
{: shortdesc}

此特性僅供測試版程式的參與者使用。若要瞭解如何要求存取權，請參閱[參與測試版程式](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![測試版](images/beta.png) IBM 發行分類為測試版的評估的服務、特性及語言支援。這些特性可能不穩定、經常變更，且可能接到簡短通知就中斷服務。測試版特性也可能未提供正式發行特性所提供但不要用於正式作業環境的相同層次的效能或相容性。 

您可以將某種對話技能新增至助理。如需每個方案之限制的相關資訊，請參閱[技能限制](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)。

## 建立對話技能
{: #beta-skill-dialog-add-task}

您可以從頭開始建立技能、使用 IBM 所提供的範例技能，或是從 JSON 檔匯入技能。

如果您尚未這麼做，請完成[入門指導教學](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)中的必要步驟，以建立 {{site.data.keyword.conversationshort}} 服務實例並啟動工具。

您可以使用 {{site.data.keyword.conversationshort}} 工具來建置技能。請遵循下列步驟來建立對話技能：

1.  按一下**技能**標籤。

    如果您已建立或已獲授與開發人員角色，以存取任何使用 {{site.data.keyword.conversationshort}} 服務正式發行版本（早期稱為 Watson Conversation）所建置的工作區，則會在這裡看到這些工作區被列為對話技能。
    {: note}

1.  按一下**建立新的項目**。

1.  按一下**新增**，以建立*對話技能*。

1.  執行下列其中一項作業：

    - 若要從頭開始建立技能，請按一下**建立技能**。
    - 若要新增服務隨附的範例技能，作為您自己技能的起點，或作為要在您自行建立技能之前探索的範例，請按一下**使用範例技能**，然後按一下您要使用的範例。

      範例技能即會新增至您的技能清單。它不會與任何助理相關聯。請跳過本程序中的其餘步驟。

    - 若要將現有技能新增至此服務實例，您可以將它匯入為 JSON 檔案。按一下**匯入技能**，然後按一下**選擇 JSON 檔案**，並選取您要匯入的 JSON 檔案。

      **重要事項：**

      - 匯入的 JSON 檔案必須使用 UTF-8 編碼。
      - 技能 JSON 檔案的大小上限是 10 MB。如果您需要匯入較大的技能，請考慮在匯入技能之後個別匯入目的及實體。（您也可以使用 REST API 來匯入較大的技能。如需相關資訊，請參閱 [API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}。）
      - JSON 不能包含定位點、換行或回車字元。

      指定您要包含的資料：

        - 如果您要匯入已匯出目的的完整副本（包括對話），請選取**全部（目的、實體及對話）**。
        - 如果您要使用已匯出技能中的目的及實體，但計劃要建置新的對話，請選取**目的及實體**。

      按一下**匯入**。

      如果您在匯入技能時遇到問題，請參閱[疑難排解技能匯入問題](#beta-skill-dialog-add-import-errors)。

1.  指定技能的詳細資料：

    - **名稱**：名稱長度不得超過 100 個字元。名稱是必要項目。
    - **說明**：選用說明長度不得超過 200 個字元。
    - **語言**：要訓練技能瞭解之使用者輸入的語言。預設值是英文。

在建立技能之後，它會顯示為「技能」頁面上的磚。現在，您可以開始識別想要對話技能處理的使用者目標。

- 若要將預先建置的目的新增至您的技能，請參閱[使用內容型錄](/docs/services/assistant?topic=assistant-catalog)。
- 若要定義您自己的目的，請參閱[目的](/docs/services/assistant?topic=assistant-intents)。

除非將對話技能新增至助理並部署助理，否則此對話技能無法與客戶互動。請參閱[建立助理](/docs/services/assistant?topic=assistant-assistant-add)。

### 疑難排解技能匯入問題
{: #beta-skill-dialog-add-import-errors}

如果您收到一則訊息，指出技能所包含的構件超過服務方案所強加的限制，請完成下列步驟來順利匯入技能：

1.  購買具有較高構件限制的方案。
1.  在新的方案中建立服務實例。
1.  將技能匯入至新的服務實例。
1.  對技能進行編輯，讓它符合您今後想要使用之方案的構件限制需求。例如，您可能需要減少對話樹狀結構中所使用的對話節點數目。
1.  藉由下載來匯出已編輯的技能。
1.  再試一次，將已編輯的技能匯入至方案上您想要的原始服務實例。

### 將技能新增至助理
{: #beta-skill-dialog-add-to-assistant}

您可以將某種技能新增至助理。您必須開啟助理磚，然後將技能從助理配置頁面新增至助理；您無法從技能配置頁面內選擇將使用技能的助理。多個助理可以使用一個對話技能。

1.  從「助理」標籤中，按一下以開啟您要新增其技能的助理磚。

1.  按一下**新增對話技能**。

1.  按一下**新增現有技能**。

    從顯示的可用技能中，按一下您要新增的技能。

    如果您要新增現有對話技能，並已建立或已獲授與開發人員角色，以存取任何使用 {{site.data.keyword.conversationshort}} 服務正式發行版本（早期稱為 Watson Conversation）所建置的工作區，則會在這裡看到這些工作區被列為對話技能。
    {: note}

## 下載對話技能
{: #beta-skill-dialog-add-download}

您可以下載 JSON 格式的對話技能。例如，如果您想要在另一個 {{site.data.keyword.conversationshort}} 服務實例中使用相同的對話技能，則可能要下載一個技能。您可以從某個實例下載技能，然後將它匯入至另一個實例，作為新的對話技能。

若要下載對話技能，請完成下列步驟：

1.  在「技能」頁面上或在使用技能之助理的配置頁面上，尋找對話技能磚。

1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**下載 JSON**。

1.  指定 JSON 檔案的名稱及儲存位置，然後按一下**儲存**。

您也可以使用 API 來匯出技能。包含 `export=true` 參數與要求。如需詳細資料，請參閱 [API 參考資料](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace)。

## 與團隊成員共用對話技能
{: #beta-skill-dialog-add-invite-others}

建立服務實例之後，就可以讓其他人存取它。您也可以定義訓練資料，並建置對話。

一次只有一個人可以編輯目的、實體或對話節點。如果多人同時使用相同的項目，則最後儲存變更的人所做的變更會是唯一套用的變更。不會保留其他人在相同時間範圍所做的變更以及較早儲存的變更。請與團隊成員協調您計劃進行的更新，以避免任何人遺失工作。
{: important}

若要與其他人員共用對話技能，您必須讓他們可存取管理技能的服務實例。請注意，您邀請的人員將能夠存取此服務實例所管理的任何技能。

1.  記下現行實例名稱，它會顯示在現行頁面的頂端。
1.  按一下頁面標頭中的「使用者」![使用者](images/user-icon2.png) 圖示，從下拉清單中選取**管理使用者**。

1.  按一下**邀請使用者**，然後輸入您要授與存取權之團隊人員的電子郵件位址。

    如果您已讓某人可存取 Cloud Foundry 中的服務實例，則該人員可能已被列為受邀的使用者。按一下人員的名稱，以開啟使用者的存取管理設定。按一下**指派存取權**，然後選擇**指派對資源的存取權**。
1.  在*服務* 區段中，至少進行下列選擇：

    - **服務**：{{site.data.keyword.conversationshort}}
    - **指派平台存取角色**：操作員

    如需平台管理角色的相關資訊，請參閱 [IAM 存取 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/iam?topic=iam-userroles)。（依預設，{{site.data.keyword.conversationshort}} 不會運用服務存取角色。）

    對於 Cloud Foundry 管理的較舊實例，您必須展開 *Cloud Foundry 存取* 區段、選擇您的組織，然後將人員指派給 **開發人員**空間角色。{: note}

1.  按一下**邀請使用者**。

    如果您要編輯現有使用者的存取權，請按一下**指派存取權**。

當您邀請的人員下一次登入 {{site.data.keyword.cloud_notm}} 時，您的帳戶將會內含在其帳戶清單中。如果他們選取您的帳戶，則可以看到您的服務實例，並且開啟及編輯您的技能。

隨著更多人參與對話技能開發，可能會發生非預期的變更（包括技能刪除）。請考慮定期建立對話技能的備份副本，讓您可以在必要時回復至舊版本。若要建立備份，只需要[將技能下載為 JSON 檔案](#beta-skill-dialog-add-download)。
