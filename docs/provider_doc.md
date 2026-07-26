# 服务商

**文档更新时间：2026年7月26日**

为了让您能够自由接入最适合自己的 AI 模型，无论是 OpenAI 官方、DeepSeek、Codex API、Claude / Anthropic，还是各类兼容协议的中转服务，Anywhere Desktop 都提供了灵活且强大的 **服务商管理系统**。

本文档将说明如何在桌面端配置 API 连接、管理模型列表，并高效组织多个服务商来源。

---

## 1. 添加与配置服务商

在主控台的 **服务商** 页面中，您可以管理所有模型来源。

### 1.1 添加服务商

1. 点击左下角的 **“+ 添加服务商”**。
2. 输入一个易识别的名称，例如：`OpenAI 官方`、`DeepSeek`、`公司内部 API`。

### 1.2 核心参数配置

选中左侧某个服务商后，在右侧进行详细配置：

* **API 密钥 (API Key)**：
  * 输入服务商提供的 Key；
  * 支持多个 Key，用英文逗号 `,` 分隔；
  * 桌面端会在请求时做随机轮询，降低单 Key 限流风险；
  * API Key 属于敏感信息。导出、备份或分享配置前，请确认文件仅交给可信对象。
* **API URL (Base URL)**：
  * 通常填写服务商的基础接口地址；
  * 常见示例：
    * OpenAI：`https://api.openai.com/v1`
    * DeepSeek：`https://api.deepseek.com/v1`
    * Anthropic：`https://api.anthropic.com`
    * 自建中转：`https://api.example.com/v1`
* **协议类型 (Protocol)**：
  * **OpenAI Compatible**：适合 OpenAI 官方、DeepSeek 与大多数 OpenAI 风格中转；
  * **Codex API**：适合接入 Codex API 风格请求；
  * **Claude / Anthropic API**：适合直接或通过兼容网关接入 Anthropic Claude 模型。
* **自定义 Header**：
  * 可为当前服务商添加额外请求头，例如 `User-Agent`、网关鉴权字段、企业代理所需标识等；
  * Header 会随模型列表获取与聊天请求一起转发；
  * Header 可能包含鉴权信息，请勿填写无关敏感数据，也不要将包含它的配置文件公开分享。
* **启用开关**：停用后，该服务商及其模型将不再出现在模型选择中。

---

### 1.3 协议选择建议

不同服务商的请求格式并不完全相同，建议按以下规则选择：

* 如果服务商声明“兼容 OpenAI”，优先选择 **OpenAI Compatible**。
* 如果服务商要求 Codex API 请求体或会话参数，选择 **Codex API**。
* 如果直接使用 Claude / Anthropic 官方接口，或网关明确提供 Anthropic 协议，选择 **Claude / Anthropic API**。

> 如果模型列表能获取但聊天失败，通常需要同时检查协议类型、Base URL、模型 ID 与自定义 Header 是否匹配服务商要求。

## 2. 模型管理

### 2.1 自动获取（推荐）

点击 **“从 API 获取模型”** 按钮：

* 桌面端会尝试访问 `/models` 接口；
* 若成功，会弹出模型列表供您勾选；
* 选中的模型会加入当前服务商的可用模型列表。

### 2.2 手动添加

如果服务商不支持列出模型，或您想手动添加自定义模型：

1. 点击 **“手动添加”**；
2. 输入准确的模型 ID，例如 `gpt-4o`、`deepseek-chat`、`claude-3-5-sonnet-20240620`。

### 2.3 排序与整理

* **拖拽排序**：调整模型显示顺序；
* **删除模型**：点击标签上的删除按钮即可移除。

---

## 3. 文件夹与分组

当服务商较多时，可以使用文件夹管理：

* **创建文件夹**：新建目录用于归类不同服务商；
* **移动服务商**：把指定服务商移入某个目录；
* **管理文件夹**：支持折叠、重命名、删除。

删除文件夹不会删除服务商，只会把服务商移回根目录。

---

## 4. 排序与优先级

* **服务商排序**：可通过上移 / 下移按钮调整展示顺序；
* **默认选择策略**：历史对话继续聊天时，若原模型仍存在，会优先复用原服务商与模型。

---

## 5. 常见问题 (FAQ)

**Q1: 为什么“从 API 获取模型”失败？**

> **A:**
>
> 1. 检查 API URL 是否正确；
> 2. 检查 API Key 是否有效；
> 3. 有些中转不支持 `/models`，可改用手动添加。

**Q2: 自定义 Header 适合在什么情况下使用？**

> **A:** 当服务商或企业网关要求附加 `User-Agent`、组织标识、代理鉴权字段等请求头时，可以在 Provider 中配置。普通用户如果没有明确要求，通常无需填写。

**Q3: Claude 协议使用报错怎么回事？**

> **A:** 请以服务商提供的 Base URL 为准。通常 **OpenAI Compatible** 接口使用带 `/v1` 的基础地址；**Claude / Anthropic API** 则填写 Anthropic 风格的基础地址，不要为了保持一致而额外拼接 `/v1`。同时检查协议类型、模型 ID 与自定义 Header 是否匹配网关要求。

**Q4: 什么是 OpenAI 兼容格式？**

> **A:** Anywhere Desktop 的主通信链路基于 OpenAI 风格接口。只要某个服务商提供兼容输入输出结构的接口，通常就可以接入。

**Q5: 多 Key 轮询是怎么工作的？**

> **A:** 当您填写多个 Key 时，桌面端每次发请求会随机选择其中一个，达到简单负载均衡和分摊限流的效果。

**Q6: 删除服务商后，旧对话还能继续吗？**

> **A:** 历史消息内容不会丢失，但继续对话时若原服务商已不存在，系统会分配启用的第一个模型。

---

如需更多帮助，请访问开源社区或联系开发者。

[GITHUB 开源项目 Anywhere Desktop](https://github.com/Komorebi-yaodong/anywheredesktop)
[GITHUB 开源项目 Anywhere Desktop 文档](https://github.com/Komorebi-yaodong/anywhere_doc)
[GITEE 文档镜像仓库](https://gitee.com/Komorebi-yaodong/anywhere_)
QQ群：1065512489