---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-03"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:pre: .pre}
{:tip: .tip}
{:screen: .screen}

# 了解用户概要文件
{: #user-profile}

通过 {{site.data.keyword.appid_full}}，您可以通过访问 {{site.data.keyword.appid_short_notm}} 存储的用户相关信息来构建个性化应用程序体验。
{: shortdesc}

## 重要概念
{: #key-concepts}

**什么是用户概要文件？**

用户概要文件是 {{site.data.keyword.appid_short_notm}} 存储的属性集合。属性是有关与应用程序交互的用户的信息片段。可获取两种类型的属性：`预定义`和`定制`。

**什么是预定义属性？**

在用户登录到应用程序时身份提供者返回预定义属性。这些属性可能包含他们的用户名、年龄或性别。

**什么是定制属性？**

在用户与应用程序交互期间了解到关于用户的定制属性。可能是用户使用的字体大小或放入购物车中的项目。定制属性可进行编辑。

## 访问用户属性
{: #access}

可通过不同的方式来访问用户概要文件信息。在用户认证成功后，应用程序收到访问和身份令牌。身份令牌包含身份提供者返回的用户属性的标准化子集。要获取完整的用户属性列表，可使用 OIDC `/userinfo` 端点。要管理定制属性，可以使用 `REST API`。用户信息和定制属性端点均受 {{site.data.keyword.appid_short_notm}} 在认证流程结束时生成的访问令牌的保护。



![{{site.data.keyword.appid_short_notm}} 用户概要文件体系结构](/images/user-profile1.png)

图. 用户概要文件信息流程

要查看定制属性，您可以使用 <a href="https://appid-profiles.ng.bluemix.net/swagger-ui/index.html#/Attributes" target="_blank">REST API <img src="../../icons/launch-glyph.svg" alt="外部链接图标"></a>。

</br>
</br>
