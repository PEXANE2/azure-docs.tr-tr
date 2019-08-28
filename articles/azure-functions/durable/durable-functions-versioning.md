---
title: Dayanıklı İşlevler sürüm oluşturma-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda sürüm oluşturmayı nasıl uygulayacağınızı öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: ef64a43cbed7f033a938351506b7f78142ff044c
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70097632"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler sürüm oluşturma (Azure Işlevleri)

İşlevlerin bir uygulamanın kullanım ömrü boyunca ekleneceğini, kaldırılacağını ve değiştirilmesini sağlayacak bir işlem olabilir. [Dayanıklı işlevler](durable-functions-overview.md) , işlevleri daha önce mümkün olmayan yollarla birlikte zincirlemeye olanak tanır ve bu zincir, sürüm oluşturmayı nasıl işleyebileceğini etkiler.

## <a name="how-to-handle-breaking-changes"></a>Son değişiklikleri işleme

Dikkat edilecek değişikliklere yönelik birkaç örnek vardır. Bu makalede, en yaygın olarak karşılaşılan anlatılmaktadır. Tüm bunların arkasındaki ana tema, yeni ve var olan işlev düzenleyiclerinin işlev kodundaki değişikliklere göre etkilenmesidir.

### <a name="changing-activity-function-signatures"></a>Etkinlik işlev imzalarını değiştirme

İmza değişikliği, bir işlevin adında, girişte veya çıkışında bir değişikliğe başvurur. Bir etkinlik işlevinde bu tür bir değişiklik yapılırsa, ona bağlı olan Orchestrator işlevini bozabilir. Orchestrator işlevini bu değişikliğe uyum sağlayacak şekilde güncelleştirirseniz, var olan uçuş örneklerini kesebilirsiniz.

Örnek olarak, aşağıdaki fonksiyonumuz olduğunu varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu uyarlaması işlevi, **foo** 'ın sonuçlarını alır ve **çubuğa**geçirir. Daha geniş bir sonuç değerini desteklemek için **foo** `bool` öğesinin dönüş değerini olarak `int` değiştirmemiz gerektiğini varsayalım. Sonuç şöyle görünür:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu değişiklik Orchestrator işlevinin tüm yeni örnekleri için ince çalışır, ancak uçuş dışı örnekleri keser. Örneğin, bir düzenleme örneğinin **foo**çağrısı yaptığı durumu göz önünde bulundurun, bir Boole değeri geri alır ve kontrol noktaları. Bu noktada imza değişikliği dağıtılırsa Checkpoint örneği, çağrısı `context.CallActivityAsync<int>("Foo")`devam ettiğinde ve yeniden yürütüldüğünde hemen başarısız olur. Bunun nedeni, geçmiş tablosundaki `bool` sonucun olduğu ancak yeni kodun ' de seri durumdan `int`çıkarmaya çalışacağı bir sonucudur.

Bu, bir imza değişikliğinin varolan örnekleri bozulabileceği birçok farklı yönden yalnızca biridir. Genel olarak, bir Orchestrator 'ın bir işlevi çağırdığı şeklini değiştirmesi gerekiyorsa, değişikliğin sorunlu olması olasıdır.

### <a name="changing-orchestrator-logic"></a>Orchestrator mantığını değiştirme

Sürüm oluşturma sorunlarının diğer sınıfı, Orchestrator işlev kodunun, uçuş örnekleri için yeniden yürütme mantığını kullanmasını sağlayan bir şekilde değiştirilmesinin ardından gelir.

