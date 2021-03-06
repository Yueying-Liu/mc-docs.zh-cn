---
title: 使用 Visual Studio Code 创建 C# 函数 - Azure Functions
description: 了解如何创建 C# 函数，然后使用 Visual Studio Code 中的 Azure Functions 扩展将本地项目发布到 Azure Functions 中的无服务器托管。
ms.topic: quickstart
ms.date: 11/17/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: bc7757bc5c96d13ddf9523f828a1ff0cfd81a912
ms.sourcegitcommit: b072689d006cbf9795612acf68e2c4fee0eccfbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "95970686"
---
# <a name="quickstart-create-a-c-function-in-azure-using-visual-studio-code"></a>快速入门：在 Azure 中使用 Visual Studio Code 创建 C# 函数

[!INCLUDE [functions-language-selector-quickstart-vs-code](../../includes/functions-language-selector-quickstart-vs-code.md)]

在本文中，我们使用 Visual Studio Code 来创建一个响应 HTTP 请求的基于 C# 类库的函数。 在本地测试代码后，将代码部署到 Azure Functions 的无服务器环境。

完成本快速入门会从你的 Azure 帐户中扣取最多几美分的费用。

本文还有一个[基于 CLI 的版本](create-first-function-cli-csharp.md)。

## <a name="configure-your-environment"></a>配置环境

在开始之前，请确保已满足下列要求：

+ 具有活动订阅的 Azure 帐户。 [创建帐户](https://www.azure.cn/pricing/1rmb-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。

+ [Node.js](https://nodejs.org/)，在 Windows 上使用 npm 时所必需的。 仅限[活动 LTS 和维护 LTS 版本](https://nodejs.org/about/releases/)。 可以使用 `node --version` 命令检查你的版本。
    对于 macOS 和 Linux 上的本地开发而言，这不是必需的。

+ 安装在某个[受支持的平台](https://code.visualstudio.com/docs/supporting/requirements#_platforms)上的 [Visual Studio Code](https://code.visualstudio.com/)。

+ Visual Studio Code 的 [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)。  

+ Visual Studio Code 的 [Azure Functions 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>创建本地项目

在本部分，你将使用 Visual Studio Code 在 C# 中创建一个本地 Azure Functions 项目。 稍后在本文中，你要将函数代码发布到 Azure。

1. 在活动栏中选择“Azure”图标，然后在“Azure:函数”区域中选择“创建新项目...”图标。

    ![选择“创建新项目”](./media/functions-create-first-function-vs-code/create-new-project.png)

1. 为项目工作区选择目录位置，然后选择“选择”  。

    > [!NOTE]
    > 这些步骤已设计为在工作区之外完成。 在这种情况下，请不要选择属于工作区内的项目文件夹。

1. 根据提示提供以下信息：

    + **选择函数项目的语言**：选择`C#`。

    + **为项目的第一个函数选择模板**：选择`HTTP trigger`。

    + **提供函数名称**：键入 `HttpExample`。

    + **提供命名空间**：键入 `My.Functions`。

    + **授权级别**：选择 `Anonymous`，这使任何人都可以调用你的函数终结点。 若要了解授权级别，请参阅[授权密钥](functions-bindings-http-webhook-trigger.md#authorization-keys)。

    + **选择打开项目的方式**：选择`Add to workspace`。

1. Visual Studio Code 将使用此信息生成一个包含 HTTP 触发器的 Azure Functions 项目。 可以在资源管理器中查看本地项目文件。 若要详细了解所创建的文件，请参阅[生成的项目文件](functions-develop-vs-code.md#generated-project-files)。

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

确认该函数可以在本地计算机上正确运行以后，可以使用 Visual Studio Code 将项目直接发布到 Azure。

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

[!INCLUDE [functions-vs-code-run-remote](../../includes/functions-vs-code-run-remote.md)]

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code.md)]

## <a name="next-steps"></a>后续步骤

你已使用 Visual Studio Code 通过简单的 HTTP 触发函数创建了函数应用。 在下一篇文章中，将通过添加输出绑定来扩展该函数。 此绑定将 HTTP 请求中的字符串写入 Azure 队列存储队列中的消息。 

> [!div class="nextstepaction"]
> [连接到 Azure 存储队列](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-csharp)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions

