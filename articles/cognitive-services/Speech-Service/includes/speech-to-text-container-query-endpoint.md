---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 1773e22a54cc86e7736c91e4be757caa0250cbf7
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81421970"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Konuşmadan metne veya Özel Konuşmadan metne

Kapsayıcı, [Konuşma SDK'sı](../index.yml)aracılığıyla erişilen websocket tabanlı sorgu uç noktası API'leri sağlar. Varsayılan olarak, Konuşma SDK çevrimiçi konuşma hizmetleri kullanır. Kapsayıcıyı kullanmak için başlatma yöntemini değiştirmeniz gerekir.

> [!TIP]
> Konuşma SDK'sını kapsayıcılarla kullanırken, Azure Konuşma kaynak [abonelik anahtarını veya kimlik doğrulama taşıyıcı belirteci](../rest-speech-to-text.md#authentication)sağlamanız gerekmez.

Aşağıdaki örneklere bakın.

# <a name="c"></a>[C #](#tab/csharp)

Bu Azure bulutu başlatma çağrısını kullanarak değiştirin:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

bu çağrıya konteyner [ana bilgisayarını](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet)kullanarak:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```
# <a name="python"></a>[Python](#tab/python)

Bu Azure bulutu başlatma çağrısını kullanarak değiştirin:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

bu çağrıya konteyner [ana bilgisayarını](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)kullanarak:

```python
speech_config = speechsdk.SpeechConfig(
    host="ws://localhost:5000")
```

***
