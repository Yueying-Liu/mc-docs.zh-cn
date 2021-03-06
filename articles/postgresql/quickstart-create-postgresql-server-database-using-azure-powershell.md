---
title: 快速入门：创建服务器 - Azure PowerShell - Azure Database for PostgreSQL（单一服务器）
description: 使用 Azure PowerShell 创建 Azure Database for PostgreSQL（单一服务器）的快速入门指南。
author: WenJason
ms.author: v-jay
ms.service: postgresql
ms.devlang: azurepowershell
ms.topic: quickstart
origin.date: 06/08/2020
ms.date: 11/09/2020
ms.custom: mvc, devx-track-azurepowershell
ms.openlocfilehash: cb29007bcfc062d40253aa7a41db77ad96ff744e
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328021"
---
# <a name="quickstart-create-an-azure-database-for-postgresql---single-server-using-powershell"></a>快速入门：使用 PowerShell 创建 Azure Database for PostgreSQL - 单一服务器

本快速入门介绍如何使用 PowerShell 在 Azure 资源组中创建 Azure Database for PostgreSQL 服务器。 可以使用 PowerShell 以交互方式或者通过脚本创建和管理 Azure 资源。

## <a name="prerequisites"></a>先决条件

如果没有 Azure 订阅，请在开始前创建一个[试用帐户](https://wd.azure.cn/pricing/1rmb-trial-full)。

本文要求安装 Az PowerShell 模块，并使用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 连接到 Azure 帐户。 有关安装 Az PowerShell 模块的详细信息，请参阅[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)。

> [!IMPORTANT]
> 尽管 Az.PostgreSql PowerShell 模块为预览版，但必须使用以下命令从 Az PowerShell 模块单独安装它：`Install-Module -Name Az.PostgreSql -AllowPrerelease`。

如果这是你第一次使用 Azure Database for PostgreSQL 服务，必须注册 Microsoft.DBforPostgreSQL 资源提供程序。

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.DBforPostgreSQL
```

如果有多个 Azure 订阅，请选择应当计费的资源所在的相应订阅。 使用 [Set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) cmdlet 选择特定的订阅 ID。

```azurepowershell
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>创建资源组

使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) cmdlet 创建 [Azure 资源组](../azure-resource-manager/management/overview.md)。 资源组是在其中以组的形式部署和管理 Azure 资源的逻辑容器。

以下示例在“中国北部 2”区域中创建名为“myresourcegroup”的资源组。

```azurepowershell
New-AzResourceGroup -Name myresourcegroup -Location chinanorth2
```

## <a name="create-an-azure-database-for-postgresql-server"></a>创建 Azure Database for PostgreSQL 服务器

使用 `New-AzPostgreSqlServer` cmdlet 创建 Azure Database for PostgreSQL 服务器。 一个服务器可以管理多个数据库。 通常，每个项目或每个用户使用一个单独的数据库。

下表列出了 `New-AzPostgreSqlServer` cmdlet 的常用参数和示例值。

|        **设置**         | **示例值** |                                                                                                                                                             **说明**                                                                                                                                                              |
| -------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 名称                       | mydemoserver     | 选择用于标识 Azure Database for PostgreSQL 服务器的在 Azure 中全局唯一的名称。 该服务器名称只能包含字母、数字和连字符 (-)。 在创建过程中，指定的任何大写字符将自动转换为小写。 必须包含 3 到 63 个字符。 |
| ResourceGroupName          | myresourcegroup  | 提供 Azure 资源组的名称。                                                                                                                                                                                                                                                                                            |
| SKU                        | GP_Gen5_2        | SKU 的名称。 请遵循简写约定：pricing-tier\_compute-generation\_vCores。 有关 SKU 参数的详细信息，请参阅此表格后面的信息。                                                                                                                                           |
| BackupRetentionDay         | 7                | 备份保留时间。 单位为天。 范围为 7-35。                                                                                                                                                                                                                                                                       |
| GeoRedundantBackup         | Enabled          | 是否应为此服务器启用异地冗余备份。 不能为基本定价层级中的服务器启用此值，并且在创建服务器后无法更改此值。 允许的值：“Enabled”、“Disabled”。                                                                                                      |
| 位置                   | chinanorth2      | 服务器的 Azure 区域。                                                                                                                                                                                                                                                                                                         |
| SslEnforcement             | Enabled          | 是否应该为此服务器启用 SSL。 允许的值：“Enabled”、“Disabled”。                                                                                                                                                                                                                                                 |
| StorageInMb                | 51200            | 服务器的存储容量（以 MB 为单位）。 有效的 StorageInMb 最小为 5120 MB，以 1024 MB 为增量递增。 有关存储大小限制的详细信息，请参阅 [Azure Database for PostgreSQL 定价层](./concepts-pricing-tiers.md)。                                                                               |
| 版本                    | 9.6              | PostgreSQL 主版本。                                                                                                                                                                                                                                                                                                                 |
| AdministratorUserName      | myadmin          | 管理员的登录用户名。 不能是 **azure_superuser** 、 **admin** 、 **administrator** 、 **root** 、 **guest** 或 **public** 。                                                                                                                                                                                            |
| AdministratorLoginPassword | `<securestring>` | 管理员用户的密码，采用安全字符串格式。 该密码必须包含 8 到 128 个字符。 密码必须包含以下三个类别的字符：英文大写字母、英文小写字母、数字和非字母数字字符。                                       |

