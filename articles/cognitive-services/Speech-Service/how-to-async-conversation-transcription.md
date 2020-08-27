---
title: Zaman uyumsuz konuşma dökümü-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmetini kullanarak zaman uyumsuz konuşma dökümünü nasıl kullanacağınızı öğrenin. Yalnızca Java ve C# için kullanılabilir.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.custom: devx-track-csharp
zone_pivot_groups: programming-languages-set-twenty-one
ms.openlocfilehash: 4b2dfa8d474f10d6b4ca1c46ac2b575e8d8407ff
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88934659"
---
# <a name="asynchronous-conversation-transcription"></a>Zaman uyumsuz Konuşma Transkripsiyonu

Bu makalede, **Remotekonuşmayı Tiontranscriptionclient** API 'si kullanılarak zaman uyumsuz görüşme dökümü gösterilmektedir. Görüşme dökümünü zaman uyumsuz olarak yapılacak şekilde yapılandırdıysanız ve ' a sahipseniz `conversationId` , `conversationId` **Remotekonuşmayı Tiontranscriptionclient** API 'sini kullanarak bununla ilişkili dökümü elde edebilirsiniz.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Zaman uyumsuz ile gerçek zamanlı + zaman uyumsuz

Zaman uyumsuz döküm sayesinde, konuşma sesini akışa alırsınız ancak gerçek zamanlı olarak geri döndürülen bir döküm gerekmez. Bunun yerine, ses gönderildikten sonra, `conversationId` `Conversation` zaman uyumsuz döküm durumunu sorgulamak için öğesini kullanın. Zaman uyumsuz döküm hazırlandıktan sonra bir ile karşılaşırsınız `RemoteConversationTranscriptionResult` .

Gerçek zamanlı ve zaman uyumsuz olarak, dökümü gerçek zamanlı olarak alır, ancak `conversationId` (zaman uyumsuz senaryoya benzer şekilde) ile sorgulama yaparak da bu işlemi gerçekleştirebilirsiniz.

Zaman uyumsuz dökümü gerçekleştirmek için iki adım gerekir. İlk adım, yalnızca zaman uyumsuz veya gerçek zamanlı ve zaman uyumsuz olarak seçerek sesi karşıya yüklemedir. İkinci adım, döküm sonuçlarını almak için kullanılır.

::: zone pivot="programming-language-csharp"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/csharp/examples.md)]
::: zone-end

::: zone pivot="programming-language-java"
[!INCLUDE [prerequisites](includes/how-to/remote-conversation/java/examples.md)]
::: zone-end


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da örneklerimizi keşfet](https://aka.ms/csspeech/samples)
