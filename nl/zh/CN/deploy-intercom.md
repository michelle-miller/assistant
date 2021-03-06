---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# 与 Intercom 集成 ![仅限增强版或高端套餐](images/plus.png)
{: #deploy-intercom}

Intercom 是一种客户消息传递平台，通过在整个客户生命周期内改善关系来帮助推动业务增长。
{: shortdesc}

Intercom 通过与 IBM 合作，向客户支持团队添加了新的座席，即虚拟 Watson Assistant。您可以将助手与 Intercom 应用程序相集成，以支持应用程序在助手和人工座席之间无缝地传递用户会话。要了解有关集成的更多信息，请阅读 [Watson 博客帖子 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8)。

此集成仅可供增强版或高端套餐用户使用。
如果您想试用，可以注册免费的增强试用版套餐。[获取增强试用版](https://cloud.ibm.com/registration?target=%2Fdeveloper%2Fwatson%2Flaunch-tool%2Fconversation%3Fplan%3Dplus-trial&cm_mmc=OSocial_Voicestorm-_-Watson+AI_Watson+Core+-+Conversation-_-WW_WW-_-Intercom+Trial+Registration+Link&cm_mmca1=000027BD&cm_mmca2=10004432)。
{: note}

如果将助手与 Intercom 集成，那么 Intercom 应用程序会成为可供技能使用的面向客户的应用程序。与用户的所有交互都由 Intercom 发起和管理。

目前还没有方法可将正在进行的会话从一个集成通道传递到另一个集成通道。

## 一次性座席创建
{: #deploy-intercom-account-prereq}

将 Intercom 集成添加到助手之前，您或您组织中的某人必须完成以下一次性先决条件步骤。

1.  为助手创建功能电子邮件帐户。

    每个助手都必须具有有效的唯一电子邮件地址，才能将其添加到 Intercom 中的团队。
1.  在 Intercom 工作空间中，将助手作为新座席添加到团队。

    转至 Intercom 工作空间中的“队友设置”页面，通过将上一步中创建的电子邮件添加到“邀请”字段，邀请助手成为新座席。

    如果尚未设置 Intercom 工作空间，请在 [www.intercom.com ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.intercom.com) 上创建 Intercom 工作空间。至少需要预订 Intercom 的 *Inbox* 产品才能创建工作空间。

1.  在早先创建的助手电子邮件帐户中，找到来自 Intercom 的邀请。单击电子邮件中的链接以加入团队。使用助手的功能电子邮件地址进行注册，然后加入团队。

1.  **可选**：更新助手的概要文件。

    可以编辑助手的名称和概要文件图片。在工作空间内的专用座席通信中，以及在通过 Intercom 应用程序与客户进行的公共交互中，此概要文件代表助手。请创建反映您品牌的概要文件。

    单击导航栏中的 Intercom 概要文件图标可访问概要文件和工作空间设置。

    ![Intercom 的“设置”页面的屏幕快照。](images/intercom-settings.png)

## 准备对话
{: #deploy-intercom-dialog-prereq}

如果您没有与助手关联的对话技能，请立即创建对话技能或向助手添加对话技能。有关更多详细信息，请参阅[构建对话](/docs/services/assistant?topic=assistant-dialog-build)。

Intercom 集成目前不支持通过搜索技能来触发搜索。
{: note}

在对话技能中完成以下步骤，以便助手可以处理用户请求，并可以在客户要求时将会话传递给人工座席。

1.  向技能添加意向，用于识别用户要求与人工进行交谈的请求。

    您可以创建自己的意向，也可以添加随 IBM 开发的**常规**内容目录一起提供的名为 `#General_Connect_to_Agent` 的预构建意向。

1.  向对话添加一个根节点，该节点以在上一步中创建的意向为条件。选择**连接到人工座席**作为响应类型。

1.  准备要由助手通过 Intercom 应用程序触发的每个对话分支。

    对话中的每个根对话节点都可以由充当 Intercom 队友的助手进行处理，包括文件夹中的根节点。在配置 Intercom 集成时，将指定日后希望助手为每个对话分支执行的操作。因此，尽管无法在 Intercom 中隐藏节点，但可以将助手配置为在触发该节点时不执行任何操作。

    填写每个对话分支的根节点的以下字段：

    - **节点名**：为节点提供名称。此名称用于日后在为节点配置交互时标识该节点。如果未添加名称，那么必须改为基于节点标识来选择节点。
    - **外部节点名**：添加对话分支目的的摘要。例如，*查找门店*。

      助手要求回答用户查询时，会向助手团队中的其他座席显示此信息。如果有多个对话节点可以处理该查询，那么助手会与人工座席队友共享响应选项列表，以获取有关要使用哪个响应的建议。

      ![在节点编辑视图中添加节点目的摘要的字段的屏幕快照。](images/disambig-node-purpose.png)

      **不要**向步骤 2 中创建的根节点添加外部节点名。发生上报时，助手会查看最后一个处理的节点的外部节点名，以了解未成功满足的是哪个用户目标。如果在有“连接到人工座席”意向的节点中包含外部节点名，那么将阻止助手了解用户在上报问题前，与之交互的最后一个目标明确的实际节点。
     {: tip}

1.  如果分支中的某个子节点以您不希望助手处理的跟进请求或问题为条件，请向该节点添加**连接到人工座席**响应类型。

    例如，一些节点涉及只应由人工处理的敏感问题，或负责跟踪助手总是无法理解用户意图的情况，您可能希望将此响应类型添加到此类节点。

    在运行时，如果会话到达此子节点，那么对话将在此时传递给人工座席。稍后，设置 Intercom 集成时，可以选择人工座席作为每个分支的替补。

现在，对话已准备好在 Intercom 中支持助手。

### 对话注意事项
{: #deploy-intercom-dialog}

添加到对话的某些富文本响应在“试用”窗格中的显示方式与向 Intercom 用户显示的方式不同。下表描述了 Intercom 如何处理响应类型。

|响应类型|向 Intercom 用户的显示方式|
|---------------|---------------------------|
|**选项**|选项显示为编号列表。在**标题**或**描述**字段中，提供向用户说明如何从列表中选择一个选项的指示信息。|
|**图像**|将呈现图像**标题**、**描述**和图像本身。|
|**暂停**|无论是否启用此类型，在暂停期间都不会显示正在输入指示符。|

有关响应类型的更多信息，请参阅[富文本响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)。

## 添加 Intercom 集成
{: #deploy-intercom-add-intercom}

1.  在“助手”选项卡中，单击以打开要部署的助手的磁贴。

1.  在“集成”部分中，单击**添加集成**。

1.  单击 **Intercom**。

    按照屏幕上提供的指示信息进行操作。以下各部分将帮助您执行这些步骤。

以下 4 分钟的视频说明了相关步骤。

<iframe class="embed-responsive-item" id="youtubeplayer" title="快速设置" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/SkbFWNScueU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 将助手连接到 Intercom
{: #deploy-intercom-connect}

授予 Intercom 使用助手的许可权后，该助手即成为 Intercom 团队的有效成员。

人工座席可以使用 Intercom 的分配规则将消息分配给助手。消息可以通过以下方式分配给助手：

- 根据某些条件自动将入站会话分配给队友或团队收件箱

  以下一分半钟的视频说明了相关步骤。

  <iframe class="embed-responsive-item" id="youtubeplayer2" title="自动分配" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/4M9wu8NHxcY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- 人工座席在运行时手动进行了重新分配。

  以下不到 3 分钟的视频说明了相关步骤。

  <iframe class="embed-responsive-item" id="youtubeplayer3" title="手动分配" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/jAnolyUJAIA" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

有关更多详细信息，请参阅 [Intercom 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams)。

1.  对话准备就绪后，单击**立即连接**。
1.  单击**访问 Intercom** 以重定向到 Intercom 站点。

     使用助手的（而不是您自己的）功能电子邮件地址和密码登录到 Intercom。您希望建立与组织中多人共享的功能标识的连接。
     {: important}

1.  单击**授予访问权**。
1.  单击**返回到概述**。

现在，助手可用于接收来自 Intercom 队友的分配。如果还不能接收，请[设置对话](#deploy-intercom-dialog-prereq)。
{: important}

## 配置消息路由
{: #deploy-intercom-config-backup}

将人工队友指定为助手的替补，以防助手需要将进行中的会话转移给人工。可以选择其他团队或队友作为每个对话分支的替补联系人。

要设置从助手上报到人工的路由分配，请完成以下步骤：

1.  在 Intercom 集成页面中，单击**我的对话技能已就绪**以确认您已准备好对话。

    仅当完成[准备对话](#deploy-intercom-dialog-prereq)过程后，才单击此按钮。
    {: important}

1.  在*设置*部分中，单击**管理规则**。

    如果不进行任何更改，那么所有节点的替补联系人都会保持未分配状态。

1.  单击**新建规则**。

1.  从*选择节点*下拉列表中，选择要配置的对话分支的节点。

    请记住，分支由其节点名进行标识。如果未指定节点名，那么将改为显示节点的标识。

1.  选择团队或人工座席队友作为此对话分支的替补联系人。如果助手无法回答用户查询，或命中具有*连接到人工座席*响应类型（从而指示该查询只应由人工回答）的子节点，那么该查询将上报到此人。

1.  要为其他对话分支定义路由规则，请再次单击**新建规则**，然后重复上述步骤。

    对于其分支中的子节点中具有*连接到人工座席*响应类型的任何根节点，不要忘记为这些根节点设置分配。如果未将关联的根节点转移到特定人员或团队，那么敏感问题可能会转移到*未分配*收件箱。

1.  添加规则后，单击**返回到概述**以退出页面。

以下 3 分钟的视频说明了相关步骤。

<iframe class="embed-responsive-item" id="youtubeplayer0" title="基于主题的上报路线" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/dTwJZOqdzII" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 授予助手监视和回答用户查询的许可权
{: #deploy-intercom-config-action}

希望助手开始监视 Intercom 收件箱，并自行回答消息时，请开启监视功能。

助手会监视在 Intercom 中记录的用户查询。助手确信自己知道如何回答用户查询时，会直接响应用户。（助手识别到最热门意向的置信度分数等于或高于 0.75 时，即说明助手确信。）

如果不希望助手回答某些类型的用户查询，那么可以添加规则，按对话分支指定助手要执行的其他操作。例如，您可能希望一开始以更保守的方式将助手合并到 Intercom 团队中，仅允许助手建议响应，并将用户消息转移给其他队友进行回答。一段时间后，助手证明自己的能力后，可以给予助手更多责任。

要配置希望助手如何处理特定对话分支，请定义规则。

1.  在 Intercom 集成页面的*允许助手监视收件箱*部分中，将监视功能切换为**开启**。

1.  在*设置*中，单击**管理规则**。

    如果未定义规则，那么会将助手配置为监视*未分配*收件箱，并在助手确信自己可以处理用户查询时自动进行回答。

1.  在*监视 Intercom 收件箱*字段中，选择希望助手监视的 Intercom 收件箱。

1.  单击**新建规则**，为特定对话分支定义唯一交互模式。

1.  从*选择节点*下拉列表中，选择要配置的分支的节点。

    请记住，分支由其节点名进行标识。如果未指定节点名，那么将改为显示节点的标识。

1.  选取在触发此对话节点时希望助手执行的操作类型。操作类型选项如下：

    - **不执行任何操作**：助手不参与响应；用户的消息会保留在收件箱中，以供其他人处理。
    - **发送给团队或队友**：助手对用户输入进行评估以确定其目标，然后将其转移给相应的队友。
    - **向团队或队友提供建议**：助手通过内部 Intercom 应用程序与人工座席共享注释，从而为队友提供关于如何响应的建议。

      - 如果用户输入触发了一个对话分支，即具有表示全面交互的子节点的根对话节点，那么助手会指示自己能够处理该请求，并要求这样做。
      - 如果用户输入触发了没有子代的根节点，那么助手只会与人工座席共享来自该节点的已编程响应，而不会直接响应用户。
      - 如果输入触发了多个具有高置信度的对话节点，那么助手会向人工队友显示可能响应的列表，并要求队友选择最佳响应。

      对于上述每种情况，人工座席都可决定是否允许助手接管。

    - **回答**：助手直接响应用户，而不与其他任何队友协商。

    对于为其分配了*发送到团队或队友*或者*向团队或队友提供建议*操作类型的任何节点，都需要队友参与。请确保返回并添加一个规则，用于将合适的人员或团队专门指定为此对话节点的替补。

1.  要为其他对话分支定义唯一交互设置，请再次单击**新建规则**，然后重复上述步骤。

1.  添加规则后，单击**返回到概述**以退出页面。

随着对话的更改，您可能要返回到 Intercom 集成页面，对这些规则进行递增更改。

以下 3 分钟的视频说明了相关步骤。

<iframe class="embed-responsive-item" id="youtubeplayer1" title="收件箱监视" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/fFKjWUfIftw" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## 测试集成
{: #deploy-intercom-try}

要有效地测试端到端的 Intercom 集成，您必须有权访问 Intercom 最终用户应用程序。您已创建或编辑了 Intercom 工作空间。该工作空间必须具有关联的用户界面客户机。如果没有，请参阅 [Apps in Intercom ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.intercom.com/help/apps-in-intercom){: new_window} 以获取有关设置客户机的帮助。

通过与 Intercom 工作空间关联的客户机应用程序来提交测试用户查询，以了解 Intercom 如何处理消息。验证应该由助手回答的消息是否生成了合适的响应，并且助手是否未响应自己未配置为回答的消息。
