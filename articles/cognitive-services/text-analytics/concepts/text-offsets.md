---
title: 文本分析 API 中的文本偏移量
titleSuffix: Azure Cognitive Services
description: 了解多语言和表情符号编码导致的偏移量。
services: cognitive-services
author: Johnnytechn
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 10/26/2020
origin.date: 03/09/2020
ms.author: v-johya
ms.reviewer: jdesousa
ms.openlocfilehash: 28198f42c9e638cd5d617bd16c761df0a4e58baa
ms.sourcegitcommit: 93309cd649b17b3312b3b52cd9ad1de6f3542beb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93105625"
---
# <a name="text-offsets-in-the-text-analytics-api-output"></a>文本分析 API 输出中的文本偏移量

多语言和表情符号支持已导致 Unicode 编码，该编码使用多个码位来表示单个显示的字符（称为字形）。 例如，🌷 和 👍 之类的表情符号可以使用几个字符将形状与其他字符组合起来，以提供视觉对象特性（例如肤色）。 同样，印地语文字 `अनुच्छेद` 将编码为五个字母和三个组合标记。

由于可能的多语言和表情符号编码的长度不同，文本分析 API 可能会在响应中返回偏移量。

## <a name="offsets-in-the-api-response"></a>API 响应中的偏移量。 

每当在 API 响应（如[命名实体识别](../how-tos/text-analytics-how-to-entity-linking.md)或[情绪分析](../how-tos/text-analytics-how-to-sentiment-analysis.md)）中返回偏移量时，请记住以下事项：

* 响应中的元素可能会特定于所调用的终结点。 
* HTTP POST/GET 有效负载以 [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp) 格式进行编码，该编码不一定是客户端编译器或操作系统上的默认字符编码。
* 偏移量是指基于 [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) 标准的字形计数，而不是字符计数。

## <a name="extracting-substrings-from-text-with-offsets"></a>从具有偏移量的文本中提取子字符串

使用基于字符的子字符串方法（例如 .NET [substring()](https://docs.microsoft.com/dotnet/api/system.string.substring?view=netframework-4.8) 方法）时，偏移量可能会导致问题。 一个问题是，偏移量可能导致子字符串方法在多字符字形编码的中间而不是结尾处结束。

在 .NET 中，考虑使用 [StringInfo](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo?view=netframework-4.8) 类，该类使你可以将字符串作为一系列文本元素（而不是单个字符对象）来处理。 也可以在首选软件环境中查找字形拆分器库。 

为方便起见，文本分析 API 也会返回这些文本元素。

## <a name="offsets-in-api-version-31-preview"></a>API 版本 3.1-preview 中的偏移

从 API 版本 3.1-preview.1 开始，返回偏移量的所有文本分析 API 终结点都会支持 `stringIndexType` 参数。 此参数在 API 输出中调整 `offset` 和 `length` 属性，以匹配请求的字符串迭代方案。 目前，我们支持三种类型：

1. `textElement_v8`（默认值）：根据 [Unicode 8.0.0](https://unicode.org/versions/Unicode8.0.0) 标准的定义循环访问字形
2. `unicodeCodePoint`：循环访问 [Unicode 码位](http://www.unicode.org/versions/Unicode13.0.0/ch02.pdf#G25564)（Python 3 的默认方案）
3. `utf16CodeUnit`：循环访问 [UTF-16 代码单位](https://unicode.org/faq/utf_bom.html#UTF16)（Javascript、Java 和 .NET 的默认方案）

如果请求的 `stringIndexType` 与所选的编程环境相匹配，则可以使用标准子字符串或切片方法提取子字符串。 

## <a name="see-also"></a>另请参阅

* [文本分析概述](../overview.md)
* [情绪分析](../how-tos/text-analytics-how-to-sentiment-analysis.md)
* [实体识别](../how-tos/text-analytics-how-to-entity-linking.md)
* [检测语言](../how-tos/text-analytics-how-to-keyword-extraction.md)
* [语言识别](../how-tos/text-analytics-how-to-language-detection.md)

