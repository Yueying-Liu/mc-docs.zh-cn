---
title: Azure Cosmos DB 查询语言中的 ARRAY_CONCAT
description: 了解 Azure Cosmos DB 中的 Array Concat SQL 系统函数如何返回一个数组来表示连接两个或更多数组值的结果
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/03/2020
author: rockboyfor
ms.date: 11/16/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: 8750533595c23eda246f55db0d1003ec5d321457
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94551795"
---
# <a name="array_concat-azure-cosmos-db"></a>ARRAY_CONCAT (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 返回一个数组，该数组是连接两个或更多数组值的结果。  

## <a name="syntax"></a>语法

```sql
ARRAY_CONCAT (<arr_expr1>, <arr_expr2> [, <arr_exprN>])  
```  

## <a name="arguments"></a>参数

*arr_expr*  
  是要连接到其他值的数组表达式。 `ARRAY_CONCAT` 函数需要至少两个 *arr_expr* 参数。  

## <a name="return-types"></a>返回类型

  返回一个数组表达式。  

## <a name="examples"></a>示例

  以下示例演示了如何连接两个数组。  

```sql
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  

 下面是结果集。  

```json
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  

## <a name="remarks"></a>备注

此系统函数不会使用索引。

## <a name="next-steps"></a>后续步骤

- [数组函数 Azure Cosmos DB](sql-query-array-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->
