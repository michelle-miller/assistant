---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Intercom との統合 ![プラス・プランとプレミアム・プランのみ](images/premium.png)
{: #deploy-intercom}

Intercom は、顧客ライフサイクル全体を通じてより良い関係を築くことによりビジネスの成長の促進を支援するカスタマー・メッセージング・プラットフォームです。
{: shortdesc}

Intercom は IBM と提携して、新しいエージェントである仮想 Watson Assistant を顧客サポート・チームに導入しました。ご使用のアシスタントを Intercom アプリケーションと統合して、アシスタントと人間の担当者の間のユーザー会話をアプリでシームレスに渡すことができます。統合について詳しくは、[Watson ブログ投稿 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8) を参照してください。

この統合は、プラス・プランとプレミアム・プランのユーザーのみ使用できます。
{: note}

アシスタントを Intercom と統合すると、Intercom アプリケーションはスキルのクライアント向けアプリケーションになります。ユーザーとの対話はすべて Intercom によって開始され、管理されます。

現時点では、進行中の会話を 1 つの統合チャネルから別の統合チャネルに渡す方法はありません。

## 一回限りのエージェントの作成
{: #deploy-intercom-account-prereq}

Intercom の統合をアシスタントに追加する前に、自分または組織内の別のメンバーが以下の一回限りの前提条件ステップを実行する必要があります。

1.  アシスタントの機能 E メール・アカウントを作成します。

    アシスタントを Intercom のチームに追加するには、その前に各アシスタントに有効かつ固有の E メール・アドレスを設定しておく必要があります。
1.  Intercom ワークスペースから、アシスタントを新規エージェントとしてチームに追加します。

    Intercom ワークスペースのチームメイト設定ページに進み、上記のステップで作成した E メールを招待フィールドに追加することによって、アシスタントを新規エージェントとして招待します。

    Intercom ワークスペースをまだセットアップしていない場合は、[www.intercom.com ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.intercom.com) で作成してください。ワークスペースを作成するには、少なくとも Intercom の *Inbox* 製品に登録している必要があります。

1.  上記で作成したアシスタントの E メール・アカウントで、Intercom からの招待を見つけます。E メールに記載されているリンクをクリックして、チームに参加します。アシスタントの機能 E メール・アドレスを使用して登録してから、チームに参加します。

1.  **オプション**: アシスタントのプロファイルを更新します。

    アシスタントの名前とプロファイル・ピクチャーを編集することができます。このプロファイルは、ワークスペース内のプライベートなエージェント通信、および Intercom アプリを介した顧客とのパブリックな対話におけるアシスタントを表します。ご使用のブランドを反映するプロファイルを作成します。

    ナビゲーション・バーで Intercom のプロファイル・アイコンをクリックして、プロファイルとワークスペースの設定にアクセスします。

    ![「Intercom Setting」ページのスクリーン・ショット](images/intercom-settings.png)

## ダイアログの準備
{: #deploy-intercom-dialog-prereq}

ダイアログ・スキルで以下の手順を実行して、アシスタントでユーザー要求を処理できるようにするとともに、顧客からの要求時に会話を人間の担当者に渡すことができるようにします。

1.  ユーザーによる人との会話要求を認識できるインテントをスキルに追加します。

    独自のインテントを作成することも、IBM が開発した**「一般」**コンテンツ・カタログに用意されている `#General_Connect_to_Agent` という名前の作成済みインテントを追加することもできます。

1.  上記のステップで作成したインテントを条件とするルート・ノードをダイアログに追加します。応答タイプとして**「Connect to human agent」**を選択します。

1.  Intercom アプリからアシスタントによってトリガーされる各ダイアログ・ブランチを作成します。

    ダイアログ内のすべてのルート・ダイアログ・ノードは、フォルダー内のルート・ノードを含め、Intercom チームメイトとして機能している間にアシスタントで処理できます。お客様は、後で Intercom 統合を構成するときに、アシスタントが各ダイアログ・ブランチで行うアクションを指定します。そのため、ノードを Intercom で非表示にすることはできませんが、ノードのトリガー時にアシスタントが何のアクションも実行しないように構成することはできます。

    各ダイアログ・ブランチのルート・ノードの以下のフィールドに入力します。

    - **Node name**: ノードに名前を付けます。この名前は、後でノードの対話を構成するときに、ノードを識別する手段となります。名前を追加しない場合、代わりにノード ID に基づいてノードを選択する必要があります。
    - **External node name**: ダイアログ・ブランチの目的の要約を追加します。例えば、*「ストアの検索」*などです。

      この情報は、アシスタントがユーザー照会への回答を提示するときに、アシスタントのチームの他の担当者に表示されます。照会に対応できるダイアログ・ノードが複数ある場合、アシスタントは応答オプションのリストを人間の担当者のチームメイトと共有して、使用する応答についてのアドバイスを取得します。

      ![ノードの目的の要約を追加するノード編集ビューのフィールドのスクリーン・ショット。](images/disambig-node-purpose.png)

      ステップ 2 で作成したルート・ノードに外部ノード名は追加**しないでください**。エスカレーションが発生したときに、本サービスは最後に処理されたノードの外部ノード名を参照して、うまく達成されていないユーザー目標を認識します。人間の担当者のインテントへの接続を持つノードに外部ノード名を含めると、問題をエスカレートする前にユーザーが対話していた目標指向の最後の実際のノードを本サービスで識別できなくなってしまいます。
      {: tip}

1.  ブランチの下位ノードが、アシスタントに処理させたくないフォローアップの要求または質問を条件とする場合は、**「Connect to human agent」**応答タイプをノードに追加します。

    例えば、人間でしか処理できない機密性の高い問題を扱うノード、またはアシスタントがユーザーの意図を理解する上で何度も失敗するケースを追跡するノードにこの応答タイプを追加することができます。

    実行時に、会話がこの下位ノードに到達すると、その時点でダイアログは人間の担当者に渡されます。後で Intercom 統合をセットアップするときに、各ブランチのバックアップとして人間の担当者を選択することもできます。

これで、Intercom でアシスタントをサポートするための準備がダイアログで整いました。

## Intercom 統合の追加
{: #deploy-intercom-add-intercom}

1.  「アシスタント (Assistants)」タブから、デプロイするアシスタントのタイルをクリックして開きます。

1.  「Integrations」セクションから、**「Add Integration」**をクリックします。

1.  **「Intercom」**をクリックします。

    画面に表示される指示に従います。以下のセクションでは、各ステップの実行を支援します。

## Intercom へのアシスタントの接続
{: #deploy-intercom-connect}

アシスタントを使用するための権限を Intercom に付与すると、アシスタントはすぐに Intercom チームの実行可能なメンバーになります。

人間の担当者は、Intercom の割り当てルール (何らかの基準に基づいてインバウンドの会話をチームメイトまたはチームの受信ボックスに自動的に割り当てることができるルール) を使用するか、実行時に人間の担当者が手動で再割り当てすることによって、アシスタントにメッセージを割り当てることができます。詳しくは、[Intercom の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams) を参照してください。

1.  ダイアログの準備ができたら、**「Connect now」**をクリックします。
1.  **「Access Intercom」**をクリックして、Intercom のサイトにリダイレクトします。

     アシスタントの機能 E メール・アドレスとパスワード (自分の E メール・アドレスとパスワードではない) を使用して Intercom にログインします。組織内の複数の担当者で共有される機能 ID への接続を確立することもできます。
     {: important}

1.  **「Authorize access」**をクリックします。
1.  **「概要に戻る」**をクリックします。

これで、アシスタントは Intercom チームメイトからの割り当てを受け取ることができるようになりました。[ダイアログをセットアップ](#deploy-intercom-dialog-prereq)してください (まだである場合)。
{: important}

## メッセージ・ルーティングの構成
{: #deploy-intercom-config-backup}

アシスタントが進行中の会話を人間の担当者に転送する必要がある場合に、人間のチームメイトをアシスタントのバックアップとして割り当てます。ダイアログ・ブランチごとに別のチームまたはチームメイトをバックアップ連絡先として選択できます。

アシスタントから人間へのエスカレーション時のルーティング割り当てを設定するには、以下の手順を実行します。

1.  Intercom 統合ページから**「My Dialog Skill is ready」**をクリックして、ダイアログの準備が整っていることを確認します。

    [ダイアログの準備](#deploy-intercom-dialog-prereq)の手順が完了している場合にのみ、このボタンをクリックします。
    {: important}

1.  *「設定」*セクションで、**「Manage rules」**をクリックします。

    変更を行わない場合、すべてのノードのバックアップ連絡先は未割り当てのままになります。

1.  **「New rule」**をクリックします。

1.  *「Choose node」*ドロップダウン・リストから、構成するダイアログ・ブランチのノードを選択します。

    ブランチはそのノード名で識別されることを思い出してください。ノード名を指定しなかった場合は、代わりにノードの ID が表示されます。

1.  このダイアログ・ブランチのバックアップ連絡先とするチームまたは人間の担当者のチームメイトを選択します。アシスタントが照会に回答できない場合や、*「Connect to human agent」*応答タイプ (人間しか回答できないことを示す) の下位ノードを扱う場合に、ユーザー照会はここで選択した担当者にエスカレートされます。

1.  他のダイアログ・ブランチのルーティング・ルールを定義するには、**「New rule」**をもう一度クリックし、上記のステップを繰り返します。

    このとき、ブランチの下位ノードに*「Connect to human agent」*応答タイプがあるようなルート・ノードに対して、忘れずに割り当てをセットアップしてください。これに関連するルート・ノードを特定の担当者またはチームに転送しないと、機密性の高い内容が*「Unassigned」*の受信ボックスに転送されることがあります。

1.  ルールを追加したら、**「Return to overview」**をクリックして、ページを終了します。

## ユーザー照会のモニターおよび回答を行う権限をアシスタントに付与する操作
{: #deploy-intercom-config-action}

アシスタントで Intercom 受信ボックスのモニタリングおよびメッセージへの自動回答を開始するときに、モニタリングをオンにします。

アシスタントは、ユーザー照会が Intercom に記録されるときに、その内容を監視します。アシスタントは、ユーザー照会への回答方法を知っていると確信できる場合は、直接ユーザーに応答します。(アシスタントは、本サービスによって識別される上位インテントの信頼度スコアが 0.75 以上の場合に確信があると判断します。)

特定のタイプのユーザー照会に対する回答についてはアシスタントで行わない場合は、アシスタントが実行する他のアクションをダイアログ・ブランチごとに指定するルールを追加できます。例えば、最初はアシスタントによる Intercom チームへの関与を控えめにして、回答を行う他のチームメイトにアシスタントがユーザー・メッセージを転送するときに、応答の推奨を提示するのみにすることもできます。その後、アシスタントの能力が発揮できるようになったら、アシスタントの責任を増やすこともできます。

アシスタントで特定のダイアログ・ブランチを扱う方法を構成するには、ルールを定義します。

1.  Intercom 統合ページの*「Enable your assistant to monitor an inbox」*セクションで、モニターを**「On」**に切り替えます。

1.  *「設定」*で、**「Manage rules」**をクリックします。

    ルールを定義していない場合、アシスタントは*「Unassigned」*受信ボックスをモニターし、対処できるという確信を得たユーザー照会に自動的に回答するように構成されています。

1.  *「Monitor Intercom inbox」*フィールドで、アシスタントでモニターする Intercom 受信ボックスを選択します。

1.  **「New rule」**をクリックし、特定のダイアログ・ブランチに固有の対話パターンを定義します。

1.  *「Choose node」*ドロップダウン・リストから、構成するブランチのノードを選択します。

    ブランチはそのノード名で識別されることを思い出してください。ノード名を指定しなかった場合は、代わりにノードの ID が表示されます。

1.  このダイアログ・ノードがトリガーされたときにアシスタントで実行するアクションのタイプを選択します。アクション・タイプのオプションは以下のとおりです。

    - **Do nothing**: アシスタントは応答に関与しません。ユーザーのメッセージは、他のメンバーが対処するまで受信ボックスに残されます。
    - **Send to team or teammate**: アシスタントはユーザー入力を評価してその目標を判別し、適切なチームメイトに転送します。
    - **Suggest to team or teammate**: アシスタントは、社内の Intercom アプリを介して人間の担当者とメモを共有することにより、応答方法に関する提案をチームメイトに提供します。

      - ユーザー入力がダイアログ・ブランチ (包括的な対話を表す、下位ノードを持つルート・ダイアログ・ノード) をトリガーする場合、アシスタントは要求に対応できることを示し、対応することを申し出ます。
      - ユーザー入力が下位ノードを持たないルート・ノードをトリガーする場合、アシスタントはノードからのプログラムによる応答を人間の担当者と共有するだけで、ユーザーには直接応答しません。
      - 入力が信頼度の高い複数のダイアログ・ノードをトリガーする場合、アシスタントは可能な応答のリストを人間のチームメイトに提示して、最善の応答を選択するようチームメイトに依頼します。

      すべてのケースで、人間の担当者はアシスタントに応答を引き継がせるかどうかを決定します。

    - **Answer**: アシスタントは、他のチームメイトと相談することなく、直接ユーザーに応答します。

    *「Send to team or teammate」*または*「Suggest to team or teammate」*のいずれかのアクション・タイプを割り当てたノードでは、チームメイトによる関与が必要になります。必ず前述の手順に戻って、特にこのダイアログ・ノードのバックアップとして適切な担当者またはチームを割り当てるルールを追加してください。

1.  他のダイアログ・ブランチの固有の対話設定を定義するには、**「New rule」**をもう一度クリックし、上記のステップを繰り返します。

1.  ルールを追加したら、**「Return to overview」**をクリックして、ページを終了します。

ダイアログが変更されると、多くの場合、これらのルールに増分変更を行うための Intercom 統合ページに戻ります。

## 統合のテスト
{: #deploy-intercom-try}

Intercom の統合をエンドツーエンドで効果的にテストするには、Intercom エンド・ユーザー・アプリケーションへのアクセス権限が必要になります。Intercom ワークスペースは既に作成または編集されています。ワークスペースには、関連付けられたユーザー・インターフェース・クライアントが必要です。このクライアントがない場合は、[Apps in Intercom ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.intercom.com/help/apps-in-intercom){: new_window} を参照して、このセットアップに役立ててください。

Intercom ワークスペースに関連付けられたクライアント・アプリケーションを介してテストのユーザー照会を送信し、Intercom によってメッセージが処理される方法を確認します。アシスタントで回答する予定のメッセージが適切な応答を生成していること、および、回答するように構成されていないメッセージにアシスタントが応答していないことを確認します。

## ダイアログについての考慮事項
{: #deploy-intercom-dialog}

ダイアログに追加する一部のリッチ応答は、「Try it out」ペイン内では Intercom ユーザーに表示される方法とは異なる方法で表示されます。以下の表で、それぞれの応答タイプが Intercom でどのように扱われるかを説明します。

| 応答タイプ | Intercom ユーザーへの表示方法  |
|---------------|---------------------------|
| **Option**    | オプションが番号付きリストとして表示されます。**「title」**または**「description」**フィールドに、リストからオプションを選択する方法についてユーザーに説明する記述を入力します。|
| **Image**     | イメージの**タイトル**、**説明**、およびイメージ自体がレンダリングされます。|
| **Pause**     | 入力インディケーターは、有効化するかどうかに関係なく、一時停止中は表示されません。|

応答タイプについて詳しくは、[リッチ応答](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)を参照してください。
