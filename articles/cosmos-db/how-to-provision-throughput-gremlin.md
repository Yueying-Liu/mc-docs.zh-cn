---
title: 在 Azure Cosmos DB Gremlin API 资源上预配吞吐量
description: 了解如何在 Azure Cosmos DB Gremlin API 资源中预配容器、数据库和自动缩放吞吐量。 你将使用 Azure 门户、CLI、PowerShell 和其他各种 SDK。
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: how-to
origin.date: 10/15/2020
author: rockboyfor
ms.date: 11/09/2020
ms.testscope: yes|no
ms.testdate: 11/09/2020null
ms.author: v-yeche
ms.custom: devx-track-js, devx-track-azurecli, devx-track-csharp
ms.openlocfilehash: 4f260ed78a1d47ef0eec7a2692c9ffadddb3e345
ms.sourcegitcommit: 6b499ff4361491965d02bd8bf8dde9c87c54a9f5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94328979"
---
<!--Verified successfully on portal-->
# <a name="provision-database-container-or-autoscale-throughput-on-azure-cosmos-db-gremlin-api-resources"></a>在 Azure Cosmos DB Gremlin API 资源上预配数据库、容器或自动缩放吞吐量
[!INCLUDE[appliesto-gremlin-api](includes/appliesto-gremlin-api.md)]

本文介绍如何在 Azure Cosmos DB Gremlin API 中预配吞吐量。 你可在一个容器或数据库上预配标准（手动）或自动缩放吞吐量，并在数据库中的容器之间共享它。 你可使用 Azure 门户、Azure CLI 或 Azure Cosmos DB SDK 来预配吞吐量。

如果你使用的是其他 API，请参阅 [SQL API](how-to-provision-container-throughput.md)、[Cassandra API](how-to-provision-throughput-cassandra.md)、[适用于 MongoDB 的 API](how-to-provision-throughput-mongodb.md) 这几篇文章来预配吞吐量。

<a name="portal-gremlin"></a>
## <a name="azure-portal"></a>Azure 门户

1. 登录 [Azure 门户](https://portal.azure.cn/)。

1. [创建新的 Azure Cosmos 帐户](create-mongodb-dotnet.md#create-a-database-account)，或选择现有的 Azure Cosmos 帐户。

1. 打开“数据资源管理器”窗格，然后选择“新建图形” 。 接下来，请提供以下详细信息：

   * 表明要创建新数据库还是使用现有数据库。 如果要在数据库级别预配吞吐量，请选择“预配数据库吞吐量”选项。
   * 输入图形 ID。
   * 输入分区键值（例如 `/ItemID`）。
   * 输入要预配的吞吐量（例如，1000 RU）
   * 选择“确定”。

    :::image type="content" source="./media/how-to-provision-throughput-gremlin/provision-database-throughput-portal-gremlin-api.png" alt-text="使用数据库级吞吐量创建新图时的数据资源管理器的屏幕截图":::

## <a name="net-sdk"></a>.NET SDK

> [!Note]
> 使用适用于 SQL API 的 Cosmos SDK 为除 Cassandra 和 MongoDB API 之外的所有 Azure Cosmos DB API 预配吞吐量。

### <a name="provision-container-level-throughput"></a>预配容器级吞吐量

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
// Create a container with a partition key and provision throughput of 400 RU/s
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "myContainerName";
myCollection.PartitionKey.Paths.Add("/myPartitionKey");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("myDatabaseName"),
    myCollection,
    new RequestOptions { OfferThroughput = 400 });
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
// Create a container with a partition key and provision throughput of 1000 RU/s
string containerName = "myContainerName";
string partitionKeyPath = "/myPartitionKey";

await this.cosmosClient.GetDatabase("myDatabase").CreateContainerAsync(
    id: containerName,
    partitionKeyPath: partitionKeyPath,
    throughput: 1000);

```

---

### <a name="provision-database-level-throughput"></a>预配数据库级吞吐量

# <a name="net-sdk-v2"></a>[.NET SDK V2](#tab/dotnetv2)

```csharp
//set the throughput for the database
RequestOptions options = new RequestOptions
{
    OfferThroughput = 500
};

//create the database
await client.CreateDatabaseIfNotExistsAsync(
    new Database {Id = databaseName},  
    options);
```

# <a name="net-sdk-v3"></a>[.NET SDK V3](#tab/dotnetv3)

```csharp
//create the database with throughput
string databaseName = "MyDatabaseName";
await this.cosmosClient.CreateDatabaseIfNotExistsAsync(
        id: databaseName,
        throughput: 1000);

```

---

## <a name="azure-resource-manager"></a>Azure 资源管理器

Azure 资源管理器模板可用于在数据库或容器级资源上为所有 Azure Cosmos DB API 预配自动缩放吞吐量。 有关示例，请参阅 [Azure Cosmos DB 的 Azure 资源管理器模板](templates-samples-gremlin.md)。

## <a name="azure-cli"></a>Azure CLI

Azure CLI 可用于在数据库或容器级资源上为所有 Azure Cosmos DB API 预配自动缩放吞吐量。 有关示例，请参阅[用于 Azure Cosmos DB 的 Azure CLI 示例](cli-samples-gremlin.md)。

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell 可用于在数据库或容器级资源上为所有 Azure Cosmos DB API 预配自动缩放吞吐量。 有关示例，请参阅[适用于 Azure Cosmos DB 的 Azure PowerShell 示例](powershell-samples-gremlin.md)。

## <a name="next-steps"></a>后续步骤

请参阅以下文章，了解如何在 Azure Cosmos DB 中预配吞吐量：

* [Azure Cosmos DB 中的请求单位和吞吐量](request-units.md)

<!-- Update_Description: new article about how to provision throughput gremlin -->
<!--NEW.date: 11/09/2020-->