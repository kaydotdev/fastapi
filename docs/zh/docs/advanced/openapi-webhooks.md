# OpenAPI 网络钩子

有些情况下，您可能想告诉您的 API **用户**，您的应用程序可以携带一些数据调用*他们的*应用程序（给它们发送请求），通常是为了**通知**某种**事件**。

这意味着，除了您的用户向您的 API 发送请求的一般情况，**您的 API**（或您的应用）也可以向**他们的系统**（他们的 API、他们的应用）**发送请求**。

这通常被称为**网络钩子**（Webhook）。

## 使用网络钩子的步骤

通常的过程是**您**在代码中**定义**要发送的消息，即**请求的主体**。

您还需要以某种方式定义您的应用程序将在**何时**发送这些请求或事件。

**用户**会以某种方式（例如在某个网页仪表板上）定义您的应用程序发送这些请求应该使用的 **URL**。

所有关于注册网络钩子的 URL 的**逻辑**以及发送这些请求的实际代码都由您决定。您可以在**自己的代码**中以任何想要的方式来编写它。

## 使用 `FastAPI` 和 OpenAPI 文档化网络钩子

使用 **FastAPI**，您可以利用 OpenAPI 来自定义这些网络钩子的名称、您的应用可以发送的 HTTP 操作类型（例如 `POST`、`PUT` 等）以及您的应用将发送的**请求体**。

这能让您的用户更轻松地**实现他们的 API** 来接收您的**网络钩子**请求，他们甚至可能能够自动生成一些自己的 API 代码。

/// info

网络钩子在 OpenAPI 3.1.0 及以上版本中可用，FastAPI `0.99.0` 及以上版本支持。

///

## 带有网络钩子的应用程序

当您创建一个 **FastAPI** 应用程序时，有一个 `webhooks` 属性可以用来定义网络钩子，方式与您定义*路径操作*的时候相同，例如使用 `@app.webhooks.post()` 。

{* ../../docs_src/openapi_webhooks/tutorial001.py hl[9:13,36:53] *}

您定义的网络钩子将被包含在 `OpenAPI` 的架构中，并出现在自动生成的**文档 UI** 中。

/// info

`app.webhooks` 对象实际上只是一个 `APIRouter` ，与您在使用多个文件来构建应用程序时所使用的类型相同。

///

请注意，使用网络钩子时，您实际上并没有声明一个*路径*（比如 `/items/` ），您传递的文本只是这个网络钩子的**标识符**（事件的名称）。例如在 `@app.webhooks.post("new-subscription")` 中，网络钩子的名称是 `new-subscription` 。

这是因为我们预计**您的用户**会以其他方式（例如通过网页仪表板）来定义他们希望接收网络钩子的请求的实际 **URL 路径**。

### 查看文档

现在您可以启动您的应用程序并访问 <a href="http://127.0.0.1:8000/docs" class="external-link" target="_blank">http://127.0.0.1:8000/docs</a>.

您会看到您的文档不仅有正常的*路径操作*显示，现在还多了一些**网络钩子**：

<img src="/img/tutorial/openapi-webhooks/image01.png">