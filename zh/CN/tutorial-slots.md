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

# 教程：向对话添加带槽的节点
{: #tutorial-slots}

在本教程中，您将向对话节点添加槽，以便在单个节点内从用户那里收集多条信息。您创建的节点将收集进行餐厅预订所需的信息。
{: shortdesc}

## 学习目标
{: #tutorial-slots-objectives}

完成本教程后，您将了解如何执行以下操作：

- 定义对话所需的意向和实体
- 向对话节点添加槽
- 测试带槽的节点

### 持续时间
{: #tutorial-slots-duration}

完成本教程大约需要 30 分钟。

### 先决条件
{: #tutorial-slots-prereqs}

开始之前，请先完成[入门教程](/docs/services/assistant?topic=assistant-getting-started)。您将使用您所创建的 {{site.data.keyword.conversationshort}} 教程技能，向您在入门练习中构建的简单对话添加节点。

如果您愿意，也可以从新的对话技能开始。只要确保在开始本教程之前已创建好技能即可。
{: note}

## 步骤 1：添加意向和示例
{: #tutorial-slots-add-intent}

在“意向”选项卡上添加意向。意向是用户输入中表达的目的或目标。您将添加 #reservation 意向，用于识别指示用户想进行餐厅预订的用户输入。

1.  在教程技能的**意向**页面中，单击**添加意向**。
1.  添加以下意向名称，然后单击**创建意向**：

    ```json
    reservation
    ```
    {: screen}

    这将添加 #reservation 意向。编号符号 (`#`) 会插入到意向名称开头以将其标注为意向。这种命名约定有助于您和其他人将该意向识别为意向。此意向还没有与之关联的示例用户发声。
1.  在**添加用户示例**字段中，输入以下发声，然后单击**添加示例**：

    ```json
    我要订位
    ```
    {: screen}

1.  添加以下其他示例，以帮助 Watson 识别 `#reservation` 意向。

    ```json
    我要预订晚餐
    我们有 3 位，午餐有位吗？
    下周三 7 点有位吗？
    周二晚上，4 位，有位吗？
    我想订明天的早午餐
    我能订位吗？
    ```
    {: screen}

1.  单击**关闭** ![关闭箭头](images/close_arrow.png) 图标以完成添加 `#reservation` 意向及其示例发声的操作。

## 步骤 2：添加实体
{: #tutorial-slots-add-entity}

实体定义包含一组实体*值*，用于表示在给定意向的上下文中经常使用的词汇表。通过定义实体，可以帮助服务识别用户输入中与相关意向有关系的引用。在此步骤中，将启用可以识别时间、日期和数字引用的系统实体。

1.  单击**实体**以打开“实体”页面。
1.  启用可以识别用户输入中的日期、时间和数字引用的系统实体。 单击**系统实体**选项卡，然后启用这些实体：

    - `@sys-time`
    - `@sys-date               `
    - `@sys-number               `

您已成功启用 @sys-date、@sys-time 和 @sys-number 系统实体。现在，可以在对话中使用这些实体。

## 步骤 3：添加带槽的对话节点
{: #tutorial-slots-add-dialog-with-slots}

对话节点表示服务与用户之间的对话线程的开始。它包含为使服务能够处理该节点而必须满足的条件。它至少还包含一个响应。例如，节点条件可能会在用户输入中查找 `#hello` 意向，并响应`您好。有什么能为您效劳的吗？`此示例是最简单形式的对话节点，包含单个条件和单个响应。可以通过向单个节点添加条件响应，添加延长与用户的交流的子节点等等，从而定义复杂的对话。（如果要了解有关复杂对话的更多信息，可以完成[构建复杂对话](/docs/services/assistant?topic=assistant-tutorial)教程。）

您将在此步骤中添加的节点是包含槽的节点。槽提供了一种结构化格式，通过该格式，可以在单个节点内向用户询问并保存来自用户的多条信息。当您打算执行特定的任务，并且在执行该任务之前需要用户的关键信息时，槽是最有用的。有关更多信息，请参阅[使用槽收集信息](/docs/services/assistant?topic=assistant-dialog-slots)。

添加的节点将收集在餐厅进行预订所需的信息。

1.  单击**对话**选项卡以打开对话树。
1.  单击 **#General_Greetings** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**。
1.  在“条件”字段中开始输入 `#reservation`，然后从列表中进行相应选择。如果用户输入与 `#reservation` 意向匹配，那么将对此节点求值。
1.  单击**定制**，单击**槽**切换开关以将其**开启**，然后单击**应用**。

    ![显示在其中开启槽的定制对话。](images/slots-toggle-on.png)
1.  定义以下槽：

    <table>
    <caption>槽详细信息</caption>
    <tr>
      <th>检查对象</th>
      <th>另存为</th>
      <th>如果不存在，请询问</th>
    </tr>
    <tr>
      <td>@sys-date               </td>
      <td>$date</td>
      <td>您要订哪天？</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>您要订在几点？</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number               </td>
      <td>$guests</td>
      <td>几位就餐？</td>
    </tr>
    </table>

1.  将响应指定为`好。我将为您订位，$guests 位，$date $time。`

    ![显示在工具中为每个槽和响应填充指定内容后的结果视图。](images/slots-simple-node.png)

1.  单击 ![关闭](images/close.png) 以关闭节点编辑视图。

## 步骤 4：测试对话
{: #tutorial-slots-test}

1.  选择 ![询问 Watson](images/ask_watson.png) 图标以打开交谈窗格。
1.  输入`我要订位`。

    助手识别到 #reservation 意向后，以第一个槽的提示进行响应：`您要订哪天？`。

1.  输入`周五`。

    助手识别到该值后，会将其用于填充第一个槽的 $date 上下文变量。然后会显示下一个槽的提示：`您要订在几点？`

1.  输入`下午 5 点`。

    助手识别到该值后，会将其用于填充第二个槽的 $time 上下文变量。然后会显示下一个槽的提示：`几位就餐？`

1.  输入 `6 位`。

    助手识别到该值后，会将其用于填充第三个槽的 $guests 上下文变量。现在所有的槽都已填充，将显示节点响应`好。我将为您订位，6 位，2017-12-29 17:00:00。`

![显示“试用”窗格中的测试对话，该对话已成功填充各个节点槽。](images/slots-test-simple-node.png)

运行正常！祝贺您。您已成功创建带槽的节点。

## 摘要
{: #tutorial-slots-summary}

在本教程中，您创建了一个带槽的节点，可以捕获在餐厅订位时所需的信息。

## 后续步骤
{: #tutorial-slots-next-steps}

改善与节点交互的用户的体验。完成后续教程[改进带槽的节点](/docs/services/assistant?topic=assistant-tutorial-slots-complex)。此教程涵盖简单的改进，例如如何重新设置系统返回的日期 (2017-12-28) 和时间 (17:00:00) 值的格式。此外，还涵盖更复杂的任务，例如，如果用户未提供对话期望的槽值的类型，该怎么做。