Aşağıdaki Orchestrator işlevini göz önünde bulundurun:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Şimdi başka bir işlev çağrısı eklemek için bir masum değişikliği yapmak istediğinizi varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    if (result)
    {
        await context.CallActivityAsync("SendNotification");
    }

    await context.CallActivityAsync("Bar", result);
}
```

Bu değişiklik, **foo** ve **Bar**arasında **SendNotification** öğesine yeni bir işlev çağrısı ekler. İmza değişikliği yok. Bu sorun, var olan bir örnek, **çubuğa**yapılan çağrıdan devam ettiğinde ortaya çıkar. Yeniden yürütme sırasında, **foo** öğesine yapılan özgün çağrı döndürülürse `true`Orchestrator Replay, yürütme geçmişinde olmayan **SendNotification** ' a çağrı yapılır. Sonuç olarak, dayanıklı görev çerçevesi, bir `NonDeterministicOrchestrationException` **çubuğa**çağrı görmediğinde **SendNotification** çağrısıyla karşılaştığından bir ile başarısız olur.

## <a name="mitigation-strategies"></a>Risk azaltma stratejileri

Sürüm oluşturma güçlükleri ile ilgili bazı stratejiler aşağıda verilmiştir:

* Hiçbir şey yapma
* Tüm uçuş örneklerini durdur
* Yan yana dağıtımlar

### <a name="do-nothing"></a>Hiçbir şey yapma

Önemli bir değişikliği işlemenin en kolay yolu, uçuş düzenleme örneklerinin başarısız olmasına imkan tanır. Yeni örnekler değiştirilen kodu başarıyla Çalıştır.

Bunun bir sorun olup olmadığı, uçuş örneklerinizin önem derecesine bağlıdır. Etkin geliştirme yapıyorsanız ve uçuş yerinde örneklerle ilgilenmezseniz, bu yeterince iyi olabilir. Ancak, tanılama işlem hattınızdaki özel durumlar ve hatalarla uğraşmanız gerekir. Bu şeyleri önlemek istiyorsanız, diğer sürüm oluşturma seçeneklerini göz önünde bulundurun.

### <a name="stop-all-in-flight-instances"></a>Tüm uçuş örneklerini durdur

Diğer bir seçenek de tüm uçuş örneklerini durdurmaktır. Bu, iç **Denetim kuyruğu** ve iş **öğesi kuyruğu** sıralarının içerikleri temizlenerek yapılabilir. Örnekler her yerde kalır, ancak hata iletileriyle Telemetriyi yığılmazlar. Bu, hızlı prototip geliştirmede idealdir.

> [!WARNING]
> Bu kuyrukların ayrıntıları zaman içinde değişebilir, bu nedenle üretim iş yükleri için bu tekniğin dayanmayın.

### <a name="side-by-side-deployments"></a>Yan yana dağıtımlar

Üst düzey değişikliklerin güvenli bir şekilde dağıtılmasını sağlamanın en son hata kanıtlama yöntemi, eski sürümleriniz ile yan yana dağıtılır. Bu, aşağıdaki tekniklerden herhangi biri kullanılarak yapılabilir:

* Tüm güncelleştirmeleri tamamen yeni işlevler (yeni adlar) olarak dağıtın.
* Tüm güncelleştirmeleri, farklı bir depolama hesabıyla yeni bir işlev uygulaması olarak dağıtın.
* İşlev uygulamasının yeni bir kopyasını, ancak güncelleştirilmiş `TaskHub` bir adı ile dağıtın. Bu önerilen tekniktir.

### <a name="how-to-change-task-hub-name"></a>Görev hub 'ı adını değiştirme

Görev hub 'ı *Host. JSON* dosyasında şu şekilde yapılandırılabilir:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "HubName": "MyTaskHubV2"
    }
}
```

#### <a name="functions-2x"></a>İşlevler 2.x

Varsayılan değer `DurableFunctionsHub` şeklindedir.

Tüm Azure depolama varlıkları `HubName` yapılandırma değerine göre adlandırılır. Görev merkezine yeni bir ad vererek uygulamanızın yeni sürümü için ayrı sıraların ve geçmiş tablosunun oluşturulmasını sağlarsınız.

İşlev uygulamasının yeni sürümünü yeni bir [dağıtım yuvasına](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/)dağıtmanızı öneririz. Dağıtım yuvaları, işlev uygulamanızın etkin *Üretim* yuvası olarak yalnızca biri ile yan yana birden çok kopyasını çalıştırmanızı sağlar. Yeni düzenleme mantığını var olan altyapınızla kullanıma sunmaya hazırsanız, yeni sürümü üretim yuvasına değiştirme kadar basit olabilir.

> [!NOTE]
> Bu strateji, Orchestrator işlevleri için HTTP ve Web kancası Tetikleyicileri kullandığınızda en iyi şekilde çalışır. Kuyruklar veya Event Hubs gibi HTTP olmayan Tetikleyiciler için tetikleyici tanımı, değiştirme işleminin bir parçası olarak güncelleştirilmiş [bir uygulama ayarından türetilmelidir](../functions-bindings-expressions-patterns.md#binding-expressions---app-settings) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans ve ölçek sorunlarını nasıl ele alabileceğinizi öğrenin](durable-functions-perf-and-scale.md)