Sku 参数值遵循 pricing-tier\_compute-generation\_vCores 约定，如以下示例所示 。

- `-Sku B_Gen5_1` 映射到基本、第 5 代和 1 个 vCore。 此选项是可用的最小 SKU。
- `-Sku GP_Gen5_32` 映射到常规用途、第 5 层和 32 个 vCore。
- `-Sku MO_Gen5_2` 映射到内存优化、第 5 层和 2 个 vCore。

有关各区域和层级的有效 Sku 值的信息，请参阅 [Azure Database for PostgreSQL 定价层](./concepts-pricing-tiers.md)。

以下示例使用服务器管理员登录名“myadmin”在“中国北部 2”区域中的“myresourcegroup”资源组内创建名为“mydemoserver”的 PostgreSQL 服务器   。 此服务器是常规用途定价层中的第 5 代服务器，其中启用了 2 个 vCore 和异地冗余备份。 记下示例的第一行中使用的密码，因为这是 PostgreSQL 服务器管理员帐户的密码。

> [!TIP]
> 服务器名称映射到 DNS 名称，必须在 Azure 中全局唯一。

```azurepowershell
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -GeoRedundantBackup Enabled -Location chinanorth2 -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

如果轻量级计算和 I/O 足以满足工作负荷要求，请考虑使用基本定价层级。

> [!IMPORTANT]
> 在基本定价层中创建的服务器以后无法扩展到常规用途或内存优化层级，并且无法异地复制。

## <a name="configure-a-firewall-rule"></a>配置防火墙规则

使用 `New-AzPostgreSqlFirewallRule` cmdlet 创建 Azure Database for PostgreSQL 服务器级防火墙规则。 服务器级防火墙规则允许外部应用程序（如 `psql` 命令行工具或 PostgreSQL Workbench）穿过 Azure Database for PostgreSQL 服务防火墙连接到服务器。

以下示例创建名为 AllowMyIP 的防火墙规则，该规则允许从特定的 IP 地址 (192.168.0.1) 进行连接。 替换与要从中进行连接的位置相对应的 IP 地址或 IP 地址范围。

```azurepowershell
New-AzPostgreSqlFirewallRule -Name AllowMyIP -ResourceGroupName myresourcegroup -ServerName mydemoserver -StartIPAddress 192.168.0.1 -EndIPAddress 192.168.0.1
```

> [!NOTE]
> 连接到 Azure Database for PostgreSQL 时，经端口 5432 进行通信。 如果尝试从企业网络内部进行连接，则可能不允许经端口 5432 的出站流量。 这种情况下，只有在 IT 部门打开了端口 5432 时，才能连接到服务器。

## <a name="get-the-connection-information"></a>获取连接信息

若要连接到服务器，需要提供主机信息和访问凭据。 使用以下示例来确定连接信息。 记下 FullyQualifiedDomainName 和 AdministratorLogin 的值。

```azurepowershell
Get-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup |
  Select-Object -Property FullyQualifiedDomainName, AdministratorLogin
```

```Output
FullyQualifiedDomainName                    AdministratorLogin
------------------------                    ------------------
mydemoserver.postgres.database.chinacloudapi.cn       myadmin
```

## <a name="connect-to-postgresql-database-using-psql"></a>使用 psql 连接到 PostgreSQL 数据库

如果客户端计算机已安装 PostgreSQL，则可以使用 [psql](https://www.postgresql.org/docs/current/static/app-psql.html) 的本地实例连接到 Azure PostgreSQL 服务器。

1. 使用 `psql` 命令行实用工具连接到 Azure PostgreSQL 服务器。

   ```azurepowershell
   psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
   ```

   例如，以下命令使用访问凭据连接到 PostgreSQL 服务器 `mydemoserver.postgres.database.chinacloudapi.cn` 上名为“postgres”的默认数据库。 提示输入密码时，输入之前选择的 `<server_admin_password>`。

   ```azurepowershell
   psql --host=mydemoserver.postgres.database.chinacloudapi.cn --port=5432 --username=myadmin@mydemoserver --dbname=postgres
   ```

   > [!TIP]
   > 如果更喜欢使用 URL 路径连接到 Postgres，则 URL 会使用 `%40` 对用户名中的 @ 符号进行编码。 例如，psql 的连接字符串将是 `psql postgresql://myadmin%40mydemoserver@mydemoserver.postgres.database.chinacloudapi.cn:5432/postgres`

