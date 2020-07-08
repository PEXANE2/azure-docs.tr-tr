---
title: Dayanıklı İşlevler sürüm oluşturma-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda sürüm oluşturmayı nasıl uygulayacağınızı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 87cbb94dbab241630dc7585bdf4314d858d5b4da
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74232750"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler sürüm oluşturma (Azure Işlevleri)

İşlevlerin bir uygulamanın kullanım ömrü boyunca ekleneceğini, kaldırılacağını ve değiştirilmesini sağlayacak bir işlem olabilir. [Dayanıklı işlevler](durable-functions-overview.md) , işlevleri daha önce mümkün olmayan yollarla birlikte zincirlemeye olanak tanır ve bu zincir, sürüm oluşturmayı nasıl işleyebileceğini etkiler.

## <a name="how-to-handle-breaking-changes"></a>Son değişiklikleri işleme

Dikkat edilecek değişikliklere yönelik birkaç örnek vardır. Bu makalede, en yaygın olarak karşılaşılan anlatılmaktadır. Tüm bunların arkasındaki ana tema, yeni ve var olan işlev düzenleyiclerinin işlev kodundaki değişikliklere göre etkilenmesidir.

### <a name="changing-activity-or-entity-function-signatures"></a>Etkinlik veya varlık işlev imzalarını değiştirme

İmza değişikliği, bir işlevin adında, girişte veya çıkışında bir değişikliğe başvurur. Bir etkinlik veya varlık işlevinde bu tür bir değişiklik yapılırsa, ona bağlı herhangi bir Orchestrator işlevini bozabilir. Orchestrator işlevini bu değişikliğe uyum sağlayacak şekilde güncelleştirirseniz, var olan uçuş örneklerini kesebilirsiniz.

Örnek olarak, aşağıdaki Orchestrator işlevine sahip olduğunuzu varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu uyarlaması işlevi, **foo** 'ın sonuçlarını alır ve **çubuğa**geçirir. **Foo** `bool` `int` Daha geniş bir sonuç değerini desteklemek için foo öğesinin dönüş değerini olarak değiştirmemiz gerektiğini varsayalım. Sonuç şöyle görünür:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı İşlevler 2. x ' tir. Dayanıklı İşlevler 1. x için yerine kullanmanız gerekir `DurableOrchestrationContext` `IDurableOrchestrationContext` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

Bu değişiklik Orchestrator işlevinin tüm yeni örnekleri için ince çalışır, ancak uçuş dışı örnekleri keser. Örneğin, bir düzenleme örneğinin adlı bir işlevi çağırdığı `Foo` , bir Boole değeri geri aldığı ve sonra kontrol noktalarının durumunu göz önünde bulundurun. Bu noktada imza değişikliği dağıtılırsa Checkpoint örneği, çağrısı devam ettiğinde ve yeniden yürütüldüğünde hemen başarısız olur `context.CallActivityAsync<int>("Foo")` . Bu hata, geçmiş tablosundaki sonuç, `bool` ancak yeni kod onu içine serisini kaldırma girişiminde bulunduğu için oluşur `int` .

Bu örnek, bir imza değişikliğinin varolan örnekleri bozulabileceği birçok farklı yönden yalnızca biridir. Genel olarak, bir Orchestrator 'ın bir işlevi çağırdığı şeklini değiştirmesi gerekiyorsa, değişikliğin sorunlu olması olasıdır.

### <a name="changing-orchestrator-logic"></a>Orchestrator mantığını değiştirme

Sürüm oluşturma sorunlarının diğer sınıfı, Orchestrator işlev kodunun, uçuş örnekleri için yeniden yürütme mantığını kullanmasını sağlayan bir şekilde değiştirilmesinin ardından gelir.

Aşağıdaki Orchestrator işlevini göz önünde bulundurun:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Şimdi başka bir işlev çağrısı eklemek için bir masum değişikliği yapmak istediğinizi varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    if (result)
    {
        await context.CallActivityAsync("SendNotification");
    }

    await context.CallActivityAsync("Bar", result);
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı İşlevler 2. x ' tir. Dayanıklı İşlevler 1. x için yerine kullanmanız gerekir `DurableOrchestrationContext` `IDurableOrchestrationContext` . Sürümler arasındaki farklılıklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

Bu değişiklik, **foo** ve **Bar**arasında **SendNotification** öğesine yeni bir işlev çağrısı ekler. İmza değişikliği yok. Bu sorun, var olan bir örnek, **çubuğa**yapılan çağrıdan devam ettiğinde ortaya çıkar. Yeniden yürütme sırasında, **foo** öğesine yapılan özgün çağrı döndürülürse `true` , Orchestrator Replay, yürütme geçmişinde olmayan **SendNotification**öğesine çağrı yapılır. Sonuç olarak, dayanıklı görev çerçevesi, bir `NonDeterministicOrchestrationException` **çubuğa**çağrı görmediğinde **SendNotification** çağrısıyla karşılaştığından bir ile başarısız olur. , Vb. dahil olmak üzere "dayanıklı" API 'Lerine çağrı eklenirken aynı tür bir sorun oluşabilir `CreateTimer` `WaitForExternalEvent` .

