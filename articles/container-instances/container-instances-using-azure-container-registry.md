---
title: 从 Azure 容器注册表部署容器映像
description: 了解如何通过从 Azure 容器注册表拉取容器映像在 Azure 容器实例中部署容器。
services: container-instances
ms.topic: article
origin.date: 07/02/2020
author: rockboyfor
ms.date: 11/30/2020
ms.author: v-yeche
ms.custom: mvc, devx-track-azurecli
ms.openlocfilehash: 66992cbf6f60fd552f9300e7683c0f320b4ac591
ms.sourcegitcommit: ea52237124974eda84f8cef4bf067ae978d7a87d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96024419"
---
<!--Verified successfully-->
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>从 Azure 容器注册表部署到 Azure 容器实例

[Azure 容器注册表](../container-registry/container-registry-intro.md)是基于 Azure 的托管容器注册表服务，用于存储专用的 Docker 容器映像。 本文介绍如何在部署到 Azure 容器实例时拉取 Azure 容器注册表中存储的容器映像。 建议配置注册表访问权限的方法是创建 Azure Active Directory 服务主体和密码，并将登录凭据存储在 Azure 密钥保管库中。

## <a name="prerequisites"></a>先决条件

**Azure 容器注册表**：需要一个 Azure 容器注册表（注册表中的至少一个容器映像）才能完成本文中的步骤。 如果需要注册表，请参阅[使用 Azure CLI 创建容器注册表](../container-registry/container-registry-get-started-azure-cli.md)。

