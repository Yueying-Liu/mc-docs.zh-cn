---
title: Azure Cosmos DB 查询语言中的 FLOOR
description: 了解 Azure Cosmos DB 中的 FLOOR SQL 系统函数，该函数返回小于或等于指定数值表达式的最大整数
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 09/13/2019
author: rockboyfor
ms.date: 11/16/2020
ms.author: v-yeche
ms.custom: query-reference
ms.openlocfilehash: a63036d3ca6ba9cbc5b7500800b9602f8d1fc990
ms.sourcegitcommit: 5f07189f06a559d5617771e586d129c10276539e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94551823"
---
# <a name="floor-azure-cosmos-db"></a>FLOOR (Azure Cosmos DB)
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

 返回小于或等于指定数值表达式的最大整数。  

## <a name="syntax"></a>语法

```sql
FLOOR (<numeric_expr>)  
```  

## <a name="arguments"></a>参数

*numeric_expr*  
  是一个数值表达式。  

## <a name="return-types"></a>返回类型

  返回一个数值表达式。  

## <a name="examples"></a>示例

  以下示例显示了如何对正值、负值和零值使用 `FLOOR` 函数。  

```sql
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  

 下面是结果集。  

```json
[{fl1: 123, fl2: -124, fl3: 0}]  
```

## <a name="remarks"></a>备注

此系统函数将从[范围索引](index-policy.md#includeexclude-strategy)中获益。

## <a name="next-steps"></a>后续步骤

- [数学函数 Azure Cosmos DB](sql-query-mathematical-functions.md)
- [系统函数 Azure Cosmos DB](sql-query-system-functions.md)
- [Azure Cosmos DB 简介](introduction.md)

<!-- Update_Description: update meta properties, wording update, update link -->