## <a name="mitigation-strategies"></a>Risk azaltma stratejileri

Sürüm oluşturma güçlükleri ile ilgili bazı stratejiler aşağıda verilmiştir:

* Hiçbir şey yapma
* Tüm uçuş örneklerini durdur
* Yan yana dağıtımlar

### <a name="do-nothing"></a>Hiçbir şey yapma

Önemli bir değişikliği işlemenin en kolay yolu, uçuş düzenleme örneklerinin başarısız olmasına imkan tanır. Yeni örnekler değiştirilen kodu başarıyla Çalıştır.

Bu tür bir başarısızlığın bir sorun olup olmadığı, uçuş örneklerinizin önem derecesine bağlıdır. Etkin geliştirme yapıyorsanız ve uçuş yerinde örneklerle ilgilenmezseniz, bu yeterince iyi olabilir. Ancak, tanılama işlem hattınızdaki özel durumlar ve hatalarla uğraşmanız gerekir. Bu şeyleri önlemek istiyorsanız, diğer sürüm oluşturma seçeneklerini göz önünde bulundurun.

### <a name="stop-all-in-flight-instances"></a>Tüm uçuş örneklerini durdur

Diğer bir seçenek de tüm uçuş örneklerini durdurmaktır. İç **Denetim kuyruğu** ve iş **öğesi kuyruğu** sıralarının içerikleri temizlenerek tüm örnekleri durdurmak yapılabilir. Örnekler her yerde kalır, ancak günlükleri hata iletileriyle kalabalımez. Bu yaklaşım, hızlı prototip geliştirmede idealdir.

> [!WARNING]
> Bu kuyrukların ayrıntıları zaman içinde değişebilir, bu nedenle üretim iş yükleri için bu tekniğin dayanmayın.

### <a name="side-by-side-deployments"></a>Yan yana dağıtımlar

Üst düzey değişikliklerin güvenli bir şekilde dağıtılmasını sağlamanın en son hata kanıtlama yöntemi, eski sürümleriniz ile yan yana dağıtılır. Bu, aşağıdaki tekniklerden herhangi biri kullanılarak yapılabilir:

* Tüm güncelleştirmeleri tamamen yeni işlevler olarak dağıtın ve var olan işlevleri olduğu gibi bırakın. Bu, yeni işlev sürümlerinin çağıranlarının aynı kurallara göre de güncelleştirilmeleri gerektiğinden, bu karmaşık olabilir.
* Tüm güncelleştirmeleri, farklı bir depolama hesabıyla yeni bir işlev uygulaması olarak dağıtın.
* İşlev uygulamasının yeni bir kopyasını aynı depolama hesabıyla, ancak güncelleştirilmiş bir `taskHub` adla dağıtın. Yan yana dağıtımlar önerilen tekniktir.

### <a name="how-to-change-task-hub-name"></a>Görev hub 'ı adını değiştirme

Görev hub 'ı dosyada *host.js* şu şekilde yapılandırılabilir:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "hubName": "MyTaskHubV2"
    }
}
```

#### <a name="functions-20"></a>İşlevler 2,0

```json
{
    "extensions": {
        "durableTask": {
            "hubName": "MyTaskHubV2"
        }
    }
}
```

Dayanıklı İşlevler v1. x için varsayılan değer `DurableFunctionsHub` . Dayanıklı İşlevler v 2.0 'dan başlayarak, varsayılan görev hub 'ı adı, Azure 'daki işlev uygulama adı ile veya `TestHubName` Azure dışında çalışıyor.

Tüm Azure depolama varlıkları `hubName` yapılandırma değerine göre adlandırılır. Görev merkezine yeni bir ad vererek uygulamanızın yeni sürümü için ayrı sıraların ve geçmiş tablosunun oluşturulmasını sağlarsınız. Ancak, işlev uygulaması, önceki görev hub 'ı adı altında oluşturulan düzenlemeler veya varlıklar için olayları işlemeyi durdurur.

İşlev uygulamasının yeni sürümünü yeni bir [dağıtım yuvasına](../functions-deployment-slots.md)dağıtmanızı öneririz. Dağıtım yuvaları, işlev uygulamanızın etkin *Üretim* yuvası olarak yalnızca biri ile yan yana birden çok kopyasını çalıştırmanızı sağlar. Yeni düzenleme mantığını var olan altyapınızla kullanıma sunmaya hazırsanız, yeni sürümü üretim yuvasına değiştirme kadar basit olabilir.

> [!NOTE]
> Bu strateji, Orchestrator işlevleri için HTTP ve Web kancası Tetikleyicileri kullandığınızda en iyi şekilde çalışır. Kuyruklar veya Event Hubs gibi HTTP olmayan Tetikleyiciler için tetikleyici tanımı, değiştirme işleminin bir parçası olarak güncelleştirilmiş [bir uygulama ayarından türetilmelidir](../functions-bindings-expressions-patterns.md#binding-expressions---app-settings) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans ve ölçek sorunlarını nasıl ele alabileceğinizi öğrenin](durable-functions-perf-and-scale.md)
