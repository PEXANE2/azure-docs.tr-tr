---
title: Azure Dayanıklı Fonksiyonlar birimi testi
description: Dayanıklı İşlevler test ini nasıl birleştireceğiz öğrenin.
ms.topic: conceptual
ms.date: 11/03/2019
ms.openlocfilehash: 86733f8b5b80799bad3e52c643ed27465dfc7641
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74231218"
---
# <a name="durable-functions-unit-testing"></a>Dayanıklı Fonksiyonlar birim testi

Birim testi, modern yazılım geliştirme uygulamalarının önemli bir parçasıdır. Birim testleri iş mantığı davranışını doğrular ve gelecekte fark edilmeyen kırılma değişiklikleri nin ortaya konmasından korur. Dayanıklı Fonksiyonlar karmaşıklığı kolayca büyüyebilir, bu nedenle birim testleri nin getirilmesi değişikliklerin kırılmasını önlemeye yardımcı olacaktır. Aşağıdaki bölümlerde üç işlev türü - Orkestrasyon istemci, orkestratör ve etkinlik işlevleri birleştirilmesi nasıl açıklanmıştır.

> [!NOTE]
> Bu makalede, Dayanıklı Fonksiyonlar 1.x hedefleyen Dayanıklı Fonksiyonlar uygulamaları için birim testi için kılavuz sağlar. Dayanıklı Fonksiyonlar 2.x'te tanıtılan değişiklikleri hesaba katmak için henüz güncelleştirilmiştir. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki örnekler aşağıdaki kavram ve çerçeveler hakkında bilgi gerektirir:

* Birim testi

* Dayanıklı İşlevler

