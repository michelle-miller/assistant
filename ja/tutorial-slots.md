---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# チュートリアル: スロットを含むノードをダイアログに追加する
{: #tutorial-slots}

このチュートリアルでは、ダイアログ・ノードにスロットを追加して、単一のノード内でユーザーから複数の情報を収集します。 作成するノードが、レストランの予約を行うために必要な情報を収集します。
{: shortdesc}

## 学習目標
{: #tutorial-slots-objectives}

このチュートリアルを完了すると、以下の方法について理解できます。

- ダイアログに必要なインテントとエンティティーを定義する
- ダイアログ・ノードにスロットを追加する
- スロットを含むノードをテストする

### 所要時間
{: #tutorial-slots-duration}

このチュートリアルを完了するための所要時間は約 30 分です。

### 前提条件
{: #tutorial-slots-prereqs}

始めに、[開始チュートリアル](/docs/services/assistant?topic=assistant-getting-started)を完了します。 作成した {{site.data.keyword.conversationshort}} チュートリアル・スキルを使用して、開始演習の一部として作成した簡単なダイアログにノードを追加します。

必要に応じて新規ダイアログ・スキルから開始することもできます。 このチュートリアルを始める前に、必ずスキルを作成しておいてください。
{: note}

## ステップ 1: インテントと例を追加する
{: #tutorial-slots-add-intent}

「インテント (Intents)」タブでインテントを追加します。 インテントとは、ユーザー入力で表現される目的または目標のことです。 ユーザーがレストランの予約を希望していることを示すユーザー入力を認識する #reservation インテントを追加します。

1.  チュートリアル・スキルの**「インテント (Intents)」**ページから、**「インテントの追加 (Add intent)」**をクリックします。
1.  次のインテント名を追加して、**「インテントの作成 (Create intent)」**をクリックします。

    ```json
    reservation
    ```
    {: screen}

    #reservation インテントが追加されました。 インテント名には、インテントであることを示すラベルとして番号記号 (`#`) が付加されます。 この命名規則により、インテントをインテントとして認識することができます。 このインテントには、ユーザーの発話例がまだ関連付けられていません。
1.  **「ユーザーの例の追加 (Add user examples)」**フィールドに次の発話を入力して、**「例の追加 (Add example)」**をクリックします。

    ```json
    予約がしたい (i'd like to make a reservation)
    ```
    {: screen}

1.  Watson が `#reservation` インテントを認識しやすくなるように、次の例をさらに追加します。

    ```json
    ディナーの予約がしたい (I want to reserve a table for dinner)
    3 名でランチの予約をしたいのですが (Can 3 of us get a table for lunch?)
    次の水曜の 7 時に予約できますか (do you have openings for next Wednesday at 7?)
    火曜の夜に 4 名で予約したいのですが (Is there availability for 4 on Tuesday night?)
    明日のブランチの予約がしたいのですが (i'd like to come in for brunch tomorrow)
    座席の予約は可能ですか (can i reserve a table?)
    ```
    {: screen}

1.  **「閉じる」**アイコン ![「閉じる」矢印](images/close_arrow.png) をクリックして、`#reservation` インテントとその発話例の追加を完了します。

## ステップ 2: エンティティーを追加する
{: #tutorial-slots-add-entity}

エンティティー定義には、指定されたインテントのコンテキストで頻繁に使用される語彙を表す一連のエンティティー*値*が含まれます。 エンティティーを定義することで、サービスがユーザー入力内にある対象のインテントに関連した語彙の言及を特定しやすくなります。 このステップでは、時刻、日付、および数値の言及を認識できるシステム・エンティティーを有効にします。

1.  **「エンティティー (Entities)」**をクリックして「エンティティー (Entities)」ページを開きます。
1.  ユーザー入力内の日付、時刻、および数値の言及を認識できるシステム・エンティティーを有効にします。 **「システム・エンティティー (System entities)」**タブをクリックして、以下のエンティティーを有効にします。

    - `@sys-time`
    - `@sys-date               `
    - `@sys-number               `

@sys-date、@sys-time、および @sys-number システム・エンティティーが正常に有効化されました。 これで、これらのエンティティーをダイアログで使用できるようになりました。

## ステップ 3: スロットを含むダイアログ・ノードを追加する
{: #tutorial-slots-add-dialog-with-slots}

ダイアログ・ノードは、サービスとユーザー間のダイアログのスレッドの開始を表します。 このノードには条件が含まれており、その条件を満たすと、ノードがサービスによって処理されます。 また、応答が 1 つ以上含まれています。 例えば、ノード条件によって、ユーザー入力の中にある `#hello` インテントを検出し、「`こんにちは、ご用件をどうぞ (Hi. How can I help you?)`」と応答することができます。 この例は、1 つの条件と 1 つの応答が含まれた、ダイアログ・ノードの最も単純な形式になります。 複雑なダイアログを定義するには、単一のノードに条件付き応答を追加したり、ユーザーとのやり取りを引き延ばす子ノードを追加したりします。 (複雑なダイアログについて理解するには、[複雑なダイアログの作成](/docs/services/assistant?topic=assistant-tutorial)チュートリアルを完了してください。)

このステップで追加するノードは、スロットを含むノードです。 スロットは、単一のノード内でユーザーに複数の情報を要求して保存するための構造化形式を提供します。 これらのスロットは、特定のタスクを念頭に置いており、タスクの実行前にユーザーから重要な情報を取得する必要がある場合に最も役立ちます。 詳しくは、[スロットを使用した情報の収集](/docs/services/assistant?topic=assistant-dialog-slots)を参照してください。

追加するノードは、レストランでの予約に必要な情報を収集します。

1.  **「ダイアログ (Dialogs)」**タブをクリックして、ダイアログ・ツリーを開きます。
1.  **#General_Greetings** ノードで「その他 (More)」アイコン ![その他のオプション](images/kabob.png) をクリックしてから、**「ノードを下に追加 (Add node below)」**を選択します。
1.  条件フィールドに `#reservation` の入力を始めると、リストが表示されます。そのリストから該当するインテント名を選択します。
    このノードは、ユーザー入力が `#reservation` インテントと一致する場合に評価されます。
1.  **「カスタマイズ (Customize)」**をクリックし、**「スロット (Slots)」**トグルをクリックして**オン**に切り替え、**「適用」**をクリックします。

    ![スロットがオンに切り替えられたカスタマイズ・ダイアログが示されています。](images/slots-toggle-on.png)
1.  以下のスロットを定義します。

    <table>
    <caption>スロットの詳細</caption>
    <tr>
      <th>チェック対象 (Check for)</th>
      <th>保存名</th>
      <th>存在しない場合の質問</th>
    </tr>
    <tr>
      <td>@sys-date</td>
      <td>$date</td>
      <td>どの日に予約しますか? (What day would you like to come in?)</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>何時に予約しますか? (What time do you want the reservation to be made for?)</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number</td>
      <td>$guests</td>
      <td>予約人数は何名ですか? (How many people will be dining?)</td>
    </tr>
    </table>

1.  応答として、「`わかりました。$date の $time に $guests 名で予約します (OK. I am making you a reservation for $guests on $date at $time`」を指定します。

    ![ツールで各スロットと応答が指定どおりに入力されている場合の様子が示されています。](images/slots-simple-node.png)

1.  ノード編集ビューを閉じるには、![閉じる](images/close.png) をクリックします。

## ステップ 4: ダイアログをテストする
{: #tutorial-slots-test}

1.  ![Watson に質問する](images/ask_watson.png) アイコンを選択して、チャット・ペインを開きます。
1.  「`予約がしたい (i want to make a reservation)`」と入力します。

    アシスタントが #reservation インテントを認識し、最初のスロットのプロンプト「`どの日に予約しますか? (What day would you like to come in?)`」で応答します。

1.  「`金曜日 (Friday)`」と入力します。

    アシスタントが値を認識し、その値を最初のスロットの $date コンテキスト変数値として保存します。 その後、次のスロットのプロンプト「`何時に予約しますか? (What time do you want the reservation to be made for?)`」を表示します。

1.  「`午後 5 時 (5pm)`」と入力します。

    アシスタントが値を認識し、その値を 2 番目のスロットの $time コンテキスト変数値として保存します。 その後、次のスロットのプロンプト「`予約人数は何名ですか? (How many people will be dining?)`」が表示されます。

1.  「`6`」と入力します。

    アシスタントが値を認識し、その値を 3 番目のスロットの $guests コンテキスト変数値として保存します。 すべてのスロットにデータが取り込まれたので、ノードの応答「`わかりました。2017-12-29 の 17:00:00 に 6 名で予約します (OK. I am making you a reservation for 6 on 2017-12-29 at 17:00:00)`」が表示されます。

![「試行する (Try it out)」ペインにテスト・ダイアログが表示されています。ノード・スロットに正常にデータが入力されています。](images/slots-test-simple-node.png)

うまくいきました。 お疲れさまでした。 スロットを含むノードを正常に作成できました。

## サマリー
{: #tutorial-slots-summary}

このチュートリアルでは、レストランを予約するために必要な情報を取得できるスロットを含むノードを作成しました。

## 次のステップ
{: #tutorial-slots-next-steps}

ノードと対話するユーザーのエクスペリエンスを向上させます。 後続のチュートリアル、[スロットを含むノードを改善する](/docs/services/assistant?topic=assistant-tutorial-slots-complex)を完了してください。 このチュートリアルでは、システムから返される日付 (2017-12-28) と時刻 (17:00:00) の値の形式を変更する方法など、単純な改善方法について説明しています。 また、ダイアログがスロットに期待するタイプの値をユーザーが提供しなかった場合の対処方法など、より複雑な作業についても説明しています。
