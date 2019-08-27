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

# フィルター照会のリファレンス
{: #filter-reference}

{{site.data.keyword.conversationshort}} サービスの REST API は、フィルター照会を使用する、強力なログ検索機能を提供しています。 /logs API の `filter` パラメーターを使用すると、スキル・ログで、指定した照会に一致するイベントを検索することができます。

`filter` パラメーターは、指定されたフィルターに一致する項目に結果を限定する、キャッシュ可能な照会です。 JSON 応答モデルの一部である任意のオブジェクト (例えば、ユーザー入力テキスト、検出されたインテントとエンティティー、信頼度スコアなど) を対象にフィルタリングすることができます。

さまざまなフィルター照会の例を参照するには、[例](#filter-reference-examples)を参照してください。

/logs `GET` メソッドとその応答モデルについて詳しくは、[API リファレンス![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window} を参照してください。

## フィルター照会の構文
{: #filter-reference-syntax}

以下の例では、フィルター照会の一般的な形式を示します。

| Location           | 照会演算子 | 語句         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- _場所_ は、フィルタリングの対象にするフィールド (この例では `request.input.text`) を指定します。
- _照会演算子_ は、使用するマッチングのタイプ (ファジー・マッチングまたは完全一致) を指定します。
- _語句_ は、マッチングを検出するためにフィールドを評価するのに使用する、式または値を指定します。 語句には、[次のセクション](#filter-reference-operators)で示されているように、リテラル・テキストと演算子を指定できます。

インテントまたはエンティティーによるフィルタリングでは、他のフィールドを対象にしたフィルタリングとは若干異なる構文が必要になります。 詳しくは、[インテントまたはエンティティーによるフィルタリング](#filter-reference-intent-entity)を参照してください。

**注:** フィルター照会の構文で使用する文字のいくつかは、HTTP 照会で使用できません。 特殊文字 (スペースや引用符を含む) を HTTP 照会の一部として送信する場合には、必ずそれらをすべて URL エンコードするようにしてください。 例えば、フィルター `response_timestamp<2016-11-01` の場合は、`response_timestamp%3C2016-11-01` と指定します。

## 演算子
{: #filter-reference-operators}

以下の演算子をフィルター照会で使用できます。

| 演算子 | 説明 |
|:-------------------:|-----------|
| `:` | ファジー・マッチング照会演算子。 照会語句か、または照会語句の文法上の異形を含むすべての値とマッチングさせるには、照会語句の前に `:` を付けます。 ファジー・マッチングは、ユーザー入力テキスト、応答出力テキスト、エンティティー値に対して使用できます。 |
| `::` | 完全一致照会演算子。 照会語句と完全に一致する値のみをマッチングさせるには、照会語句の前に `::` を付けます。 |
| `:!` | 否定ファジー・マッチング照会演算子。 照会語句も、照会語句の文法上の異形も_含まない_ 値のみとマッチングさせるには、照会語句の前に `:!` を付けます。 |
| `::!` | 否定完全一致照会演算子。 照会語句と完全には_一致しない_ 値のみをマッチングさせるには、照会語句の前に `::!` を付けます。 |
| `<=`, `>=`, `>`, `<` | 比較演算子。 算術比較に基づいてマッチングさせるには、照会語句の前にこれらの演算子を付けます。 |
| `\` | エスケープ演算子。 制御文字を含む照会で、それらの制御文字を演算子として解析させない場合に使用します。 例えば、`\!hello` は、`!hello` というテキストとマッチングします。 |
| `""` | リテラル句。 スペースなどの特殊文字を含む照会語句を囲むために使用します。 引用符の中にある特殊文字は API で解析されません。 |
| `~` | 近似一致。 照会ストリングと、応答オブジェクト内の一致ストリングとの間で許容される、単一文字の相違数を指定するには、照会語句の末尾にこの演算子を付加し、その直後に `1` または `2` を付けます。 例えば、`car~1` は `car`、`cat`、または `cars` とマッチングしますが、`cats` とはマッチングしません。 この演算子は、`log_id` またはいずれかの日時フィールドのフィルタリングの場合、あるいはファジー・マッチングを使用したフィルタリングの場合は無効になります。 |
| `*` | ワイルドカード演算子。 ゼロ個以上の任意の文字列とマッチングします。 この演算子は、`log_id` またはいずれかの日時フィールドのフィルタリングの場合は無効になります。 |
| `()`, `[]` | グループ化演算子。 ブール演算子を使用した、複数の式からなる論理グループを囲むために使用します。 |
| `|` | _or_ ブール演算子。 |
| `,` | _and_ ブール演算子。 |

### インテントまたはエンティティーによるフィルタリング
{: #filter-reference-intent-entity}

インテントとエンティティーを内部格納する仕組みに違いがあるため、特定のインテントまたはエンティティーに対するフィルタリングに使用する構文は、戻される JSON 内の他のフィールドの場合に使用する構文と異なります。 `intents` または `entities` コレクション内の `intent` または `entity` フィールドを指定するには、ドットの代わりに、`:` 一致演算子を使用する必要があります。

例えば次の照会では、応答に `hello` という名前の検出されたインテントが含まれる、ログ内のイベントとマッチングします。

`response.intents:intent::hello`

ドットの代わりに `:` 演算子 (intents:intent) を使用していることに注目してください。

応答で検出されるインテントまたはエンティティーの任意のフィールドとマッチングさせる際には、同じパターンを使用してください。 例えば次の照会では、応答に `soda` という値の検出されたエンティティーが含まれる、ログ内のイベントとマッチングします。

`response.entities:value::soda`

同様に、次の例のように、要求の一部として送信されたインテントまたはエンティティーに対してフィルタリングすることができます。

`request.intents:intent::hello`

インテントに対するフィルタリングは、検出されたすべてのインテントに対して作用します。 検出されたインテントのうち、信頼度が最も高いものに限ってフィルタリングするには、`response.top_intent` 省略表現構文を使用することができます。次に例を示します。

`response.top_intent::goodbye`

### お客様 ID によるフィルタリング
{: #filter-reference-customer-id}

お客様 ID によってフィルタリングするには、特殊な場所 `customer_id` を使用します (お客様 ID を含むメッセージへのラベル付けについて詳しくは、[機密保護](/docs/services/assistant?topic=assistant-information-security)を参照してください)。

### 他のフィールドによるフィルタリング
{: #filter-reference-fields}

ログ・データ内のその他のフィールドを対象にフィルタリングするには、/logs API から、JSON 応答内のネスト・オブジェクトのレベルを示すパスで場所を指定します。 ドット (`.`) を使用して、JSON データ内のネスト構造における一連のレベルを指定します。 例えば、`request.input.text` という場所は、以下の JSON フラグメントに示したユーザー入力テキスト・フィールドを表しています。

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```

## 例
{: #filter-reference-examples}

以下の例では、当該構文を使用してさまざまなタイプの照会を示します。

| 説明 | 照会 |
|---------|-----------|
| 応答の日付の月が 2017 年 7 月です。 | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| 応答のタイム・スタンプが `2016-11-01T04:00:00.000Z` より前です。 | `response_timestamp<2016-11-01T04:00:00.000Z` |
| メッセージはお客様 ID `my_id` でラベル付けされます。 | `customer_id::my_id` |
| ユーザー入力テキストに、「order」という単語か、または文法上の異形 (`orders` や `ordering` など) が含まれます。 | `request.input.text:order` |
| 応答内のインテント名が `place_order` と完全に一致します。 | `response.intents:intent::place_order` |
| 応答内のエンティティー名が `beverage` と完全に一致します。  | `response.entities:entity::beverage` |
| ユーザー入力テキストに、「order」という単語も、文法上の異形も含まれません。 | `request.input.text:!order` |
| 検出された、信頼度が最も高いインテントの名前が、`hello` と完全には一致しません。 | `response.top_intent::!hello` |
| ユーザー入力テキストに、`!hello` というストリングが含まれます。 | `request.input.text:\!hello` |
| ユーザー入力テキストに、`IBM Watson` というストリングが含まれます。 | `request.input.text:"IBM Watson"` |
| ユーザー入力テキストに、`Watson` との単一文字の相違数が 2 つ以内のストリングが含まれます。 | `request.input.text:Watson~2` |
| ユーザー入力テキストに、`comm` と、その直後に追加されたゼロ個以上の文字と、その直後に付いた `s` からなるストリングが含まれます。 | `request.input.text:comm*s` |
| コンテキスト内のデプロイメント ID が `my_app` と一致します。 | `request.context.metadata.deployment::my_app` |
| 応答内のインテントの信頼度が 0.8 より大きい値です。 | `response.intents:confidence>0.8` |
| 応答内のインテント名が `hello` または `goodbye` のいずれかと完全に一致します。 | `response.intents:intent::(hello|goodbye)` |
| 応答内のインテントの名前が `hello` で、信頼度が 0.8 以上の値です。 | `response.intents:(intent:hello,confidence>=0.8)` |
| 応答内のインテント名が `order` と完全に一致し、応答内のエンティティー名が `beverage` と完全に一致します。 | `[response.intents:intent::order,response.entities:entity::beverage]` |
