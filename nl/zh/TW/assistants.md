---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# 助理
{: #assistants}

助理是一種認知機器人，您可以針對商業需求予以自訂，並且跨多個頻道進行部署，以隨時隨地提供客戶所需的協助。
{: shortdesc}

![技能](images/skill-icon.png) 助理會將客戶查詢遞送至技能，然後該技能會提供適當的回應。對話技能會傳回您為了回答常見問題而編寫的回應，而搜尋技能是搜尋現有自助服務內容並傳回一些段落以回答更複雜的查詢。

## 對話技能
{: #assistants-dialog-skill}

對話技能可以瞭解並解決客戶通常需要協助的問題或要求。您可以提供使用者所詢問之主題或作業的相關資訊，以及詢問它們的方式，而產品則會動態建置機器學習模型，其可加以自訂以瞭解相同及類似的使用者要求。

| 對話樹狀結構 | 圖形使用者介面 |
|-------------|-------------------------:|
|您可以使用圖形對話編輯器建立各種類型的 Script，以供助理在與客戶互動時從中讀取。結果是模擬實際交談的對話。對話主要提供您教導它辨識的一般客戶目標，並提供有用的回應。| ![具有範例內容的範例對話樹狀結構](images/dialog-depiction.png) |

對話技能本身是以文字定義，但您可以將它與 Watson Speech to Text 及 Watson Text to Speech 服務整合，讓使用者能口頭與助理互動。

![立即可用的訓練資料](images/oob.png)  如果您想要快速入門，請將預先建置的訓練資料新增至對話技能，讓您的助理可以開始協助您的客戶掌握基本觀念。

## 搜尋技能 ![僅限「加值」或「超值」方案](images/plus.png)
{: #assistants-search-skill}

搜尋技能僅適用於加值或超值使用者。
{: note}

搜尋技能利用現有企業知識庫或主題專家撰寫的其他內容集合中的資訊，解決意外或更細微的客戶查詢。

![IBM Cloud](images/cloud.png) 助理是完全由 {{site.data.keyword.Bluemix_notm}} 管理的機器人，這表示您不需要擔心如何設定或維護基礎架構來支援它。

| 整合       | 頻道  |
|--------------------|:----------|
| 您只需幾個步驟，即可透過多個介面（包括現有傳訊頻道，例如 Slack 及 Facebook Messenger）部署助理。或者，如果您想要設計納入它的自訂應用程式，則可以直接呼叫基礎 API 來這樣做。| ![整合方法，包括 Slack、Facebook Messenger、Web 應用程式或真人服務專員整合](images/integrations.png) |

請參閱[建立助理](/docs/services/assistant?topic=assistant-assistant-add)以開始使用。
