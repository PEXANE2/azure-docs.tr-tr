---
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 08/18/2020
ms.author: aahi
ms.custom: devx-track-csharp
ms.openlocfilehash: dd1552dda28291112a2412bdf956bc49a0b541d7
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95996457"
---
Kapsayıcı, [konuşma SDK 'sı](../index.yml)aracılığıyla erişilen WebSocket tabanlı sorgu uç noktası API 'leri sağlar. Varsayılan olarak, konuşma SDK 'Sı çevrimiçi konuşma hizmetlerini kullanır. Kapsayıcıyı kullanmak için başlatma yöntemini değiştirmeniz gerekir.

> [!TIP]
> Kapsayıcı ile konuşma SDK 'sını kullanırken, Azure konuşma kaynak [aboneliği anahtarını veya bir kimlik doğrulama taşıyıcı belirtecini](../rest-speech-to-text.md#authentication)sağlamanız gerekmez.

Aşağıdaki örneklere bakın.

# <a name="c"></a>[C#](#tab/csharp)

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

Bu çağrıyı kapsayıcı [konakla](/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?preserve-view=true&view=azure-dotnet)birlikte kullanmak için:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```

# <a name="python"></a>[Python](#tab/python)

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

Bu çağrıyı kapsayıcı [uç noktasıyla](/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?preserve-view=true&view=azure-python)birlikte kullanmak için:

```python
speech_config = speechsdk.SpeechConfig(
    endpoint="ws://localhost:5000/speech/recognition/conversation/cognitiveservices/v1"
```

---