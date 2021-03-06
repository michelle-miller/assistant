---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: condition, response, options, jump, jump-to, multiline, response variations

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

# 对话概述
{: #dialog-overview}

对话使用在用户输入中识别到的意向以及应用程序中的上下文来与用户进行交互，并最终提供有用的响应。
{: shortdesc}

对话会将意向（用户说的内容）与响应（机器人回复的内容）相匹配。响应可能是对问题（例如，`在哪里能加点天然气？`）的回答，也可能是执行命令，例如打开收音机。意向和实体的信息可能足以确定正确的响应，但若不能，对话可能会要求用户提供正确响应所需的更多输入。例如，如果用户询问：`我在哪里能找到吃的？`您可能希望弄清楚他们是想去餐馆还是杂货店，是要去店里吃还是外卖等等。您可以在文本响应中要求提供更多详细信息，并创建一个或多个子节点来处理新的输入。

<iframe class="embed-responsive-item" id="youtubeplayer" title="对话概述" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/XkhAMe9gSFU?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

注：此视频的持续时间为 15 分钟；前 5 分钟说明如何添加节点。

对话在 {{site.data.keyword.conversationshort}} 中以图形方式表示为树。创建分支来处理您希望会话处理的每个意向。分支由多个节点组成。

## 对话节点
{: #dialog-overview-nodes}

每个对话节点至少包含一个条件和一个响应。

![显示用户输入会进入包含语句“If: 条件, Then: 响应”的框](images/node1-empty.png)

- 条件：指定要触发对话中的此节点，用户输入中必须提供的信息。该信息通常是特定意向。还可以是实体类型、实体值或上下文变量值。有关更多信息，请参阅[条件](#dialog-overview-conditions)。
- 响应：助手用于响应用户的话语。响应还可以配置为显示图像或选项列表，或者触发程序化操作。有关更多信息，请参阅[响应](#dialog-overview-responses)。

您可以将节点视为具有 if/then 构造：如果此条件为 true，那么返回此响应。

例如，如果助手的自然语言处理功能确定用户输入包含 `#cupcake-menu` 意向，那么将触发以下节点。触发节点的结果是，助手会使用相应的回答进行响应。

![显示用户在询问有关纸杯蛋糕口味的问题。If 条件为 #cupcake-menu，Then 响应为列出纸杯蛋糕的各个口味。](images/node1-simple.png)

具有一个条件和响应的单个节点可以处理简单的用户请求。但是，用户往往会有更复杂的问题，或者希望获得更复杂任务的帮助。为此，可以添加子节点，这些子节点会要求用户提供助手所需的任何其他信息。

![显示对话中的第一个节点在询问用户需要哪种类型的纸杯蛋糕，是无谷蛋白的还是普通纸杯蛋糕，此节点有两个子节点，用于根据用户的回答提供不同的响应。](images/node1-children.png)

## 对话流
{: #dialog-overview-flow}

创建的对话由助手从树中的第一个节点到最后一个节点进行处理。

![位于 3 个节点旁边的向下箭头，用于表明对话是从第一个节点到最后一个节点流动](images/node-flow-down.png)

随着助手沿树向下推进，如果发现符合的条件，就会触发相应节点。然后，服务会沿着触发的节点移动，以根据任何子节点条件检查用户输入。服务检查子节点时，同样是从第一个子节点移至最后一个子节点。

助手将继续在对话树中从第一个节点向最后一个节点推进，然后沿每个触发的节点从第一个子节点向最后一个子节点推进，再沿每个触发的子节点推进，直至到达所在分支中的最后一个节点。

![显示的箭头 1 从第一个根节点指向最后一个节点，箭头 2 沿着已触发节点的长度指向，箭头 3 从已触发节点的第一个子节点指向最后一个子节点。](images/node-flow.png)

开始构建对话时，必须确定要包含的分支数以及这些分支的放置位置。分支的顺序非常重要，因为节点是按第一个到最后一个的顺序求值的。将使用其条件与输入相匹配的第一个根节点；树中后续的任何节点都不会触发。

助手到达分支末尾，或者在当前所求值的子节点集内找不到求值为 true 的条件时，会跳回至树的基本节点。随后，助手将再一次从第一个根节点到最后一个根节点进行处理。如果没有任何条件求值为 true，那么将返回树中最后一个节点的响应，该响应通常具有始终求值为 true 的特殊 `anything_else` 条件。

可以通过以下方式，中断标准的从头到尾的流。

- 通过定制在处理某个节点后执行的操作。例如，可以将一个节点配置为在进行处理后直接跳转至另一个节点，即便这另一个节点位于树中的更早位置。有关更多信息，请参阅[定义后续操作](#dialog-overview-jump-to)。
- 通过配置要跳转至其他节点的条件响应。有关更多信息，请参阅[条件响应](#dialog-overview-multiple)。
- 通过配置对话节点的离题设置。离题还可能会影响用户在运行时如何在各个节点之间移动。如果从大多数节点启用离题并配置了返回，那么用户可以更轻松地从一个节点跳转至另一个节点，然后再跳转回前者。有关更多信息，请参阅[离题](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions)。

## 条件
{: #dialog-overview-conditions}

节点条件确定会话中是否使用该节点。响应条件确定要返回给用户的响应。

- [条件工件](#dialog-overview-condition-artifacts)
- [特殊条件](#dialog-overview-special-conditions)
- [条件语法详细信息](#dialog-overview-condition-syntax)

有关在条件中执行更高级操作的提示，请参阅[条件用法提示](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-condition-usage-tips)。

### 条件工件
{: #dialog-overview-condition-artifacts}

可以使用下列一个或多个工件的任何组合来定义条件：

- **上下文变量**：如果指定的上下文变量表达式为 true，那么将使用该节点。请使用语法 `$variable_name:value` 或 `$variable_name == 'value'`。

  对于节点条件，此工件类型通常与 AND 或 OR 运算符和其他条件值一起使用。这是因为用户输入中的某些内容必须触发相应节点；仅仅是上下文变量值匹配并不足以触发该节点。例如，如果用户输入对象以某种方式设置了上下文变量值，那么会触发相应节点。

  不要在已设置上下文变量值的同一对话节点中，基于该上下文变量的值来定义节点条件。
  {: tip}

  对于响应条件，此工件类型可以单独使用。可以根据特定上下文变量值来更改响应。例如，`$city:Boston` 会检查 `$city` 上下文变量是否包含值 `Boston`。如果包含，那么会返回相应响应。
  
  有关上下文变量的更多信息，请参阅[上下文变量](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context)。

- **实体**：在用户输入中识别到实体的任何值或同义词时，将使用该节点。请使用语法 `@entity_name`。例如，`@city` 会检查在用户输入中是否检测到为 @city 实体定义的任何城市名称。如果检测到，那么会处理相应节点或响应。

  请考虑创建对等节点来处理未识别到实体的任何值或同义词的情况。
  {: tip}

  有关实体的更多信息，请参阅[定义实体](/docs/services/assistant?topic=assistant-entities)。

- **实体值**：如果在用户输入中检测到实体值，那么将使用该节点。请使用语法 `@entity_name:value`，并为实体指定定义的值，而不要指定同义词。例如：`@city:Boston` 会检查在用户输入中是否检测到特定城市名称 `Boston`。

  如果在对等节点中检查实体是否存在，而不为其指定特定值，请确保将此节点（用于检查特定实体值）置于仅检查该实体是否存在的对等节点之前。否则，将永远不会对此节点求值。
  {: tip}

  如果实体是具有捕获组的模式实体，那么可以检查特定组值匹配情况。例如，可以使用语法：`@us_phone.groups[1] == '617'`
  有关更多信息，请参阅[存储和识别输入中的模式实体组](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-get-pattern-groups)。

- **意向**：最简单的条件是单个意向。如果助手的自然语言处理在对用户的输入求值后，确定用户输入的目的已映射到预定义的意向，那么将使用相应节点。请使用语法 `#intent_name`。例如，`#weather` 会检查用户输入中是否询问了天气预报。如果是，那么将处理具有 `#weather` 意向条件的节点。

  有关意向的更多信息，请参阅[定义意向](/docs/services/assistant?topic=assistant-intents)。

- **特殊条件**：产品随附的可用于执行通用对话函数的条件。有关详细信息，请参阅下一部分中的**特殊条件**表。

### 特殊条件
{: #dialog-overview-special-conditions}

|条件语法|描述|
|----------------------|-------------|
|`anything_else`      |可以在对话末尾使用此条件，在用户输入与其他任何对话节点都不匹配时，将处理此条件。此条件将触发**其他**节点。|
|`conversation_start` |与 **welcome** 一样的是，此条件在第一轮对话期间会求值为 true。但与 **welcome** 不同的是，无论应用程序的初始请求是否包含用户输入，其都为 true。具有 **conversation_start** 条件的节点可用于在对话开始时初始化上下文变量或执行其他任务。|
|`false`              |此条件始终求值为 false。可以在正在开发的分支开始处使用此项，以阻止使用此分支，或者将其用作提供常见函数且仅用作**跳转至**操作目标的节点的条件。|
|`irrelevant`         |如果 {{site.data.keyword.conversationshort}} 服务确定用户的输入为不相关，那么此条件将求值为 true。|
|`true`               |此条件始终求值为 true。可以在节点或响应列表末尾使用此条件，以捕获与任何先前条件都不匹配的任何响应。|
|`welcome`            |仅当应用程序的初始请求不包含任何用户输入时，才会在第一轮对话期间（当会话启动时），将此条件求值为 true。在所有随后的多轮对话中，都会将其求值为 false。此条件会触发**欢迎**节点。通常，使用此条件的节点用于对用户进行问候，例如显示诸如`欢迎使用我们的披萨订购应用程序`之类的消息。在通过 Facebook 或 Slack 等通道进行的交互期间，永远不会处理此节点。|
{: caption="特殊条件" caption-side="top"}

### 条件语法详细信息
{: #dialog-overview-condition-syntax}

使用以下某个语法选项在条件中创建有效的表达式：

- 用于引用意向、实体和上下文变量的速记符号。请参阅[访问对象和对象求值](/docs/services/assistant?topic=assistant-expression-language)。

- Spring Expression (SpEL) 语言，这是一种表达式语言，用于支持在运行时查询和操作对象图形。有关更多信息，请参阅 [Spring Expression Language (SpEL) 语言 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。

可以使用正则表达式来检查是否有以其为条件的值。例如，要查找匹配的字符串，可以使用 `String.find` 方法。有关更多详细信息，请参阅[方法](/docs/services/assistant?topic=assistant-dialog-methods)。

## 响应
{: #dialog-overview-responses}

对话响应定义如何回复用户。

可以通过以下方式进行回复：

- [简单文本响应](#dialog-overview-simple-text)
- [富文本响应](#dialog-overview-multimedia)
- [条件响应](#dialog-overview-multiple)

### 简单文本响应
{: #dialog-overview-simple-text}

如果要提供文本响应，只需输入您希望助手向用户显示的文本即可。

![显示一个节点，其中显示用户提问“你们的店在哪里”，而对话响应为“我们没有实体店！但是，通过因特网连接，您可以在任何地方购买我们的产品！”](images/response-simple.png)

要在响应中包含上下文变量值，请使用语法 `$variable-name` 来指定上下文变量值。有关更多信息，请参阅[上下文变量](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context)。例如，如果您确认在处理节点之前，会将 $user 上下文变量设置为当前用户的名称，那么可以在此节点的文本响应中引用该变量，类似于：

```
您好，$user
```
{: screen}

如果当前用户的名称为 `Norman`，那么向 Norman 显示的响应为：`您好，Norman`。

如果在文本响应中包含其中一个特殊字符，请通过在其前面添加反斜杠 (``\`) 对其进行转义。如果使用的是 JSON 编辑器，那么需要使用两个反斜杠 (``\\`) 来进行转义。对字符进行转义会阻止助手将其错误地解释为下列其中一种工件类型：

|特殊字符|工件|示例|
|-------------------|----------|---------|
| `$` |上下文变量| `交易费用为 \$2。`|
| `@` |实体|`向我们发送反馈 (\@example.com)。`|
{: caption="在响应中要转义的特殊字符" caption-side="top"}

内置集成支持以下 Markdown 语法元素：

|格式|语法|示例|
|------------|--------|---------|
|斜体|`我们正在讨论*实践*。`|我们正在讨论*实践*。|
|粗体| `在棒球比赛中，**不许**哭泣。`|在棒球比赛中，**不许**哭泣。|
|超文本链接|`请联系我们：[ibm.com](https://www.ibm.com)。`|请联系我们：[ibm.com](https://www.ibm.com)。|
{: caption="支持的 Markdown 语法" caption-side="top"}

“试用”窗格目前不支持 Markdown 语法。但“预览”链接集成支持 Markdown 语法，因此可以在预览 Web 页面中测试对话，以了解 Markdown 语法的呈现方式。

“试用”窗格和“预览”链接集成都支持 HTML 语法。但 Slack 和 Facebook 集成不支持 HTML 语法。 

#### 了解有关简单响应的更多信息
{: #dialog-overview-variety}

- [添加多行](#dialog-overview-multiline)
- [添加变体](#dialog-overview-add-variety)

#### 添加多行
{: #dialog-overview-multiline}

如果希望单个文本响应包含以回车符分隔的多行，请执行以下步骤：

1.  将您希望向用户显示的每一行作为单独的语句添加到其自己的响应变体字段中。例如：

  <table>
  <caption>多行响应</caption>
  <tr>
    <th>响应变体</th>
  </tr>
  <tr>
    <td>嗨。</td>
  </tr>
  <tr>
    <td>今天感觉怎么样？</td>
  </tr>
  </table>

1.  对于响应变体设置，请选择**多行**。

    如果要使用在富文本响应类型支持添加到产品之前创建的对话技能，那么可能会看不到*多行*选项。在这种情况下，请向当前节点响应添加另一个文本响应类型。此操作会更改响应在底层 JSON 中表示的方式。因此，多行选项会变为可用。选择多行变体类型。现在，可以删除添加到响应中的这第二个文本响应类型。
    {: note}

向用户显示响应时，将显示这两个响应变体，每行一个，如下所示:

```
嗨。
今天感觉怎么样？
```
{: screen}

#### 添加变体
{: #dialog-overview-add-variety}

如果用户频繁返回到 Conversation 服务，那么若是他们每次都听到相同的问候和响应，可能会感到厌烦。为此，可以向响应添加*变体*，以便会话能够以不同方式对同一条件进行响应。

在此示例中，助手在响应有关门店位置的问题时，针对不同的交互提供不同的回答：

![显示一个节点，其中显示用户提问“你们的店在哪里”，而对话定义了三个不同的响应。](images/variety.png)

可以选择按顺序或按随机顺序循环提供响应变体。缺省情况下，响应按顺序循环，如同从排序的列表中进行选择一样。

要更改返回各个文本响应的顺序，请完成以下步骤：

1.  将响应的每个变体添加到其自己的响应变体字段中。例如：

  <table>
  <caption>变化的响应</caption>
  <tr>
    <th>响应变体</th>
  </tr>
  <tr>
    <td>您好。</td>
  </tr>
  <tr>
    <td>嗨。</td>
  </tr>
  <tr>
    <td>你好！</td>
  </tr>
  </table>

1.  对于响应变体设置，选择下列其中一个设置：

    - **顺序**：第一次触发对话节点时，系统会返回第一个响应变体，第二次触发节点时返回第二个响应变体，依此类推，响应变体返回的顺序与这些变体在节点中的定义顺序相同。

      将导致在处理节点时按以下顺序返回响应：

      - 第一次：

        ```
        您好。
        ```
        {: screen}

      - 第二次：

        ```
        嗨。
        ```
        {: screen}

      - 第三次：
        ```
        你好！
        ```
        {: screen}

    - **随机**：第一次触发对话节点时，系统会随机从变体列表中选择一个文本字符串，下次触发节点时，随机选择另一个变体，但不会连续重复选择相同的文本字符串。

      处理节点时可能返回响应的顺序示例：

      - 第一次：

        ```
        你好！
        ```
        {: screen}

      - 第二次：

        ```
        嗨。
        ```
        {: screen}

      - 第三次：

        ```
        您好。
        ```
        {: screen}

### 富文本响应
{: #dialog-overview-multimedia}

可以返回包含多媒体或交互式元素（例如，图像或可单击按钮）的响应，以简化应用程序的交互模型，并增强用户体验。

除了缺省响应类型**文本**（对于此类型，指定要作为响应返回给用户的文本）外，还支持以下响应类型：

- **连接到人工座席**：![仅限增强版或高端套餐](images/plus.png) 对话调用您指定的服务，通常是用于管理人工座席支持凭单队列的服务，以将会话传递给人员。您可以选择包含一条消息，用于概述要向人工座席提供的用户问题。外部服务负责显示该消息，向用户说明正在传输会话。对话不会管理该通信本身。在“试用”窗格中测试具有此响应类型的节点时，不会发生对话传输。您必须从测试部署访问使用此响应类型的节点，以了解用户将如何体验到此响应。

  此响应类型仅可供增强版或高端套餐用户使用，并且仅 Intercom 或定制应用程序集成支持此响应类型。
  {: note}

- **图像**：将图像嵌入到响应中。源图像文件必须在某个位置进行托管，并具有可用于引用该图像文件的 URL。这不能是存储在不可公开访问的目录中的文件。
- **选项**：添加包含一个或多个选项的列表。用户单击其中一个选项时，会向助手发送关联的用户输入值。根据对话的部署位置，选项的呈现方式会有所不同。例如，在一个集成通道中，选项可能会显示为可单击按钮，但在另一个集成通道中，可能会显示为下拉列表。
- **暂停**：强制应用程序等待指定的毫秒数，然后再继续处理。可以选择显示关于对话正在输入响应的指示符。如果需要执行可能需要一些时间的操作，请使用此响应类型。例如，父节点发出 Cloud Functions 调用，并在子节点中显示结果。您可以将此响应类型用作父节点的响应，以便给程序化调用一定时间来完成，然后跳转至子节点以显示结果。此响应类型不会在“试用”窗格中呈现。您必须从测试部署访问使用此响应类型的节点，以了解用户将如何体验到此响应。
- **搜索技能**：![仅限增强版或高端套餐](images/plus.png) 搜索外部数据源来获取相关信息以返回给用户。搜索的数据源是向使用此对话技能的助手添加搜索技能时配置的 {{site.data.keyword.discoveryshort}} 服务数据集合。有关更多信息，请参阅[创建搜索技能](/docs/services/assistant?topic=assistant-skill-search-add)。

  此响应类型仅可供增强版或高端套餐的用户使用。
  {: note}

#### 添加富文本响应
{: #dialog-overview-multimedia-add}

要添加富文本响应，请完成以下步骤：

1.  单击响应字段中的下拉菜单以选择响应类型，然后提供任何必需的信息：

    - **连接到人工座席**。![仅限增强版或高端套餐](images/plus.png) 您可以选择添加消息，以便与向其传输会话的人工座席共享。

        仅 Intercom 和定制应用程序集成支持此响应类型。对于定制应用程序，必须对客户机应用程序进行编程，以识别何时触发此响应类型。
        {: note}

    - **图像**。将托管的图像文件的完整 URL 添加到**图像源**字段中。图像必须为 .jpg、.gif 或 .png 格式。图像文件必须存储在可通过 URL 公开寻址到的位置中。

        例如：`https://www.example.com/assets/common/logo.png`。

        如果要在响应中嵌入式图像的上方显示图像标题和描述，请将其添加到提供的字段中。

        要访问存储在 {{site.data.keyword.cloud}}{{site.data.keyword.cos_short}} 中的图像，请启用对各个图像存储对象的公共访问权，然后通过类似下面的语法指定图像源来对其进行引用：`https://s3.eu.cloud-object-storage.appdomain.cloud/your-bucket-name/image-name.png`。

        Slack 集成需要标题。其他集成通道会忽略标题或描述。
        {: note}

    - **选项**。完成以下步骤：

      1.  单击**添加选项**。
      1.  在**列表标签**字段中，输入要在列表中显示的选项。标签的长度必须少于 64 个字符。
      1.  在对应的**值**字段中，输入用户输入，以便在选择此选项时，将该输入传递给助手。值的长度必须少于 2,048 个字符。

          指定您确信在提交时将触发正确意向的值。例如，值可能是来自意向训练数据中的用户示例。
      1.  重复先前的步骤以向列表添加更多选项。

          最多可以添加 20 个选项。
      1.  在**标题**字段中，添加列表简介。标题可以要求用户从选项列表中选取选项。

          某些集成通道不会显示标题。
          {: note}

      1.  可选择在**描述**字段中添加其他信息。如果指定了描述，那么描述会显示在标题之后，但位于选项列表之前。

      某些集成通道不会显示描述。
          {: note}

      例如，可以构造类似于以下内容的响应：

        <table>
        <caption>响应选项</caption>
        <tr>
          <th>列表标题</th>
          <th>列表描述</th>
          <th>选项标签</th>
          <th>单击时提交的用户输入</th>
        </tr>
        <tr>
          <td>保险类型</td>
          <td>您希望投保哪些项目？</td>
          <td></td>
          <td></td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>船舶</td>
          <td>我想买船舶保险。</td>
        </tr>
        <tr>
          <td></td>
          <td></td>
          <td>汽车</td>
          <td>我想买汽车保险。</td>
        </tr>
         <tr>
          <td></td>
          <td></td>
          <td>住宅</td>
          <td>我想买住宅保险。</td>
        </tr>
        </table>

    - **暂停**。将暂停持续的时间长度（单位为毫秒）添加到**持续时间**字段。

        值不能超过 10,000 毫秒。用户通常愿意等待某人大约 8 秒（8,000 毫秒）时间来输入响应。要阻止在暂停期间显示输入指示符，请选择**关闭**。

        在暂停后添加其他响应类型（例如，文本响应类型）即明确表示暂停结束。
        {: tip}

    - **文本**。在文本字段中添加要返回给用户的文本。可以选择文本响应的变体设置。有关更多详细信息，请参阅[简单文本响应](#dialog-overview-simple-text)。

    - **搜索技能**。![仅限增强版或高端套餐](images/plus.png) 指示要搜索外部数据源以获取相关响应。

      此响应类型仅向增强版或高端套餐用户显示。
      {: note}

      要编辑待传递到 {{site.data.keyword.discoveryshort}} 服务的搜索查询，请单击**定制**，然后填写以下字段：

        - **查询**：可选。可以使用自然语言来指定特定查询，以传递给 {{site.data.keyword.discoveryshort}}。如果未添加查询，那么客户的确切输入文本将作为查询传递。

          例如，可以指定 `What cities do you fly to?`。此查询值会作为搜索查询传递给 {{site.data.keyword.discoveryshort}}。{{site.data.keyword.discoveryshort}} 使用自然语言理解来理解查询，并在为搜索技能配置的数据集合中查找有关该主题的回答或相关信息。

          通过在查询中引用在用户输入中检测到的实体，可以包含用户提供的特定信息。例如，`Tell me about @product`。或者，可以引用上下文变量，例如 `Do you have flights to $destination?`。只需确保将对话设计为仅当在查询中引用的任何实体或上下文变量已设置为有效值时，才触发搜索。

          此字段等效于 {{site.data.keyword.discoveryshort}} `natural_language_query` 参数。有关更多信息，请参阅[查询参数 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-query-parameters#nlq){: new_window}。

        - **过滤器**：可选。指定文本字符串，用于定义在任何返回的搜索结果中必须存在的信息。

          - 例如，要指示希望仅返回检测到正面观点的文档，请指定 `enriched_text.sentiment.document.label:positive`。

          - 要过滤结果以仅包含摄入过程识别为包含实体 `Boston, MA` 的文档，请指定 `enriched_text.entities.text:"Boston, MA"`。

          - 要过滤结果以仅包含摄入过程识别为包含客户所提供产品名称的文档，可以指定 `enriched_text.entities.text:@product`。

          - 要过滤结果以仅包含摄入过程识别为包含 `$destination` 上下文变量中所保存的城市名称的文档，可以指定 `enriched_text.entities.text:$destination`。

        如果同时添加查询和过滤器值，那么将首先应用过滤器参数来过滤数据集合文档，并对结果进行高速缓存。然后，查询参数会对高速缓存的结果排名。 

        此字段等效于 {{site.data.keyword.discoveryshort}} `filter` 参数。有关更多信息，请参阅[查询参数 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](/docs/services/discovery?topic=discovery-query-parameters#filter){: new_window}。

      仅当添加了此对话技能的助手还具有与其关联的搜索技能时，此响应类型才会返回有效响应。通过预览链接或其他助手级别集成来测试此响应类型。无法在对话技能的“试用”窗格中测试此响应类型。

1.  单击**添加响应类型**，以将其他响应类型添加到当前响应。

    您可能希望向单个响应添加多个响应类型，以向用户查询提供更丰富的回答。例如，如果用户询问门店位置，您可以显示地图并为每个门店位置显示对应的按钮，用户可以单击相应按钮来获取地址详细信息。要构建该类型的响应，可以使用图像、选项和文本响应类型的组合。另一个示例是在暂停响应类型之前使用文本响应类型，以便可以在暂停对话之前向用户发送警告。

    向单个响应添加的响应类型不能超过 5 种。即，如果为一个对话节点定义了三个条件响应，那么每个条件响应可以添加的响应类型不超过 5 种。
    {: note}

    单个对话节点不能有多个**连接到人工座席**或多个**搜索技能**响应类型。
    {: note}

1.  如果添加了多个响应类型，那么可以单击**上移或下移**箭头，以按您希望助手处理响应类型的顺序来排列响应类型。

### 条件响应
{: #dialog-overview-multiple}

单个对话节点可以提供不同的响应，每个响应由不同的条件触发。使用此方法可在单个节点中应对多个场景。

<iframe class="embed-responsive-item" id="youtubeplayer1" title="添加条件响应" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/Q5_-f7_Iyvg?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

节点仍然具有一个主要条件，也就是可以使用该节点并处理其所包含的条件和响应的条件。

在此示例中，助手使用自己先前收集的有关用户位置的信息来定制其响应，并提供有关离该用户最近的门店的信息。有关如何存储从用户那里所收集的信息的更多信息，请参阅[上下文变量](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context)。

![显示一个节点，其中显示用户提问“你们的店在哪里”，而对话有三个不同的响应，根据条件决定使用哪个，条件使用 $state 上下文变量中的信息指定在这些州中的位置。](images/multiple-responses.png)

现在，此单个节点提供的功能相当于四个单独的节点。

要向节点添加条件响应，请完成以下步骤：

1.  单击**定制**，然后单击**多个响应**切换开关以将其**开启**。

    节点响应部分会更改为显示一对条件和响应字段。您可以在其中添加条件和响应。
1.  要进一步定制响应，请单击响应旁边的**编辑响应** ![编辑响应](images/edit-slot.png) 图标。

    要完成以下任务，必须打开响应进行编辑：

    - **更新上下文**。要在触发响应时更改上下文变量的值，请在上下文编辑器中指定上下文值。为每个单独的条件响应更新上下文；没有通用于所有条件响应的上下文编辑器或 JSON 编辑器。
    - **添加富文本响应**。要添加多个文本响应或要将除文本响应以外的响应类型添加到单个条件响应，必须打开编辑响应视图。
    - **配置跳转**。要指示助手在处理此条件响应后跳转至其他节点，请在响应编辑视图的*最后*部分中选择**跳转至**。确定希望助手接下来处理的节点。有关更多信息，请参阅[配置“跳转至”操作](#dialog-overview-jump-to-config)。

      在处理所有条件响应之前，不会处理为节点配置的**跳转至**操作。因此，如果将条件响应配置为跳转至其他节点，并触发条件响应，那么永远不会处理为该节点配置的跳转，因此不会发生该跳转。

1.  单击**添加响应**以添加其他条件响应。

节点中的条件按顺序进行求值，如同节点一样。请确保条件响应按正确的顺序列出。如果需要更改顺序，请选择一个条件和响应对，然后使用显示的箭头在列表中上移或下移该对。

## 定义后续操作
{: #dialog-overview-jump-to}

在进行指定的响应后，可以指示助手执行下列其中一个操作：

- **等待用户输入**：助手等待用户提供响应引出的新输入。例如，响应可能会询问用户“是”或“否”问题。在用户提供更多输入之前，此对话不会继续。
- **跳过用户输入**：希望不等待用户输入而改为直接转至当前节点的第一个子节点时，请使用此选项。

  当前节点必须具有至少一个子节点，此选项才可用。
  {: note}

- **跳转至其他对话节点**：希望会话直接转至完全不同的对话节点时，请使用此选项。例如，可以使用*跳转至*操作将流从树中的多个位置路由到一个常见对话节点。

  必须存在要跳转至的目标节点，然后才能配置“跳转至”操作来使用该节点。
  {: note}

### 配置“跳转至”操作
{: #dialog-overview-jump-to-config}

如果选择跳转至其他节点，请通过选择下列其中一个选项来指定何时处理目标节点：

- **条件**：如果语句的目标是所选对话节点的条件部分，那么助手会首先检查目标节点的条件是否求值为 true。
    - 如果条件求值为 true，那么系统会立即处理目标节点。
    - 如果条件未求值为 true，那么系统将移至目标节点的下一个同代节点以对其条件求值，并重复此过程，直至找到具有求值为 true 的条件的对话节点为止。

    - 如果系统处理了所有同代，仍然没有任何条件求值为 true，那么将使用基本回退策略，并且对话会对对话树基本级别的节点求值。

    将条件设置为目标对于链接对话节点的条件很有用。例如，您可能希望首先检查输入是否包含意向，例如 `#turn_on`；如果输入包含意向，那么您可能希望检查输入是否包含实体，例如 `@lights`、`@radio` 或 `@wips`。链接条件有助于构造更大的对话树。

    在配置从条件响应发生的“跳转至”以转至对话树中当前节点上方的节点时，请避免选择此选项。否则，可能会创建无限循环。如果助手跳转至较早的节点并检查其条件，那么可能会返回 false，因为正在对上次通过对话触发当前节点的相同用户输入求值。助手将转至下一个同代或返回根，以检查这些节点上的条件，结果可能再次触发此节点，这意味着过程将重复自身。
    {: note}

- **响应**：如果语句的目标是所选对话节点的响应部分，那么会立即运行。即，系统不会对所选对话节点的条件求值，而是立即处理所选对话节点的响应。

  将响应设置为目标对于将多个对话节点链接在一起很有用。响应的处理就像此对话节点的条件为 true 时一样。如果所选对话节点有其他**跳转至**操作，那么该操作也会立即运行。

- **等待用户输入**：等待用户的新输入，然后开始从您跳转至的节点对输入进行处理。例如，如果在源节点提出问题，而您希望跳转至其他节点来处理用户对该问题的回答，那么此选项很有用。

## 更多信息

有关对话使用的表达式语言及方法和系统实体的信息以及其他有用的详细信息，请参阅“导航”窗格中的**参考**部分。

您还可以使用 API 来添加节点，否则请编辑对话。有关更多信息，请参阅[使用 API 修改对话](/docs/services/assistant?topic=assistant-api-dialog-modify)。
