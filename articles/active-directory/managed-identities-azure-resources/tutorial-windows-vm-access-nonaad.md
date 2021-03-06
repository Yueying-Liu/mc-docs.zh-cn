---
title: 教程：使用托管标识访问 Azure Key Vault - Windows - Azure AD
description: 本教程将指导你完成使用 Windows VM 系统分配的托管标识访问 Azure Key Vault 的过程。
services: active-directory
documentationcenter: ''
author: barclayn
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/23/2020
ms.author: v-junlch
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fa31551706cecf48ab7eb7b4e95e028baec653a
ms.sourcegitcommit: 7ad3bfc931ef1be197b8de2c061443be1cf732ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91245355"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-key-vault"></a>教程：使用 Windows VM 系统分配的托管标识访问 Azure Key Vault 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

本教程介绍了如何使用 Windows 虚拟机 (VM) 系统分配的托管标识来访问 Azure Key Vault。 作为引导，Key Vault 随后可让客户端应用程序使用机密访问未受 Azure Active Directory (AD) 保护的资源。 托管服务标识由 Azure 自动管理，可用于向支持 Azure AD 身份验证的服务进行身份验证，这样就无需在代码中插入凭据了。 

学习如何：


> [!div class="checklist"]
> * 授予 VM 对 Key Vault 中存储的密钥的访问权限 
> * 使用 VM 标识获取访问令牌，并使用它来检索 Key Vault 中的密钥 

## <a name="prerequisites"></a>先决条件

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="enable"></a>启用

[!INCLUDE [msi-tut-enable](../../../includes/active-directory-msi-tut-enable.md)]



## <a name="grant-access"></a>授予访问权限  
 
本部分说明如何授予 VM 访问密钥保管库中存储的密钥的权限。 使用 Azure 资源的托管标识，代码可以获取访问令牌，对支持 Azure AD 身份验证的资源进行身份验证。  但是，并非所有 Azure 服务都支持 Azure AD 身份验证。 若要将 Azure 资源的托管标识用于这些服务，请将服务凭据存储在 Azure Key Vault 中，然后使用 VM 的托管标识访问 Key Vault 以检索凭据。 

首先，我们需要创建一个 Key Vault 并授予 VM 的系统分配托管标识对 Key Vault 的访问权限。   

1. 在左侧导航栏的顶部，依次选择“创建资源”   > “安全 + 标识”   > “Key Vault”  。  
2. 为新 Key Vault 提供一个名称  。 
3. 定位到之前创建的 VM 所在的订阅和资源组中的 Key Vault。 
4. 选择“访问策略”  ，然后单击“添加新”  。 
5. 在模板中的“配置”中，选择“密钥管理”  。 
6. 选择“选择主体”  ，并在搜索字段中输入之前创建的 VM 的名称。  选择结果列表中的 VM，并单击“选择”  。 
7. 单击“确定”  完成添加新的访问策略，然后单击“确定”  完成访问策略选择。 
8. 单击“创建”  完成创建 Key Vault。 

    ![Alt 图像文本](./media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


接下来，将密钥添加到 Key Vault，以便稍后可以使用在 VM 中运行的代码检索此密钥： 

1. 选择“所有资源”  ，找到并选择已创建的 Key Vault。 
2. 选择“密钥”  ，然后单击“添加”  。 
3. 从“上传选项”  中选择“手动”  。 
4. 输入密钥的名称和值。  该值可以是任何需要的内容。 
5. 明确指定激活日期和到期日期，并将“已启用”  设置为“是”  。 
6. 单击“创建”  以创建密钥。 
 
## <a name="access-data"></a>访问数据  

本部分介绍如何使用 VM 标识获取访问令牌并使用它从 Key Vault 中检索机密。 如果未安装 PowerShell 4.3.1 或更高版本，则需要[下载并安装最新版本](https://docs.microsoft.com/powershell/azure/)。

首先，我们使用 VM 的系统分配托管标识获取访问令牌，向 Key Vault 证明身份：
 
1. 在门户中，导航到“虚拟机”  并转到 Windows 虚拟机，然后在“概述”  中，单击“连接”  。
2. 输入创建 Windows VM  时添加的用户名  和密码  。  
3. 现在，已经创建了与虚拟机的远程桌面连接  ，请在远程会话中打开 PowerShell。  
4. 在 PowerShell 中，调用租户上的 Web 请求，为 VM 特定端口中的本地主机获取令牌。  

    PowerShell 请求：
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.cn' -Method GET -Headers @{Metadata="true"} 
    ```
    
    接下来，提取完整响应，响应以 JavaScript 对象表示法 (JSON) 格式字符串的形式存储在 $response 对象中。  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    接下来，从响应中提取访问令牌。  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    最后，使用 PowerShell 的 Invoke-WebRequest 命令检索之前在 Key Vault 中创建的密钥，在授权标头中传递访问令牌。  将需要 Key Vault 的 URL，该 URL 位于 Key Vault 的“概述”  页的“软件包”  部分。  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    响应将如下所示： 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.cn/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
在检索 Key Vault 中的密钥后，可以使用该密钥对需要名称和密码的服务进行身份验证。 


## <a name="disable"></a>禁用

[!INCLUDE [msi-tut-disable](../../../includes/active-directory-msi-tut-disable.md)]



## <a name="next-steps"></a>后续步骤

在本教程中，你已学习了如何使用 Windows VM 系统分配的托管标识来访问 Azure Key Vault。  若要详细了解 Azure Key Vault，请参阅：

> [!div class="nextstepaction"]
>[Azure Key Vault](../../key-vault/general/overview.md)

