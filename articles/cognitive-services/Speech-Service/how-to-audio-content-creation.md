---
title: 音频内容创建 - 语音服务
titleSuffix: Azure Cognitive Services
description: 音频内容创建是一个在线工具，可用于为应用和产品自定义和微调 Microsoft 的文本到语音输出。
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
origin.date: 01/31/2020
ms.date: 08/03/2020
ms.author: v-tawe
ms.openlocfilehash: 64bd9ef430feabdd794690367a87a707c67e28fc
ms.sourcegitcommit: 3821704fee67315badba49cf628af2aa68d98f28
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/03/2020
ms.locfileid: "87516297"
---
# <a name="improve-synthesis-with-the-audio-content-creation-tool"></a>使用有声内容创作工具优化语音合成效果

[有声内容创作](https://speech.azure.cn/audiocontentcreation)是一个简单易用的语音合成在线编辑工具，可以自定义设计和优化文本到语音的输出结果，包括文字发音、数字读法、语句韵律等。你可以轻松地制作逼真自然的有声书，新闻广播、视频旁白和对话聊天机器人等。使用有声内容创作工具，你可以使用50+个国家和地区的150+个智能语音，包括超高质量的深度神经网络语音以及您的自定义语音。

有声内容创作工具基于[语音合成标记语言 (SSML)](speech-synthesis-markup.md)。为了方便使用，有声内容创作平台以可视化的方式，让每个人都可以参与到语音合成的创作中来。

请参阅[视频教程](https://b23.tv/F3uoRU)，了解如何使用有声内容创作。

## 如何开始？

使用有声内容创作工具需要注册Azure帐户并创建语音资源。工具会根据调用语音服务的数量计费。对于每个Azure帐户，每月会有免费语音配额，其中包含每月500000个字符的神经网络语音，每月5000000个字符的标准和自定义语音，以及每月1个自定义语音终结点托管服务。每月分配的金额通常足以满足大约3-5人的有声内创创作团队。

步骤 1-创建 Azure 帐户
若要使用音频内容创建，需要注册Azure 帐户。如果你没有 Azure 帐户，可以在[此处](https://www.microsoft.com/china/azure/index.html?fromtype=cn#azurefreeform)注册试用版。

步骤 2-创建语音订阅
注册Azure账户后，[创建语音资源](https://docs.azure.cn/zh-cn/cognitive-services/speech-service/overview#try-the-speech-service-for-free)。

步骤 3-开始有声内容创作
创建 Azure 帐户和语音资源后，访问[有声内容创作](https://speech.azure.cn/audiocontentcreation)工具开始你的音频创作。选择需要使用的语音订阅。你可以随时使用位于顶部导航栏中的“设置”选项来修改语音订阅。

## <a name="how-does-it-work"></a>使用手册

此图显示了优化文本到语音输出所需的步骤。 使用以下链接详细了解每个步骤。

![微调文本到语音输出所用步骤的图表。](media/audio-content-creation/audio-content-creation-diagram.jpg)

1. 使用纯文本或 SSML 脚本[创建音频优化文件](#create-an-audio-tuning-file)。
2. 选择脚本内容的语音和语言。 音频内容创建包括所有 [Microsoft 文本到语音转换](language-support.md#text-to-speech)。 可使用标准语音、神经语音或者你自己的自定义语音。
   >[!NOTE]
   > 管控访问可用于自定义神经语音，这允许创建类似于自然语音的高清语音。 有关详细信息，请参阅[管控过程](https://docs.azure.cn/cognitive-services/speech-service/text-to-speech)。

3. 查看默认的合成输出。 然后通过调整发音、停顿、音调、速率、语调、语音风格等来改进输出。 有关选项的完整列表，请参阅[语音合成标记语言](speech-synthesis-markup.md)。
4. 保存并[导出优化音频](#export-tuned-audio)。 在系统中保存优化音轨后，可继续工作并迭代输出。 如果对输出满意，可使用导出功能创建音频创建任务。 可查看导出任务的状态，并下载用于应用和产品的输出。

## <a name="create-an-audio-tuning-file"></a>新建音频创作文件

有两种方式可以将内容导入有声内容创作工具。

选项 1：

1. 单击“新建文件”以创建新的音频优化文件。
2. 在编辑窗口键入或粘贴内容。 每个文件的字符数最多为 20,000 个。 如果脚本的长度超过 20,000 个字符，则可以使用选项 2 将内容自动拆分为多个文件。 
3. 切勿忘记保存。

选项 2：

1. 单击“上传”导入一个或多个文本文件。 支持纯文本和 SSML。
2. 如果脚本文件超过 20,000 个字符，请按段落、字符或正则表达式拆分文件。 
3. 上传文本文件时，请确保该文件满足这些要求。

   | 属性 | 值/注释 |
   |----------|---------------|
   | 文件格式 | 纯文本 (.txt)<br/> SSML 文本 (.txt)<br/> 不支持 Zip 文件 |
   | 编码格式 | UTF-8 |
   | 文件名 | 每个文件必须拥有唯一的名称。 不支持重复项。 |
   | 文本长度 | 文本长度不得超过 20,000 个字符。 |
   | SSML 限制 | 每个 SSML 文件只能包含一条 SSML。 |

### <a name="plain-text-example"></a>纯文本示例

```txt
Welcome to use Audio Content Creation to customize audio output for your products.
```

### <a name="ssml-text-example"></a>SSML 文本示例

```xml
<speak xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="http://www.w3.org/2001/mstts" version="1.0" xml:lang="en-US">
    <voice name="Microsoft Server Speech Text to Speech Voice (en-US, AriaNeural)">
    Welcome to use Audio Content Creation <break time="10ms" />to customize audio output for your products.
    </voice>
</speak>
```

## <a name="export-tuned-audio"></a>导出优化音频

查看音频输出并且对调整和优化都满意后，就可以导出音频。

1. 单击“导出”以创建音频创建任务。 建议选择“导出到音频库”，因为它支持长音频输出和完整的音频输出体验。 还可直接将音频下载到本地磁盘，但只有前 10 分钟可用。 
2. 选择优化音频的输出格式。 下面提供了支持格式和采样率的列表。
3. 可在“导出任务”选项卡上查看任务的状态。如果任务失败，请参阅详细信息页获取完整的报表。
4. 完成该任务后，可以在“音频库”选项卡上下载音频。
5. 单击“下载”。 现在，你可以在你的应用或产品中使用自定义的优化音频。

### <a name="supported-audio-formats"></a>支持的音频格式

| 格式 | 16 kHz 采样率 | 24 kHz 采样率 |
|--------|--------------------|--------------------|
| wav | riff-16khz-16bit-mono-pcm | riff-24khz-16bit-mono-pcm |
| mp3 | audio-16khz-128kbitrate-mono-mp3 | audio-24khz-160kbitrate-mono-mp3 |

## <a name="see-also"></a>另请参阅

* [长音频 API](https://docs.azure.cn/cognitive-services/speech-service/long-audio-api)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Speech Studio](https://speech.azure.cn)
