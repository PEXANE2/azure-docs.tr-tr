---
title: Konuşmadan metne kapsayıcı uç noktasını sorgula
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: a55cf5ea6aa696d0cf0cdd619dc22839d748d83c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491147"
---
### <a name="speech-to-text-or-custom-speech-to-text"></a>Konuşmadan metne veya Özel Konuşma Tanıma metne dönüştürme

Kapsayıcı, [konuşma SDK 'sı](../index.md)aracılığıyla erişilen WebSocket tabanlı sorgu uç noktası API 'leri sağlar. Varsayılan olarak, konuşma SDK 'Sı çevrimiçi konuşma hizmetlerini kullanır. Kapsayıcıyı kullanmak için başlatma yöntemini değiştirmeniz gerekir.

> [!TIP]
> Kapsayıcı ile konuşma SDK 'sını kullanırken, Azure konuşma kaynak [aboneliği anahtarını veya bir kimlik doğrulama taşıyıcı belirtecini](../rest-speech-to-text.md#authentication)sağlamanız gerekmez.

Aşağıdaki örneklere bakın.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```csharp
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

kapsayıcı [uç noktasını](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromendpoint?view=azure-dotnet)kullanarak bu çağrıya:

```csharp
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"));
```
# <a name="pythontabpython"></a>[Python](#tab/python)

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

kapsayıcı [uç noktasını](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)kullanarak bu çağrıya:

```python
speech_config = speechsdk.SpeechConfig(
    endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

***
