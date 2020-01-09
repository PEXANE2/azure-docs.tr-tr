---
title: Konuşmadan metne kapsayıcı uç noktasını sorgula
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/15/2019
ms.author: dapine
ms.openlocfilehash: 2d96385f2d2d34d161739c55228220cf28871048
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "74132608"
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

kapsayıcı [konağını](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet)kullanarak bu çağrıya:

```csharp
var config = SpeechConfig.FromHost(
    new Uri("ws://localhost:5000"));
```
# <a name="pythontabpython"></a>[Python](#tab/python)

Bu Azure-Cloud başlatma çağrısını kullanarak değiştirin:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

kapsayıcı [konağını](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python)kullanarak bu çağrıya:

```python
speech_config = speechsdk.SpeechConfig(
    host="ws://localhost:5000")
```

***