**Azure CLI**：本文中的命令行示例使用 [Azure CLI](https://docs.azure.cn/cli/)，并采用适用于 Bash shell 的格式。 可以在本地[安装 Azure CLI](https://docs.azure.cn/cli/install-azure-cli)。

<!--Not Available on [Azure Cloud Shell][cloud-shell-bash]-->

## <a name="limitations"></a>限制

* 无法使用在同一容器组中配置的[托管标识](container-instances-managed-identity.md)向 Azure 容器注册表进行身份验证，以便在容器组部署期间拉取图像。
* 目前，无法从部署到 Azure 虚拟网络的 Azure 容器注册表拉取映像。

<!--Not Available on [Azure Container Registry](../container-registry/container-registry-vnet.md)-->

## <a name="configure-registry-authentication"></a>配置注册表身份验证

在生产方案中，如果要提供对“无外设”服务和应用程序的访问权限，建议使用[服务主体](../container-registry/container-registry-auth-service-principal.md)配置注册表访问权限。 使用服务主体可以提供对容器映像的 [Azure 基于角色的访问控制 (Azure RBAC)](../container-registry/container-registry-roles.md)。 例如，可将服务主体配置为拥有注册表的仅限提取的访问权限。

Azure 容器注册表提供了附加的[身份验证选项](../container-registry/container-registry-authentication.md)。

在以下部分中，将创建一个 Azure 密钥保管库和一个服务主体，并将服务主体的凭据存储在保管库中。

### <a name="create-key-vault"></a>创建 Key Vault

如果 [Azure Key Vault](../key-vault/general/overview.md) 中没有保管库，请在 Azure CLI 中使用以下命令创建一个保管库。

将 `RES_GROUP` 变量更新为要在其中创建 Key Vault 的现有资源组的名称，将 `ACR_NAME` 更新为容器注册表的名称。 为简洁起见，本文中的命令假设你的注册表、密钥保管库和容器实例都是在同一资源组中创建的。

 在 `AKV_NAME` 中指定新 Key Vault 的名称。 保管库名称必须在 Azure 中唯一、长度必须为 3-24 个字母数字字符、以字母开头、以字母或数字结尾，并且不能包含连续的连字符。

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>创建服务主体并存储凭据

现在请创建服务主体，并将其凭据存储在密钥保管库中。

以下命令使用 [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] 创建服务主体，使用 [az keyvault secret set][az-keyvault-secret-set] 将服务主体的 **密码** 存储在保管库中。

```azurecli
# Create service principal, store its password in vault (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name http://$ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)
```

上述命令中的 `--role` 参数使用“acrpull”角色配置服务主体，该角色授予其对注册表的只拉取访问权限。 若要同时授予推送和拉取访问权限，请将 `--role` 参数更改为“acrpush”。

接下来，将服务主体的 *appId*（传递给 Azure 容器注册表用于身份验证的 **用户名**）存储在保管库中。

```azurecli
# Store service principal ID in vault (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

现已创建 Azure 密钥保管库并在其中存储了两个机密：

* `$ACR_NAME-pull-usr`：用作容器注册表 **用户名** 的服务主体 ID。
* `$ACR_NAME-pull-pwd`：用作容器注册表 **密码** 的服务主体密码。

现在，当你或你的应用程序和服务从注册表提取映像时，可以按名称引用这些机密。

## <a name="deploy-container-with-azure-cli"></a>使用 Azure CLI 部署容器

将服务主体凭据存储到 Azure Key Vault 机密中后，应用程序和服务可以使用它们来访问专用注册表。

首先，使用 [az acr show][az-acr-show] 命令获取注册表的登录服务器名称。 登录服务器名称全部小写，并且类似于 `myregistry.azurecr.cn`。

```azurecli
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --resource-group $RES_GROUP --query "loginServer" --output tsv)
```

执行以下 [az container create][az-container-create] 命令来部署容器实例。 该命令使用 Azure Key Vault 中存储的服务主体凭据对容器注册表进行身份验证，并假设事先已将 [aci-helloworld](container-instances-quickstart.md) 映像推送到注册表。 如果想要使用注册表中的不同映像，请更新 `--image` 值。

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_LOGIN_SERVER/aci-helloworld:v1 \
    --registry-login-server $ACR_LOGIN_SERVER \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

`--dns-name-label` 值必须在 Azure 中唯一，因此，上述命令会将一个随机数字追加到容器的 DNS 名称标签。 该命令的输出显示容器的完全限定域名 (FQDN)，例如：

```output
"aci-demo-25007.chinaeast2.azurecontainer.console.azure.cn"
```

成功启动容器后，可在浏览器导航到容器的 FQDN，以验证应用程序是否成功运行。

## <a name="deploy-with-azure-resource-manager-template"></a>使用 Azure 资源管理器模板进行部署

通过将 `imageRegistryCredentials` 属性包含到容器组定义中，可以在 Azure 资源管理器模板中指定 Azure 容器注册表的属性。 例如，可以直接指定注册表凭据：

```JSON
[...]
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
[...]
```

<!--Not Available on [Resource Manager template reference](https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2019-12-01/containerGroups)-->

有关在资源管理器模板中引用 Azure Key Vault 机密的详细信息，请参阅[在部署过程中使用 Azure Key Vault 传递安全参数值](../azure-resource-manager/templates/key-vault-parameter.md)。

## <a name="deploy-with-azure-portal"></a>使用 Azure 门户仅部署

如果在 Azure 容器注册表中维护容器映像，可通过 Azure 门户在 Azure 容器实例中轻松创建容器。 使用门户从容器注册表部署容器实例时，必须启用注册表的[管理员帐户](../container-registry/container-registry-authentication.md#admin-account)。 管理员帐户专门用于单个用户访问注册表，主要用于测试目的。 

1. 在 Azure 门户中，导航到容器注册表。

1. 若要确保启用管理员帐户，请选择“访问密钥”，然后在“管理员用户”下选择“启用”。

1. 选择“存储库”，然后选择想要从中进行部署的存储库，右键单击想要部署的容器映像的标记，然后选择“运行实例”。

    ![Azure 门户中 Azure 容器注册表中的“运行实例”][acr-runinstance-contextmenu]

1. 输入容器和资源组的名称。 也可根据需要更改默认值。

    ![Azure 容器实例的创建菜单][acr-create-deeplink]

1. 部署完成后，可从通知窗格导航至容器组，查找其 IP 地址和其他属性。

    ![Azure 容器实例容器组的详细信息视图][aci-detailsview]

## <a name="next-steps"></a>后续步骤

有关 Azure 容器注册表身份验证的详细信息，请参阅[使用 Azure 容器注册表进行身份验证](../container-registry/container-registry-authentication.md)。

<!-- IMAGES -->

[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->

<!--Not Available on [cloud-shell-bash]: https://shell.azure.com/bash-->
<!--Not Available on [cloud-shell-try-it]: https://shell.azure.com/powershell-->

<!-- LINKS - Internal -->

[az-acr-show]: https://docs.azure.cn/cli/acr#az_acr_show
[az-ad-sp-create-for-rbac]: https://docs.azure.cn/cli/ad/sp#az_ad_sp_create_for_rbac
[az-container-create]: https://docs.microsoft.com/cli/azure/container#az_container_create
[az-keyvault-secret-set]: https://docs.azure.cn/cli/keyvault/secret#az_keyvault_secret_set

<!-- Update_Description: update meta properties, wording update, update link -->