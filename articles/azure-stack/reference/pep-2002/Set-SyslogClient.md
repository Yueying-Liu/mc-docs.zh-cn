---
title: Azure Stack Hub 的 Set-SyslogClient 特权终结点
description: PowerShell Azure Stack 特权终结点 - Set-SyslogClient 的参考信息
author: WenJason
ms.topic: reference
origin.date: 04/27/2020
ms.date: 08/31/2020
ms.author: digimobile
ms.reviewer: fiseraci
ms.lastreviewed: 04/27/2020
ms.openlocfilehash: b237bec8fe146565ac3e4fd2865b15c6e0e0c372
ms.sourcegitcommit: 4e2d781466e54e228fd1dbb3c0b80a1564c2bf7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88867984"
---
# <a name="set-syslogclient"></a>Set-SyslogClient

## <a name="synopsis"></a>摘要
导入并应用 syslog 客户端终结点证书。

## <a name="syntax"></a>语法

```
Set-SyslogClient [-OutputSeverity <Object>] [-PfxBinary <Object>] [-RemoveCertificate] [-CertPassword <Object>]
 [-AsJob]
```

## <a name="parameters"></a>parameters

### <a name="-pfxbinary"></a>-PfxBinary
二进制格式的证书。
使用 Get-Content 从证书文件中提取字节数组。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-certpassword"></a>-CertPassword
安全字符串形式的证书密码。

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-removecertificate"></a>-RemoveCertificate
 

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-outputseverity"></a>-OutputSeverity
 

```yaml
Type: Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### <a name="-asjob"></a>-AsJob


```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

## <a name="next-steps"></a>后续步骤

有关如何访问和使用特权终结点的信息，请参阅[使用 Azure Stack Hub 中的特权终结点](../../operator/azure-stack-privileged-endpoint.md)。