* [xUnit](https://xunit.github.io/) - Test çerçevesi

* [moq](https://github.com/moq/moq4) - Alay çerçevesi

## <a name="base-classes-for-mocking"></a>Alay etmek için temel sınıflar

Alay, Dayanıklı Fonksiyonlar 1.x'te üç soyut sınıf aracılığıyla desteklenir:

* `DurableOrchestrationClientBase`

* `DurableOrchestrationContextBase`

* `DurableActivityContextBase`

Bu `DurableOrchestrationClient`sınıflar, Orchestration `DurableOrchestrationContext`Client, Orchestrator ve Etkinlik yöntemlerini tanımlayan temel `DurableActivityContext` sınıflardır. Alaylar, birim testinin iş mantığını doğrulayabilmesi için taban sınıf yöntemleri için beklenen davranışı ayarlar. Orkestrasyon İstemci ve Orkestratör'de iş mantığını test eden birim için iki aşamalı bir iş akışı vardır:

1. Orkestrasyon istemcisi ve orkestratör işlev imzalarını tanımlarken somut uygulama yerine temel sınıfları kullanın.
2. Birim testleri temel sınıfların davranışı alay ve iş mantığı doğrulamak.

Orkestrasyon istemcisi bağlama ve orkestratör tetikleme bağlama kullanan işlevleri test etmek için aşağıdaki paragraflarda daha fazla ayrıntı bulabilirsiniz.

## <a name="unit-testing-trigger-functions"></a>Birim test tetik fonksiyonları

Bu bölümde, birim testi yeni orkestrasyon başlatmak için aşağıdaki HTTP tetikleme işlevinin mantığını doğrular.

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

Birim test görevi, yanıt yükünde `Retry-After` sağlanan üstbilginin değerini doğrulamak olacaktır. Yani birim testi öngörülebilir `DurableOrchestrationClientBase` davranışı sağlamak için bazı yöntemlerle alay edecektir.

İlk olarak, taban sınıf bir `DurableOrchestrationClientBase`alay gereklidir. Mock uygulayan yeni bir sınıf `DurableOrchestrationClientBase`olabilir. Ancak, [moq](https://github.com/moq/moq4) gibi alaycı bir çerçeve kullanarak süreci kolaylaştırır:

```csharp
    // Mock DurableOrchestrationClientBase
    var durableOrchestrationClientBaseMock = new Mock<DurableOrchestrationClientBase>();
```

Daha `StartNewAsync` sonra yöntem iyi bilinen bir örnek kimliği döndürmek için alay edilir.

```csharp
    // Mock StartNewAsync method
    durableOrchestrationClientBaseMock.
        Setup(x => x.StartNewAsync(functionName, It.IsAny<object>())).
        ReturnsAsync(instanceId);
```

Sonraki `CreateCheckStatusResponse` her zaman boş bir HTTP 200 yanıtı dönmek için alay edilir.

```csharp
    // Mock CreateCheckStatusResponse method
    durableOrchestrationClientBaseMock
        .Setup(x => x.CreateCheckStatusResponse(It.IsAny<HttpRequestMessage>(), instanceId))
        .Returns(new HttpResponseMessage
        {
            StatusCode = HttpStatusCode.OK,
            Content = new StringContent(string.Empty),
            Headers =
            {
                RetryAfter = new RetryConditionHeaderValue(TimeSpan.FromSeconds(10))
            }
        });
```

`ILogger`da alay edilir:

```csharp
    // Mock ILogger
    var loggerMock = new Mock<ILogger>();
```  

Şimdi `Run` yöntem birim testinden çağrılır:

```csharp
    // Call Orchestration trigger function
    var result = await HttpStart.Run(
        new HttpRequestMessage()
        {
            Content = new StringContent("{}", Encoding.UTF8, "application/json"),
            RequestUri = new Uri("http://localhost:7071/orchestrators/E1_HelloSequence"),
        },
        durableOrchestrationClientBaseMock.Object,
        functionName,
        loggerMock.Object);
 ```

 Son adım, çıktıyı beklenen değerle karşılaştırmaktır:

```csharp
    // Validate that output is not null
    Assert.NotNull(result.Headers.RetryAfter);

    // Validate output's Retry-After header value
    Assert.Equal(TimeSpan.FromSeconds(10), result.Headers.RetryAfter.Delta);
```

Tüm adımları birleştirdikten sonra, birim testi aşağıdaki koda sahip olacaktır:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HttpStartTests.cs)]

## <a name="unit-testing-orchestrator-functions"></a>Birim test orkestratör fonksiyonları

Orkestratör fonksiyonları genellikle çok daha fazla iş mantığına sahip oldukları için birim testi için daha da ilginçtir.

Bu bölümde birim testleri `E1_HelloSequence` Orchestrator işlevinin çıktısını doğrulayacaktır:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Birim test kodu bir sahte oluşturma ile başlar:

```csharp
    var durableOrchestrationContextMock = new Mock<DurableOrchestrationContextBase>();
```

Daha sonra etkinlik yöntemi aramaları alay edilecektir:

```csharp
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Tokyo")).ReturnsAsync("Hello Tokyo!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "Seattle")).ReturnsAsync("Hello Seattle!");
    durableOrchestrationContextMock.Setup(x => x.CallActivityAsync<string>("E1_SayHello", "London")).ReturnsAsync("Hello London!");
```

Sonraki birim testi `HelloSequence.Run` yöntemi çağırır:

```csharp
    var result = await HelloSequence.Run(durableOrchestrationContextMock.Object);
```

Ve son olarak çıktı doğrulanacaktır:

```csharp
    Assert.Equal(3, result.Count);
    Assert.Equal("Hello Tokyo!", result[0]);
    Assert.Equal("Hello Seattle!", result[1]);
    Assert.Equal("Hello London!", result[2]);
```

Tüm adımları birleştirdikten sonra, birim testi aşağıdaki koda sahip olacaktır:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceOrchestratorTests.cs)]

## <a name="unit-testing-activity-functions"></a>Birim test etkinlik fonksiyonları

Etkinlik işlevleri, dayanıklı olmayan işlevler ile aynı şekilde birim test edilebilir.

Bu bölümde birim testi Etkinlik işlevinin `E1_SayHello` davranışını doğrular:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HelloSequence.cs)]

Ve birim testleri çıktının biçimini doğrular. Birim testleri parametre türlerini doğrudan `DurableActivityContextBase` veya sahte sınıf kullanabilir:

[!code-csharp[Main](~/samples-durable-functions/samples/VSSample.Tests/HelloSequenceActivityTests.cs)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [xUnit hakkında daha fazla bilgi edinin](https://xunit.github.io/docs/getting-started-dotnet-core)
> 
> [moq hakkında daha fazla bilgi edinin](https://github.com/Moq/moq4/wiki/Quickstart)
