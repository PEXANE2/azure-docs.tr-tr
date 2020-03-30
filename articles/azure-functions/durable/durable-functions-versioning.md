---
title: Dayanıklı İşlevlerde Sürüm - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısında sürüm uygulamasını öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: 87cbb94dbab241630dc7585bdf4314d858d5b4da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74232750"
---
# <a name="versioning-in-durable-functions-azure-functions"></a>Dayanıklı İşlevlerde Sürüm (Azure İşlevleri)

Bir uygulamanın ömrü boyunca işlevlerin eklenmesi, kaldırılması ve değiştirilmesi kaçınılmazdır. [Dayanıklı Fonksiyonlar,](durable-functions-overview.md) daha önce mümkün olmayan şekillerde zincirleme işlevleri bir araya sağlar ve bu zincirleme sürüm işleme nasıl etkiler.

## <a name="how-to-handle-breaking-changes"></a>Kesme değişiklikleri nasıl işleyebilir?

Dikkat edilmesi gereken kırılma değişikliklerinin birkaç örneği vardır. Bu makalede, en yaygın olanları tartışır. Hepsinin arkasındaki ana tema, hem yeni hem de varolan işlev orkestrasyonların işlev kodundaki değişikliklerden etkilenmesidir.

### <a name="changing-activity-or-entity-function-signatures"></a>Etkinlik veya varlık işlev imzalarını değiştirme

İmza değişikliği, bir işlevin adında, girişinde veya çıktısındaki bir değişikliği ifade eder. Bu tür bir değişiklik bir etkinlik veya varlık işlevine yapılırsa, buna bağlı olan herhangi bir orchestrator işlevini bozabilir. Bu değişikliği karşılamak için orkestratör işlevini güncellerseniz, varolan uçuş örneklerini kırabilirsiniz.

Örnek olarak, aşağıdaki orkestratör işlevine sahip olduğumuzu varsayalım.

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Bu basit fonksiyon **Foo** sonuçlarını alır ve **Bar**geçer. Daha geniş bir sonuç değerini desteklemek `bool` için `int` **Foo'nun** geri dönüş değerini değiştirmemiz gerektiğini varsayalım. Sonuç şu na benzer:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    int result = await context.CallActivityAsync<int>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x'i hedefalır. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

Bu değişiklik, orkestratör işlevinin tüm yeni örnekleri için iyi çalışır, ancak uçuş içi örnekleri bozar. Örneğin, bir orkestrasyon örneği adlı `Foo`bir işlev çağırır durumda düşünün , bir boolean değeri geri alır ve sonra denetim noktaları. İmza değişikliği bu noktada dağıtılırsa, denetim işaretli örnek devam ettiğinde ve aramayı `context.CallActivityAsync<int>("Foo")`yeniden oynattığında hemen başarısız olur. Bu hata, geçmiş tablosundaki sonuç `bool` ancak yeni kod onu `int`deserialize etmeye çalıştığından olur.

Bu örnek, imza değişikliğinin varolan örnekleri kırabileceği birçok farklı yoldan sadece biridir. Genel olarak, bir orkestratörbir işlevi çağırır şekilde değiştirmek gerekiyorsa, o zaman değişiklik sorunlu olması muhtemeldir.

### <a name="changing-orchestrator-logic"></a>Orkestratör mantığını değiştirme

Diğer sürüm sorunları sınıfı, orkestratör işlev kodunu, uçuş içi örneklerin yeniden oynatma mantığını karıştıracak şekilde değiştirmekten gelir.

Aşağıdaki orkestratör işlevini göz önünde bulundurun:

```csharp
[FunctionName("FooBar")]
public static Task Run([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    bool result = await context.CallActivityAsync<bool>("Foo");
    await context.CallActivityAsync("Bar", result);
}
```

Şimdi başka bir işlev çağrısı eklemek için masum gibi görünen bir değişiklik yapmak istediğinizi varsayalım.

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
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x'i hedefalır. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

Bu **değişiklik, Foo** ve **Bar**arasında **Bildirim Gönder'e** yeni bir işlev çağrısı ekler. İmza değişikliği yok. Sorun, varolan bir örnek çağrıdan **Çubuk'a**devam ettiğinde ortaya çıkar. Tekrar sırasında, **Foo'ya** yapılan `true`orijinal arama geri döndüyse, orkestratör tekrarı, yürütme geçmişinde olmayan **SendNotification'a**çağrılacaktır. Sonuç olarak, Dayanıklı Görev Çerçevesi, `NonDeterministicOrchestrationException` **Çubuk'a**bir çağrı görmesi beklenirken **Bildirim Gönder'e** bir çağrıyla karşılaştığı için başarısız olur. "Dayanıklı" API'lere , `CreateTimer`vb. `WaitForExternalEvent`dahil olmak üzere herhangi bir çağrı eklerken aynı tür de sorun oluşabilir.

## <a name="mitigation-strategies"></a>Azaltma stratejileri

Sürüm zorluklarıyla başa çıkmak için stratejilerden bazıları şunlardır:

* Hiçbir şey yapma.
* Tüm uçuş örneklerini durdurun
* Yan yana dağıtımlar

### <a name="do-nothing"></a>Hiçbir şey yapma.

Bir kırılma değişikliği işlemek için en kolay yolu, uçuş orkestrasyon örnekleri başarısız izin vermektir. Yeni örnekler değiştirilen kodu başarıyla çalıştırın.

Bu tür bir hatanın bir sorun olup olmadığı, uçuş içi örneklerinizin önemine bağlıdır. Eğer aktif geliştirme ve uçuş örnekleri umurumda değil, bu yeterince iyi olabilir. Ancak, tanılama ardışık ardınızdaki özel durumlar ve hatalarla başa çıkmanız gerekir. Bu tür şeylerden kaçınmak istiyorsanız, diğer sürüm seçeneklerini göz önünde bulundurun.

### <a name="stop-all-in-flight-instances"></a>Tüm uçuş örneklerini durdurun

Başka bir seçenek tüm uçuş örnekleri durdurmaktır. Tüm örnekleri durdurma iç **denetim sırası** ve iş öğesi **sırası** kuyruklarının içeriğini temizleyerek yapılabilir. Örnekler sonsuza kadar oldukları yerde sıkışıp kalacak, ancak hata iletileri ile günlükleri yığılmayı olmaz. Bu yaklaşım hızlı prototip geliştirme de idealdir.

> [!WARNING]
> Bu kuyrukların ayrıntıları zaman içinde değişebilir, bu nedenle üretim iş yükleri için bu tekniğe güvenmeyin.

### <a name="side-by-side-deployments"></a>Yan yana dağıtımlar

Kesme değişikliklerinin güvenli bir şekilde dağıtılmasını sağlamanın en başarısız olmasının en iyi yolu, bunları eski sürümlerinizle yan yana dağıtmaktır. Bu, aşağıdaki tekniklerden herhangi biri kullanılarak yapılabilir:

* Varolan işlevleri olduğu gibi bırakarak, tüm güncelleştirmeleri tamamen yeni işlevler olarak dağıtın. Yeni işlev sürümlerini arayanların da aynı yönergeleri izleyerek güncelleştirilmesi gerektiğinden, bu zor olabilir.
* Tüm güncelleştirmeleri farklı bir depolama hesabı yla yeni bir işlev uygulaması olarak dağıtın.
* İşlev uygulamasının yeni bir kopyasını aynı depolama hesabına, ancak güncelleştirilmiş `taskHub` bir adla dağıtın. Yan yana dağıtımlar önerilen tekniktir.

### <a name="how-to-change-task-hub-name"></a>Görev merkezi adı nasıl değiştirilir?

Görev hub'ı *ana bilgisayar.json* dosyasında aşağıdaki gibi yapılandırılabilir:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "hubName": "MyTaskHubV2"
    }
}
```

#### <a name="functions-20"></a>Fonksiyonlar 2.0

```json
{
    "extensions": {
        "durableTask": {
            "hubName": "MyTaskHubV2"
        }
    }
}
```

Dayanıklı Fonksiyonlar v1.x için `DurableFunctionsHub`varsayılan değer . Dayanıklı İşlevler v2.0'dan başlayarak, varsayılan görev hub adı Azure'daki işlev uygulaması adı ile veya Azure dışında `TestHubName` çalışıyorsa aynıdır.

Tüm Azure Depolama varlıkları yapılandırma `hubName` değerine göre adlandırılır. Görev merkezine yeni bir ad vererek, uygulamanızın yeni sürümü için ayrı kuyruklar ve geçmiş tablosu oluşturulduğundan emin olabilirsiniz. Ancak işlev uygulaması, önceki görev merkezi adı altında oluşturulan orkestrasyonlar veya varlıklar için olayları işlemeyi durdurur.

İşlev uygulamasının yeni sürümünü yeni bir [Dağıtım Yuvasına](../functions-deployment-slots.md)dağıtmanızı öneririz. Dağıtım yuvaları, işlev uygulamanızın birden çok kopyasını etkin *üretim* yuvası olarak yalnızca bir tanesiyle yan yana çalıştırmanızı sağlar. Yeni orkestrasyon mantığını mevcut altyapınıza maruz bırakmaya hazır olduğunuzda, yeni sürümü üretim yuvasına dönüştürmek kadar basit olabilir.

> [!NOTE]
> Bu strateji, orkestratör işlevleri için HTTP ve webhook tetikleyicileri kullandığınızda en iyi şekilde çalışır. Kuyruklar veya Olay Hub'ları gibi HTTP olmayan tetikleyiciler için, tetikleyici tanımı takas işleminin bir parçası olarak güncelleştirilen [bir uygulama ayarından türemelidir.](../functions-bindings-expressions-patterns.md#binding-expressions---app-settings)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Performans ve ölçek sorunlarıyla nasıl başa çıkacağınızı öğrenin](durable-functions-perf-and-scale.md)
