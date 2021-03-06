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

# 建置對話
{: #dialog-build}

對話定義助理根據自己認為的客戶意圖，在回應客戶時所說的內容。
{: shortdesc}

## 建置對話
{: #dialog-build-task}

若要建立對話，請完成下列步驟：

1.  按一下**對話**標籤，然後按一下**建立對話**。

    第一次開啟對話編輯器時，會為您建立下列節點：

    - **Welcome**：第一個節點。此節點包含使用者首次使用助理時向使用者顯示的問候語。您可以編輯問候語。

    在由使用者起始的對話流程中，不會觸發此節點。例如，與頻道（例如 Facebook 或 Slack）整合時使用的對話會跳過含 `welcome` 特殊條件的節點。如需相關資訊，請參閱[對話起始設定](/docs/services/assistant?topic=assistant-dialog-start)。
    {: note}

    - **Anything else**：最終節點。它會包含在無法辨識使用者輸入時，用來回覆使用者的詞組。您可以取代所提供的回應，或新增更多意義類似的回應，以新增交談變化。您也可以選擇讓助理逐一傳回每個定義的回應，還是隨機傳回它們。
1.  若要將更多節點新增至對話樹狀結構，請按一下 **Welcome** 節點上的**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**新增下面的節點**。
1.  在**如果助理能辨識**欄位中輸入條件，以在符合時觸發助理來處理節點。 

    首先，您一般需要將目的新增為條件。例如，如果您在這裡新增 `#open_account`，表示您要在使用者輸入指出使用者要開啟帳戶時，向使用者傳回您將在此節點中指定的回應。

    當您開始定義條件時，會顯示一個用於顯示您的選項的方框。您可以輸入下列其中一個字元，然後從所顯示的選項清單中挑選一個值。

    <table>
    <caption>條件建置器語法</caption>
    <tr>
      <th>字元</th>
      <th>列出這些構件類型的已定義值</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>目的</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>實體</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>{entity-name} 值</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>您在對話的其他位置所定義或參照的環境定義變數</td>
    </tr>
    </table>

    您可以建立新的目的、實體、實體值或環境定義變數，方法是定義使用該項目的新條件。如果您使用此方式來建立構件，請務必返回，並完成完整建立構件所需的任何其他步驟（例如定義目的的範例詞語）。

    若要定義根據多個條件觸發的節點，請輸入一個條件，然後按一下它旁邊的加號 (+) 圖示。如果您要將 `OR` 運算子套用至多個條件，而不是 `AND`，請按一下顯示在欄位之間的 `and`，以變更運算子類型。AND 運算是在 OR 運算之前執行，但您可以使用括弧來變更順序。例如：`$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    所定義條件的長度必須少於 2,048 個字元。

如需如何測試條件中之值的相關資訊，請參閱[條件](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-conditions)。
1.  **選用**：如果您要向這個節點的使用者收集多個資訊片段，請按一下**自訂**，然後啟用**空位**。如需詳細資料，請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。
1.  輸入回應。
    - 將您要助理向使用者顯示的文字或多媒體元素新增為回應。
    - 如果您要根據特定條件來定義不同的回應，則請按一下**自訂**，並啟用**多個回應**。
    - 如需條件式回應、複合式回應或如何將變化新增至回應的相關資訊，請參閱[回應](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。

1.  指定處理現行節點之後要執行的動作。您可以從下列選項選擇：

    - **等待使用者輸入**：助理暫停，直到使用者提供新的輸入。
    - **跳過使用者輸入**：助理直接跳至第一個子節點。只有在現行節點至少有一個子節點時，才能使用此選項。
    - **跳至**：助理藉由處理您所指定的節點，繼續進行對話。您可以選擇助理是否應該評估目標節點的條件，或直接跳到目標節點的回應。如需詳細資料，請參閱[配置跳至動作](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to-config)。

1.  **選用**：如果您要在執行時期向使用者顯示一組節點選擇，並要求挑選最符合其目標的節點時，考量這個節點，請將這個節點所處理的使用者目標的簡短說明新增至**外部節點名稱**欄位。例如，*開啟帳戶*。

    ![僅限加值或超值方案](images/plus.png) 只會向加值或超值方案使用者顯示*外部節點名稱* 欄位。如需詳細資料，請參閱[澄清](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

1.  **選用**：將節點命名。

    對話節點名稱可以包含字母（Unicode 形式）、數字、空格、底線、連字號及句點。

    將節點命名可讓您輕鬆地記住其用途，並且在節點最小化時可以輕鬆地找到節點。如果您未提供名稱，則會使用節點條件作為名稱。

1.  若要新增其他節點，請選取樹狀結構中的節點，然後按一下**其他** ![「其他」圖示](images/kabob.png) 圖示。
    - 若要建立在現有節點的條件不符時接著檢查的對等節點，請選取**新增下面的節點**。
    - 若要建立在檢查現有節點的條件之前檢查的對等節點，請選取**新增上面的節點**。
    - 若要建立所選取節點的子節點，請選取**新增子節點**。子節點是在其母節點之後進行處理。
    - 若要複製現行節點，請選取**複製**。

如需對話節點處理順序的相關資訊，請參閱[對話概觀](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow)。
1.  在建置對話時進行測試。
   如需相關資訊，請參閱[測試對話](#dialog-build-test)。

## 測試對話
{: #dialog-build-test}

在變更對話時，您可以隨時對它進行測試，以查看對話回應輸入的方式。

您透過「試用」窗格所提交的查詢會產生 `/message` API 呼叫，但不會記載它們，也不會產生費用。

1.  從「對話」標籤中，按一下 ![試用](images/ask_watson.png) 圖示。
1.  在聊天窗格中，鍵入一些文字，然後按 Enter 鍵。

    開始測試對話之前，請確定系統已完成最新變更的訓練。如果系統仍在訓練，則會在*試用* 窗格中顯示一則訊息：
    {: tip}

    ![訓練訊息的畫面擷取](images/training.png)
1.  檢查回應，瞭解對話是否正確地解譯您的輸入，並選擇適當的回應。

    聊天視窗指出輸入中辨識到的目的及實體。

    ![測試對話輸出的畫面擷取](images/test_dialog_output.png)
    如果您要編輯在輸入中辨識到的實體，請按一下實體名稱以在「實體」頁面中將其開啟。如果辨識到錯誤的目的，則可以按一下目的名稱旁邊的箭頭以對其進行更正，或將該主題標示為「不相關」。如需相關資訊，請參閱[進行訓練資料改善](/docs/services/assistant?topic=assistant-logs#logs-fix-data)。

1.  如果您要知道對話樹狀結構中的哪個節點觸發回應，請按一下它旁邊的**位置** ![位置](images/location.png) 圖示。如果您尚未處於「對話」標籤，請開啟它。

    來源節點會取得焦點，並強調顯示助理穿越樹狀結構以到達該位置的路徑。除非您執行另一個動作（例如輸入新的測試輸入），否則會持續將它強調顯示。


1.  若要檢查或設定環境定義變數的值，請按一下**管理環境定義**鏈結。

    即會顯示您在對話中定義的任何環境定義變數。

    此外，還會列出 `$timezone` 環境定義變數。*試用* 窗格使用者介面會從 Web 瀏覽器取得使用者語言環境資訊，並用它來設定 `$timezone` 環境定義變數。此環境定義變數讓處理測試對話交換中的時間參照更為簡單。請考慮在使用者應用程式中執行類似的動作。如果未指定，則會使用「格林威治標準時間 (GMT)」。

    您可以新增變數，並設定其值，以查看對話在下一個測試對話要求中如何回應。例如，如果對話設定成根據使用者所提供的環境定義變數值來顯示不同的回應，則此功能十分有用。

    1.  若要新增環境定義變數，請指定變數名稱，然後按 **Enter** 鍵。
    1.  若要定義環境定義變數的預設值，請在清單中尋找所新增的環境定義變數，然後指定其值。

    如需相關資訊，請參閱[環境定義變數](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context)。

1.  繼續與對話互動，以瞭解如何透過對話進行交談。
    - 若要尋找並重新提交測試詞語，您可以按向上鍵循環瀏覽最近的輸入。
    - 若要從聊天窗格中移除先前的測試詞語，然後重新開始，請按一下**清除**鏈結。這個動作不只會移除測試詞語及回應，也會清除因為與對話互動而設定的任何環境定義變數值。

### 下一步
{: #dialog-build-next-steps}

如果您判定辨識到錯誤的目的或實體，則可能需要修改目的或實體定義。

如果辨識到正確的目的及實體，但在對話中觸發錯誤的節點，請確定已適當地撰寫您的條件。

如需可協助您開始使用的提示，請參閱[對話建置提示](/docs/services/assistant?topic=assistant-dialog-tips)。

如果您已備妥讓交談可以協助您的使用者，請將您的助理與傳訊平台或自訂應用程式整合在一起。請參閱[新增整合](/docs/services/assistant?topic=assistant-deploy-integration-add)。

## 對話節點限制
{: #dialog-build-node-limits}

每個技能可以建立的對話節點數目取決於方案類型。

| 方案 | 每個技能的對話節點數     |
|------------------|---------------------------:|
|超值              |100,000 |
|加值              |100,000 |
|標準              |100,000 |
|加值試用   |25,000 |
|精簡              |                     100`*` |
{: caption="方案詳細資料" caption-side="top"}

在樹狀結構中預先移入的 welcome 及 anything_else 對話節點會計入總計中。

樹狀結構深度限制：對話支援 2,000 個對話節點後代；對話的最佳執行效能是 20 個或以下。

`*` 在 2018 年 12 月 1 日，「精簡」方案的限制已從 25,000 變更為 100。在限制變更之前建立的服務實例使用者必須在 2019 年 6 月 1 日前升級其方案，或在現有服務實例中編輯技能的對話，以符合新的限制需求。

若要查看對話技能中的對話節點數目，請執行下列其中一個動作：

- 如果它尚未與助理相關聯，請將對話技能新增至助理，然後從助理的主頁面檢視技能磚。*訓練資料* 區段會列出對話節點的數目。
- 將 GET 要求傳送至 /dialog_nodes API 端點，並包括 `include_count=true` 參數。例如：

  ```curl
  curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
  ```

  在回應中，`pagination` 物件中的 `total` 屬性包含對話節點數目。

如果總計似乎大於預期，則原因可能是您從應用程式建置的對話已轉換為 JSON 物件。一些出現為單一節點一部分的欄位實際上會結構化為基礎 JSON 物件中的個別對話節點。

  - 每個節點與資料夾都以它自己的節點表示。
  - 與單一對話節點相關聯的每個條件式回應都以個別節點表示。
  - 一個節點若具有數個空位、每個空位、找到空位回應、找不到空位回應、空位處理程式，且如果已設定的話，則「提示輸入所有資訊」回應是個別節點。實際上，某個節點若有三個空位，可能等同於十一個對話節點。

## 搜尋對話
{: #dialog-build-search}

您可以搜尋對話來尋找一個以上提及給定字組或詞組的對話節點。

1.  選取「搜尋」圖示：![「搜尋」圖示](images/search_icon.png)

1.  輸入搜尋詞彙或詞組。

    第一次搜尋時，會建立索引。對於對話節點中的文字進行編製索引時，可能會要求您等待。
    {: note}

顯示包含搜尋詞彙及對應範例的節點。選取任何結果，以開啟進行編輯。

  ![傳回的目的搜尋](images/search_dialog.png)

## 依節點 ID 尋找對話節點
{: #dialog-build-get-node-id}

您可以依其節點 ID 來搜尋對話節點。在搜尋欄位中輸入完整節點 ID。基於下列任何原因，您可能會想要尋找與已知節點 ID 相關聯的對話節點：

- 您要檢閱日誌，而且日誌依節點 ID 參照對話的某個區段。
- 您要將 API 訊息輸出的 `nodes_visited` 內容中所列出的節點 ID，對映至您可在對話樹狀結構中看到的節點。
- 對話運行環境錯誤訊息會通知您語法發生錯誤，並使用節點 ID 來識別您需要修正的節點。

另一種根據節點 ID 來探索節點的方式，就是遵循下列步驟：

1.  在「對話」標籤中，選取對話樹狀結構中的任一節點。
1.  如果已開啟現行節點的編輯視圖，則請予以關閉。
1.  在 Web 瀏覽器的位置欄位中，URL 應顯示下列語法：

    `https://assistant-location.watsonplatform.net/location/instance-id/workspaces/workspace-id/build/dialog#node=node-id`

1.  藉由將現行 `node-id` 值取代為您要尋找的節點 ID 來編輯 URL，然後提交新的 URL。
1.  必要的話，請重新強調顯示編輯過的 URL，並重新提交。

頁面會重新整理，並將焦點切換到具有指定節點 ID 的對話節點。如果節點 ID 適用於空位、「找到空位」或「找不到空位」條件、空位處理程式或條件式回應，則在其中定義空位或條件式回應的節點會取得焦點，並顯示對應的限制模式。

如果您仍然找不到節點，則可以匯出對話技能，並使用 JSON 編輯器來搜尋技能 JSON 檔案。
{: tip}

## 複製對話節點
{: #dialog-build-copy-node}

您可以複製節點來建立確切副本，以作為對話樹狀結構中直接在它下面的對等節點。複製的節點會獲指定與原始節點相同的名稱，但後面會加上 `- copy`*`n`*，其中 *`n`* 是從 1 開始的數字。如果您多次複製同一個節點，則針對每一個副本，名稱中的 *`n`* 都會加 1，以協助您區分不同的副本。如果節點沒有名稱，則名稱會是 `copy`*`n`*。

當您複製具有子節點的節點時，也會一併複製子節點。複製的子節點名稱與原始子節點名稱完全相同。區分複製的子節點與原始子節點的唯一方式是母節點名稱中的 `copy` 參照。

1.  在您要複製的節點上，按一下**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**複製**。
1.  考慮重新命名複製的節點，或編輯其條件來進行區分。

## 移動對話節點
{: #dialog-build-move-node}

您建立的每一個節點都可以在對話樹狀結構的其他位置移動。

您可能想要將先前建立的節點移至流程的另一個區域，以變更交談。您可以移動節點，以變成另一個分支中的同層級或對等節點。

1.  在您要移動的節點上，按一下**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**移動**。
1.  選取位於樹狀結構中接近您要移動此節點之位置的目標節點。選擇將此節點放在目標節點的前面或後面，還是讓它成為目標節點的子節點。

## 使用資料夾組織對話
{: #dialog-build-folders}

您可以藉由將對話節點新增至某個資料夾中，來將它們群組在一起。群組節點的原因有很多，包括：

- 將處理類似主題的節點保存在一起，這樣可以更輕鬆地找到它們。例如，您可以將處理使用者帳戶問題的節點群組在*使用者帳戶* 資料夾中，而將處理付款相關查詢的節點群組在*付款* 資料夾中。
- 將只有在符合特定條件時才要對話處理的一組節點群組在一起。例如，使用 `$isPlatinumMember` 這類條件將節點群組在一起，這些節點提供只有在現行使用者有權接收額外服務時才應該處理的額外服務。
- 在處理節點時，於運行環境隱藏節點。您可以將節點新增至具有 `false` 條件的資料夾，以避免處理它們。

資料夾的這些特徵會影響如何處理資料夾中的節點：

- 條件：如果未指定任何條件，則助理會直接處理資料夾內的節點。如果已指定條件，則助理會先評估資料夾條件來判斷是否處理其內的節點。
- 自訂：資料夾中的節點會繼承全部您套用至資料夾的配置設定。例如，如果您變更資料夾的離題設定，則資料夾中的所有節點都會繼承這些變更。
- 樹狀結構階層：根據資料夾是新增至對話樹狀結構的根層次還是子層次，而將資料夾中的節點視為根節點還是子節點。您新增至根層次資料夾的全部根層次節點都會持續作用為根節點；例如，它們不會變成資料夾的子節點。不過，如果您將根層次節點移至本身為另一個節點子項的資料夾，則根節點會變成該其他節點的子項。

資料夾不會影響節點的評估順序。會繼續從第一個到最後一個來處理節點。如果助理在樹狀結構中向下移動時發現資料夾，而資料夾沒有條件，或其條件為 true，則會立即處理此資料夾中的第一個節點，並從該處依序在樹狀結構中向下繼續處理。如果資料夾沒有資料夾條件，則助理察覺不到資料夾，而且資料夾中的每個節點都會被視為樹狀結構中任何其他的個別節點。

### 新增資料夾
{: #dialog-build-folders-add}

若要將資料夾新增至對話樹狀結構，請完成下列步驟：

1.  從**對話**標籤的樹狀結構視圖中，按一下**新增資料夾**。

    資料夾即會新增至對話樹狀結構尾端，就在 **Anything else** 節點正前方。除非已選取樹狀結構中的現有節點，否則在此情況下，它會新增至所選取節點的下方。

    如果您要在樹狀結構的其他位置新增資料夾，請從要新增資料夾的位置上方的節點，按一下**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**新增資料夾**。

    您可以在現有對話分支的子節點下方新增一個資料夾。若要這樣做，請按一下子節點上的**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**新增資料夾**。

    即會在編輯視圖中開啟資料夾。

1.  **選用**：將資料夾命名。

1.  **選用**：定義資料夾的條件。

    如果您未指定條件，則會使用 `true`，這表示一律會處理此資料夾中的節點。

1.  將對話節點新增至此資料夾。

    - 若要將現有對話節點新增至此資料夾，您必須一次將它們移動至資料夾中。

      在您要移動的節點上，按一下**其他** ![「其他」圖示](images/kabob.png) 圖示，選取**移動**，然後按一下資料夾。選取**目標資料夾**作為移至目標。

      在您移動節點時，節點會新增至資料夾內部的樹狀結構開始處。因此，舉例而言，如果您要保留一組連續根對話節點的順序，請先從最後一個節點開始進行移動。
      {: tip}

    - 若要將新的對話節點新增至資料夾，請按一下資料夾上的**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**將節點新增至資料夾**。

      對話節點即會新增至資料夾內部的對話樹狀結構尾端。

### 刪除資料夾
{: #dialog-build-folders-delete}

您可以只刪除資料夾或資料夾及其中的所有對話節點。

若要刪除資料夾，請完成下列步驟：

1.  從**對話**標籤的樹狀結構視圖中，尋找您要刪除的資料夾。

1.  按一下資料夾上的**其他** ![「其他」圖示](images/kabob.png) 圖示，然後選取**刪除**。

1.  執行下列其中一項作業：

    - 若只要刪除資料夾，並保留資料夾中的對話節點，請取消選取**刪除資料夾內的節點**勾選框，然後按一下**是，將它刪除**。
    - 若要刪除資料夾及其中的所有對話節點，請按一下**是，將它刪除**。

如果您只刪除資料夾，則資料夾中的節點會顯示在對話樹狀結構中資料夾之前所在的位置。
