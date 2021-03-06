---
title: 在 Azure Key Vault 中创建和检索密钥的属性 - Azure CLI
description: 以快速入门的方式介绍如何使用 Azure CLI 在 Azure Key Vault 中设置和检索密钥
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
origin.date: 03/30/2020
ms.date: 11/27/2020
ms.author: v-tawe
ms.custom: devx-track-azurecli
ms.openlocfilehash: e913fa5b2e70342d48406601436294210936e87e
ms.sourcegitcommit: b6fead1466f486289333952e6fa0c6f9c82a804a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2020
ms.locfileid: "96300473"
---
# <a name="quickstart-set-and-retrieve-a-key-from-azure-key-vault-using-azure-cli"></a>快速入门：使用 Azure CLI 在 Azure Key Vault 中设置和检索密钥

在本快速入门中，你将使用 Azure CLI 在 Azure Key Vault 中创建一个密钥保管库。 Azure Key Vault 是一项云服务，用作安全的机密存储。 可以安全地存储密钥、密码、证书和其他机密。 有关 Key Vault 的详细信息，可以参阅[概述](../general/overview.md)。 Azure CLI 用于通过命令或脚本创建和管理 Azure 资源。 完成该操作后，你将存储密钥。

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


<!-- [!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)] -->

 - 本快速入门需要 Azure CLI 版本 2.0.4 或更高版本。

## <a name="create-a-resource-group"></a>创建资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。 以下示例在“chinanorth”  位置创建名为“ContosoResourceGroup”  的资源组。

```azurecli
az group create --name "ContosoResourceGroup" --location chinanorth
```

## <a name="create-a-key-vault"></a>创建密钥保管库

接下来，将在上一步创建的资源组中创建 Key Vault。 需要提供某些信息：

- 对于本快速入门，请使用 **Contoso-vault2**。 必须在测试中提供唯一名称。
- 资源组名称 **ContosoResourceGroup**。
- 位置：**中国北部**。

```azurecli
az keyvault create --name "Contoso-Vault2" --resource-group "ContosoResourceGroup" --location chinanorth
```

此 cmdlet 的输出显示新创建的 Key Vault 的属性。 请记下下面列出的两个属性：

- **保管库名称**：在本示例中，此项为 **Contoso-Vault2**。 会在其他 Key Vault 命令中使用此名称。
- **保管库 URI**：在本示例中，此项为 https://contoso-vault2.vault.azure.cn/ 。 通过其 REST API 使用保管库的应用程序必须使用此 URI。

目前，只有你的 Azure 帐户才有权对这个新保管库执行任何操作。

## <a name="add-a-key-to-key-vault"></a>向 Key Vault 添加密钥

只需再执行几个步骤便可向保管库中添加密钥。 此密钥可供应用程序使用。 

键入以下命令，创建名为 ExampleKey  的密钥：

```azurecli
az keyvault key create --vault-name "Contoso-Vault2" -n ExampleKey --protection software
```

现在，可以通过 URI 来引用已添加到 Azure Key Vault 的此密钥。 使用 **https://Contoso-Vault2.vault.azure.cn/keys/ExampleKey** 获取当前版本。 

若要查看以前存储的密钥，请使用以下命令：

```azurecli

az keyvault key show --name "ExampleKey" --vault-name "Contoso-Vault2"
```

现在，你已创建了一个密钥保管库，存储了一个密钥并检索了该密钥。

## <a name="clean-up-resources"></a>清理资源

本系列中的其他快速入门和教程是在本快速入门的基础上制作的。 如果打算继续使用后续的快速入门和教程，则可能需要保留这些资源。
如果不再需要资源组和所有相关的资源，可以使用 [az group delete](/cli/group) 命令将其删除。 可以删除资源，如下所示：

```azurecli
az group delete --name ContosoResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本快速入门中，你创建了 Key Vault 并在其中存储了一个密钥。 若要详细了解 Key Vault 以及如何将其与应用程序集成，请继续阅读以下文章。

- 阅读 [Azure Key Vault 概述](../general/overview.md)
- 请参阅 [Azure CLI az keyvault 命令](/cli/keyvault)参考
- 查看 [Azure Key Vault 最佳做法](../general/best-practices.md)