1. 连接到服务器后，在出现提示时创建空数据库。

   ```sql
   CREATE DATABASE mypgsqldb;
   ```

1. 出现提示时，请执行以下命令，将连接切换到新建的数据库 mypgsqldb：

   ```sql
   \c mypgsqldb
   ```

## <a name="connect-to-the-postgresql-server-using-pgadmin"></a>使用 pgAdmin 连接到 PostgreSQL 服务器

pgAdmin 是用于 PostgreSQL 的开源工具。 可以从 [pgAdmin 网站](https://www.pgadmin.org/)安装 pgAdmin。 你所使用的 pgAdmin 版本可能不同于本快速入门中使用的版本。 如果需要更多指南，请阅读 pgAdmin 文档。

1. 在客户端计算机上打开 pgAdmin 应用程序。

1. 从工具栏转到“对象”，将鼠标指针悬停在“创建”上，然后选择“服务器”。

1. 在“创建 - 服务器”对话框中的“常规”选项卡上，为服务器输入唯一的友好名称，例如 **mydemoserver** 。

   :::image type="content" source="./media/quickstart-create-postgresql-server-database-using-azure-powershell/9-pgadmin-create-server.png" alt-text="“常规”选项卡":::

1. 在“创建 - 服务器”对话框中的“连接”选项卡上，填写设置表。

   :::image type="content" source="./media/quickstart-create-postgresql-server-database-using-azure-powershell/10-pgadmin-create-server.png" alt-text="“连接”选项卡":::

    pgAdmin 参数 |值|说明
    ---|---|---
    主机名/地址 | 服务器名称 | 此前在创建用于 PostgreSQL 的 Azure 数据库服务器时使用过的服务器名称值。 示例服务器为 **mydemoserver.postgres.database.chinacloudapi.cn** 。 请使用完全限定的域名 ( **\*.postgres.database.chinacloudapi.cn** )，如示例中所示。 如果不记得服务器名称，请按上一部分的步骤操作，以便获取连接信息。
    端口 | 5432 | 连接到用于 PostgreSQL 的 Azure 数据库服务器时使用的端口。
    维护数据库 | postgres | 系统生成的默认数据库名称。
    用户名 | 服务器管理员登录名 | 此前在创建用于 PostgreSQL 的 Azure 数据库服务器时提供的服务器管理员登录用户名。 如果不记得用户名，请按上一部分的步骤操作，以便获取连接信息。 格式为 username\@servername。
    密码 | 管理员密码 | 之前在此快速入门中创建服务器时选择的密码。
    角色 | 留空 | 此时无需提供角色名称。 此字段留空。
    SSL 模式 | *必需* | 可以在 pgAdmin 的 SSL 选项卡中设置 TLS/SSL 模式。默认情况下，所有 Azure Database for PostgreSQL 服务器在创建时都会启用“强制实施 TLS”。 若要禁用“强制实施 TLS”，请参阅[配置 TLS 的强制实施](./concepts-ssl-connection-security.md#configure-enforcement-of-tls)。

1. 选择“保存”。

1. 在左侧的“浏览器”窗格中，展开“服务器”节点 。 选择服务器，例如 **mydemoserver** 。 单击该服务器与它建立连接。

1. 展开服务器节点，然后展开其下的“数据库”。 此列表应包括现有 *postgres* 数据库和已创建的任何其他数据库。 可以使用 Azure Database for PostgreSQL 为每个服务器创建多个数据库。

1. 右键单击“数据库”，选择“创建”菜单，并选择“数据库”。

1. 在“数据库”字段中键入所选的数据库名称，例如“mypgsqldb2”。

1. 从列表框中选择数据库的“所有者”。 选择服务器管理员登录名，例如“my admin”。

   :::image type="content" source="./media/quickstart-create-postgresql-server-database-using-azure-powershell/11-pgadmin-database.png" alt-text="在 pgAdmin 中创建数据库":::

1. 选择“保存”创建新的空白数据库。

1. 在“浏览器”窗格中，可以在服务器名称下的数据库列表中看到创建的数据库。

## <a name="clean-up-resources"></a>清理资源

如果其他快速入门或教程不需要使用本快速入门中创建的资源，可以运行以下示例将其删除。

> [!CAUTION]
> 以下示例删除指定的资源组及其包含的所有资源。
> 如果指定的资源组中存在本快速入门范围外的资源，这些资源也会被删除。

```azurepowershell
Remove-AzResourceGroup -Name myresourcegroup
```

若要仅删除本快速入门中创建的服务器而不删除资源组，请使用 `Remove-AzPostgreSqlServer` cmdlet。

```azurepowershell
Remove-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 PowerShell 设计 Azure Database for PostgreSQL](tutorial-design-database-using-powershell.md)
