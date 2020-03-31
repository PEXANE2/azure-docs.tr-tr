---
title: Dayanıklı Orkestrasyonlar - Azure Fonksiyonları
description: Azure Dayanıklı İşlevler için orkestrasyon özelliğine giriş.
author: cgillum
ms.topic: overview
ms.date: 09/08/2019
ms.author: azfuncdf
ms.openlocfilehash: caa62483373a240991cfec96437cea7849d9b19c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241363"
---
# <a name="durable-orchestrations"></a>Dayanıklı Orkestrasyonlar

Dayanıklı Işlevler, [Azure İşlevlerinin](../functions-overview.md)bir uzantısıdır. Bir işlev uygulaması içindeki diğer Dayanıklı işlevlerin yürütülmesini düzenlemek için bir *orkestratör işlevi* kullanabilirsiniz. Orkestratör işlevleri aşağıdaki özelliklere sahiptir:

* Orchestrator işlevleri yordam kodunu kullanarak işlev iş akışlarını tanımlar. Hiçbir bildirimşeya veya tasarımcıya ihtiyaç yoktur.
* Orchestrator işlevleri diğer dayanıklı işlevleri eşzamanlı ve eşzamanlı olarak çağırabilir. Çağrılan işlevlerden elde edilen çıktılar güvenilir bir şekilde yerel değişkenlere kaydedilebilir.
* Orchestrator fonksiyonları dayanıklı ve güvenilirdir. Yürütme ilerlemesi, işlev "beklediğinde" veya "verildiğinde" otomatik olarak kontrol edilir. İşlem geri dönüşümleri veya VM yeniden başlatıldığında yerel durum hiçbir zaman kaybolmaz.
* Orchestrator fonksiyonları uzun soluklu olabilir. Bir *orkestrasyon örneğinin* toplam ömrü saniye, gün, ay veya hiç bitmeyen olabilir.

Bu makalede, orkestratör işlevleri ve çeşitli uygulama geliştirme zorluklarını çözmenize nasıl yardımcı olabilecekleri hakkında genel bir bakış sunar. Dayanıklı İşlevler uygulamasında bulunan işlev türlerini zaten bilmiyorsanız, önce [Dayanıklı İşlev türleri](durable-functions-types-features-overview.md) makalesini okuyun.

## <a name="orchestration-identity"></a>Orkestrasyon kimliği

Bir orkestrasyonun her *örneğinde* bir örnek tanımlayıcısı *(örnek kimliği*olarak da bilinir) vardır. Varsayılan olarak, her örnek kimliği otomatik olarak oluşturulmuş bir GUID'dir. Ancak, örnek iD'ler de herhangi bir kullanıcı tarafından oluşturulan dize değeri olabilir. Her düzenleme örneği kimliği bir [görev hub'ı](durable-functions-task-hubs.md)içinde benzersiz olmalıdır.

Örnek adlarla ilgili bazı kurallar şunlardır:

* Örnek iLikler 1 ile 256 karakter arasında olmalıdır.
* Örnek IYT'ler `@`ile başlamamalıdır.
* Örnek IYT'ler `/` `\`, `#`, `?` , veya karakter içermemelidir.
* Örnek IYT'ler denetim karakterleri içermemelidir.

> [!NOTE]
> Genellikle mümkün olduğunda otomatik olarak oluşturulmuş örnek iD'ler kullanılması önerilir. Kullanıcı tarafından oluşturulan örnek kimlikler, bir düzenleme örneği ile satınalma siparişi veya belge gibi bazı dış uygulamaya özgü varlık arasında bire bir eşleme bulunan senaryolar için tasarlanmıştır.

Bir orkestrasyonun örnek kimliği, çoğu [örnek yönetim işlemleri](durable-functions-instance-management.md)için gerekli bir parametredir. Sorun giderme veya analiz amacıyla Application Insights'taki [düzenleme izleme verilerini arama](durable-functions-diagnostics.md#application-insights) gibi tanılama için de önemlidir. Bu nedenle, oluşturulan örnek iDilerin daha sonra kolayca başvurulanabilecekleri bazı dış konuma (örneğin, bir veritabanı veya uygulama günlüklerinde) kaydedilmesi önerilir.

## <a name="reliability"></a>Güvenilirlik

Orchestrator [fonksiyonları, olay kaynak](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) tasarım deseni kullanarak yürütme durumunu güvenilir bir şekilde korur. Dayanıklı Görev Çerçevesi, bir düzenlemenin geçerli durumunu doğrudan depolamak yerine, işlev düzenlemenin gerçekleştirdiği eylemlerin tamamını kaydetmek için yalnızca ek bir mağaza kullanır. Yalnızca ek bir mağazanın, tam çalışma süresi durumunu "boşaltma" ile karşılaştırıldığında birçok faydası vardır. Avantajları artan performans, ölçeklenebilirlik ve yanıt içerir. Ayrıca işlem verileri ve tam denetim izleri ve geçmiş için nihai tutarlılık elde edersiniz. Denetim izleri güvenilir telafi edici eylemleri destekler.

Dayanıklı Fonksiyonlar, olayı saydam bir şekilde kullanır. Sahne arkasında, `await` bir orkestratör `yield` işlevindeki (C#) veya (JavaScript) işleci, orkestratör ipliği denetimini Dayanıklı Görev Çerçevesi göndericisine geri verir. Gönderici daha sonra, orkestratörün planladığı yeni eylemleri (örneğin, bir veya daha fazla alt işlevi çağırmak veya dayanıklı bir zamanlayıcıyı zamanlamak gibi) depolamaya adatır. Saydam işleme eylemi, orkestrasyon örneğinin yürütme geçmişine ekler. Geçmiş bir depolama tablosunda depolanır. Daha sonra eylem işleme, fiili çalışmayı zamanlamak için sıraya iletiekler. Bu noktada, orkestratör işlevi bellekten boşaltılabilir.

Bir düzenleme işlevine yapılacak daha fazla iş verildiğinde (örneğin, yanıt iletisi alınır veya dayanıklı bir zamanlayıcının süresi dolduğunda), orkestratör uyanır ve yerel durumu yeniden oluşturmak için tüm işlevi baştan yeniden yürütür. Yeniden yürütme sırasında, kod bir işlevi çağırmaya çalışırsa (veya başka bir async çalışması yaparsa), Dayanıklı Görev Çerçevesi geçerli yürütme nin yürütme geçmişine başvurur. [Etkinlik işlevinin](durable-functions-types-features-overview.md#activity-functions) zaten yürütülmüş ve sonuç vermiş olduğunu bulursa, bu işlevin sonucunu yeniden çalar ve orkestratör kodu çalışmaya devam eder. Yeniden oynatma, işlev kodu tamamlanana veya yeni async çalışması zamanlanana kadar devam eder.

> [!NOTE]
> Yeniden oynatma deseninin doğru ve güvenilir bir şekilde çalışabilmesi için, orkestratör işlev *kodunun belirleyici*olması gerekir. Orkestratör işlevleri için kod kısıtlamaları hakkında daha fazla bilgi [için, orkestratör işlev kodu kısıtlamaları](durable-functions-code-constraints.md) konusuna bakın.

> [!NOTE]
> Bir orkestratör işlevi günlük iletileri yayansa, yinelenen günlük iletilerinin yayımlayabılmasına neden olabilir. Bu davranışın neden oluştuğu ve nasıl çözüm emredilen hakkında daha fazla bilgi edinmek için [Günlüğe Kaydetme](durable-functions-diagnostics.md#logging) konusuna bakın.

## <a name="orchestration-history"></a>Orkestrasyon tarihi

Dayanıklı Görev Çerçevesi'nin olay kaynağı davranışı, yazdığınız orkestratör işlev koduyla yakından birleştiğinde yapılır. Aşağıdaki orkestratör işlevi gibi bir etkinlik zincirleme orkestratör işleviniz olduğunu varsayalım:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return output;
});
```

---

Her `await` (C#) `yield` veya (JavaScript) deyiminde, Dayanıklı Görev Çerçevesi işlevin yürütme durumunu bazı dayanıklı depolama arka ucuna (genellikle Azure Tablo depolama alanı) sınırlar. Bu *durum, orkestrasyon tarihi*olarak adlandırılır.

### <a name="history-table"></a>Geçmiş tablosu

Genel olarak konuşursak, Dayanıklı Görev Çerçevesi her denetim noktasında aşağıdakileri yapar:

1. Yürütme geçmişini Azure Depolama tablolarına kaydeder.
2. Orkestratörün çağırmak istediği işlevler için iletileri sıralar.
3. Örneğin, dayanıklı zamanlayıcı iletileri &mdash; için orkestratör kendisi için enqueues mesajları.

Denetim noktası tamamlandıktan sonra, orkestratör işlevi, yapması gereken daha fazla iş olana kadar bellekten kaldırılmak için ücretsizdir.

> [!NOTE]
> Azure Depolama, tablo depolama sına veri kaydetme ile kuyruklar arasında işlem garantisi sağlamaz. Dayanıklı Işlevler depolama sağlayıcısı, hataları işlemek için *nihai tutarlılık desenleri* kullanır. Bu desenler, bir denetim noktasının ortasında bir kilitlenme veya bağlantı kaybı olduğunda hiçbir veri kaybedilmesini sağlar.

Tamamlandıktan sonra, daha önce gösterilen işlevin geçmişi Azure Tablo Depolama 'daki aşağıdaki tabloya benzer (çizim amacıyla kısaltılır):

| PartitionKey (InstanceId)                     | Olay türü             | Zaman damgası               | Giriş | Adı             | Sonuç                                                    | Durum |
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|
| eaee885b | Yürütme Başlatıldı      | 2017-05-05T18:45:28.852Z | null  | E1_HelloSequence |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     |
| eaee885b | Görev Zamanlandı         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorTamamlandı | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     |
| eaee885b | Görev Tamamlandı         | 2017-05-05T18:45:34.201Z |       |                  | ""Merhaba Tokyo!"""                                        |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     |
| eaee885b | Görev Zamanlandı         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorTamamlandı | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     |
| eaee885b | Görev Tamamlandı         | 2017-05-05T18:45:34.763Z |       |                  | ""Merhaba Seattle!"""                                      |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     |
| eaee885b | Görev Zamanlandı         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorTamamlandı | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     |
| eaee885b | Görev Tamamlandı         | 2017-05-05T18:45:34.919Z |       |                  | ""Merhaba Londra!"""                                       |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorTamamlandı | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     |
| eaee885b | Yürütme Tamamlandı    | 2017-05-05T18:45:35.044Z |       |                  | "[""Merhaba Tokyo!"","Merhaba Seattle!"","Merhaba Londra!""" | Tamamlandı           |

Sütun değerleri hakkında birkaç not:

* **PartitionKey**: Orkestrasyonun örnek kimliğini içerir.
* **EventType**: Olayın türünü temsil eder. Aşağıdaki türlerden biri olabilir:
  * **OrkestrasyonBaşladı**: Orkestratör işlevi bir bekleme den devam veya ilk kez çalışıyor. Sütun, `Timestamp` (.NET) ve `CurrentUtcDateTime` `currentUtcDateTime` (JavaScript) API'leri için deterministik değeri doldurmak için kullanılır.
  * **ExecutionStarted**: Orkestratör işlevi ilk kez yürütmeye başladı. Bu olay, sütundaki işlev `Input` girişini de içerir.
  * **TaskScheduled**: Bir etkinlik işlevi zamanlandı. Etkinlik işlevinin adı `Name` sütunda yakalanır.
  * **Görev Tamamlandı**: Tamamlanan bir etkinlik işlevi. İşlevin sonucu `Result` sütundadır.
  * **TimerCreated**: Dayanıklı bir zamanlayıcı oluşturuldu. Sütun, `FireAt` zamanlayıcının süresinin dolduğu zamanlanan UTC süresini içerir.
  * **TimerFired**: Dayanıklı bir zamanlayıcı ateş.
  * **EventRaised**: Orkestrasyon örneğine harici bir olay gönderildi. Sütun `Name` olayın adını yakalar ve `Input` sütun olayın yükünü yakalar.
  * **OrchestratorCompleted**: Orkestratör işlevi bekleniyor.
  * **ContinueAsNew**: Orkestratör işlevi tamamlandı ve yeni durumla kendini yeniden başlattı. Sütun, `Result` yeniden başlatılan örnekte giriş olarak kullanılan değeri içerir.
  * **Yürütme Tamamlandı**: Orkestratör işlevi tamamlanmak üzere koştu (veya başarısız oldu). İşlevin çıktıları veya hata ayrıntıları `Result` sütunda depolanır.
* **Zaman damgası**: Tarih olayının UTC zaman damgası.
* **Adı**: Çağrılan işlevin adı.
* **Giriş**: Fonksiyonun JSON biçimlendirilmiş girişi.
* **Sonuç**: Fonksiyonun çıktısı; yani, onun iade değeri.

> [!WARNING]
> Hata ayıklama aracı olarak yararlı olsa da, bu tabloya herhangi bir bağımlılık almayın. Dayanıklı Fonksiyonlar uzantısı geliştikçe değişebilir.

İşlev bir `await` (C#) veya `yield` (JavaScript)'den devam edilse, Dayanıklı Görev Çerçevesi orkestratör işlevini sıfırdan yeniden çalıştırır. Her yeniden çalıştırmada, geçerli async işleminin gerçekleşip gerçekleşmediğini belirlemek için yürütme geçmişine başvurur.  İşlem gerçekleştiyse, çerçeve bu işlemin çıktısını hemen yeniden alır ve `await` bir sonraki `yield` (C#) veya (JavaScript) bölümüne geçer. Bu işlem, tüm geçmiş yeniden oynatıldıncaya kadar devam ediyor. Geçerli geçmiş yeniden oynatıldıktan sonra, yerel değişkenler önceki değerlerine geri yüklenmiş olur.

## <a name="features-and-patterns"></a>Özellikler ve desenler

Sonraki bölümlerde orkestratör işlevlerinin özellikleri ve desenleri açıklanabilir.

### <a name="sub-orchestrations"></a>Alt düzenlemeler

Orchestrator işlevleri etkinlik işlevlerini değil, diğer orkestratör işlevlerini de çağırabilir. Örneğin, orkestratör işlevleri kitaplığından daha büyük bir orkestrasyon oluşturabilirsiniz. Veya, bir orkestratör işlevinin birden çok örneğini paralel olarak çalıştırabilirsiniz.

Daha fazla bilgi ve örnekler için [Alt orkestrasyon](durable-functions-sub-orchestrations.md) makalesine bakın.

### <a name="durable-timers"></a>Dayanıklı zamanlayıcılar

Orkestrasyonlar gecikmeleri uygulamak veya async eylemleri üzerinde zaman çizelgesi işleme ayarlamak için *dayanıklı zamanlayıcılar* zamanlayabilirsiniz. `Thread.Sleep` Ve `Task.Delay` (C#) veya `setTimeout()` `setInterval()` (JavaScript) yerine orchestrator işlevlerinde dayanıklı zamanlayıcılar kullanın.

Daha fazla bilgi ve örnekler için [Dayanıklı zamanlayıcılar](durable-functions-timers.md) makalesine bakın.

### <a name="external-events"></a>Dış olaylar

Orchestrator işlevleri harici olayların bir orkestrasyon örneğini güncelleştirmesini bekleyebilir. Bu Dayanıklı Fonksiyonlar özelliği genellikle bir insan etkileşimi veya diğer harici geri arama işleme için yararlıdır.

Daha fazla bilgi ve örnekler için [Dış Olaylar](durable-functions-external-events.md) makalesine bakın.

### <a name="error-handling"></a>Hata işleme

Orchestrator işlevleri programlama dilinin hata işleme özelliklerini kullanabilir. Varolan `try` / `catch` desenler orkestrasyon kodunda desteklenir.

Orchestrator işlevleri, aradıkları etkinlik veya alt orchestrator işlevlerine yeniden deneme ilkeleri de ekleyebilir. Bir etkinlik veya alt orchestrator işlevi bir istisna dışında başarısız olursa, belirtilen yeniden deneme ilkesi otomatik olarak geciktirebilir ve yürütmeyi belirtilen sayıda yeniden deneyebilir.

> [!NOTE]
> Bir orkestratör işlevinde işlenmemiş bir özel durum varsa, orkestrasyon örneği bir `Failed` durumda tamamlanır. Bir orkestrasyon örneği başarısız olduktan sonra yeniden denenemez.

Daha fazla bilgi ve örnekler için [Hata işleme](durable-functions-error-handling.md) makalesine bakın.

### <a name="critical-sections-durable-functions-2x-currently-net-only"></a>Kritik bölümler (Dayanıklı Fonksiyonlar 2.x, şu anda sadece .NET)

Orkestrasyon örnekleri tek dişlidir, bu nedenle bir orkestrasyon *içindeki* Yarış koşulları hakkında endişelenmenize gerek yoktur. Ancak, orkestrasyonlar dış sistemlerle etkileşime girdiğinde Yarış koşulları mümkündür. Harici sistemlerle etkileşimde bulunan yarış koşullarını azaltmak için, orkestratör işlevleri `LockAsync` .NET'te bir yöntem kullanarak *kritik bölümleri* tanımlayabilir.

Aşağıdaki örnek kod, kritik bir bölümü tanımlayan bir orkestratör işlevini gösterir. Yöntemi kullanarak kritik bölüme `LockAsync` girer. Bu yöntem, kilit durumunu kalıcı olarak yöneten [Dayanıklı Varlık'a](durable-functions-entities.md)bir veya daha fazla başvuru nun geçirilmesini gerektirir. Bu düzenlemenin yalnızca tek bir örneği, kodu kritik bölümdeki bir anda yürütebilir.

```csharp
[FunctionName("Synchronize")]
public static async Task Synchronize(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    var lockId = new EntityId("LockEntity", "MyLockIdentifier");
    using (await context.LockAsync(lockId))
    {
        // critical section - only one orchestration can enter at a time
    }
}
```

Dayanıklı `LockAsync` kilidi(ler) elde eder ve `IDisposable` imha edildiğinde kritik bölümü sona erdiren bir döndürür. Bu `IDisposable` sonuç, kritik bölümün `using` sözdizimi temsilini almak için bir blokla birlikte kullanılabilir. Bir orkestratör işlevi kritik bir bölüme girdiğinde, bu kod bloğunu yalnızca bir örnek yürütebilir. Kritik bölüme girmeye çalışan diğer örnekler, önceki örnek kritik bölümden çıkana kadar engellenir.

Kritik bölüm özelliği, dayanıklı varlıklardaki değişiklikleri koordine etmek için de yararlıdır. Kritik bölümler hakkında daha fazla bilgi için [Dayanıklı varlıklar "Varlık koordinasyonu"](durable-functions-entities.md#entity-coordination) konusuna bakın.

> [!NOTE]
> Kritik bölümler Dayanıklı Fonksiyonlar 2.0 ve üzeri mevcuttur. Şu anda bu özelliği yalnızca .NET orkestrasyonları uygular.

### <a name="calling-http-endpoints-durable-functions-2x"></a>HTTP uç noktalarını arama (Dayanıklı Fonksiyonlar 2.x)

Orchestrator işlevleri, [orchestrator işlev kodu kısıtlamalarında](durable-functions-code-constraints.md)açıklandığı gibi G/Ç yapmak için izin verilmez. Bu sınırlama için tipik geçici çözüm, bir etkinlik işlevinde G/Ç yapması gereken herhangi bir kodu kaydırmaktır. Dış sistemlerle etkileşimedebilen orkestrasyonlar, HTTP aramaları yapmak ve sonucu orkestrasyona döndürmek için etkinlik işlevlerini sık sık kullanır.

# <a name="c"></a>[C #](#tab/csharp)

Bu ortak deseni basitleştirmek için, `CallHttpAsync` orkestratör işlevleri http API'leri doğrudan çağırmak için yöntemi kullanabilir.

```csharp
[FunctionName("CheckSiteAvailable")]
public static async Task CheckSiteAvailable(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    Uri url = context.GetInput<Uri>();

    // Makes an HTTP GET request to the specified endpoint
    DurableHttpResponse response = 
        await context.CallHttpAsync(HttpMethod.Get, url);

    if (response.StatusCode >= 400)
    {
        // handling of error codes goes here
    }
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const url = context.df.getInput();
    var res = yield context.df.callHttp("GET", url);
    if (res.statusCode >= 400) {
        // handling of error codes goes here
    }
});
```

---

Temel istek/yanıt kalıplarını desteklemeye ek olarak, yöntem ortak async HTTP 202 yoklama desenleri otomatik işleme destekler ve aynı zamanda [Yönetilen Kimlikleri](../../active-directory/managed-identities-azure-resources/overview.md)kullanarak dış hizmetler ile kimlik doğrulama destekler.

Daha fazla bilgi ve ayrıntılı örnekler için [HTTP özellikleri](durable-functions-http-features.md) makalesine bakın.

> [!NOTE]
> Doğrudan orchestrator işlevlerinden HTTP uç noktaları arama Dayanıklı Fonksiyonlar 2.0 ve üzeri mevcuttur.

### <a name="passing-multiple-parameters"></a>Birden çok parametre nin geçirilmesi

Birden çok parametreyi doğrudan bir etkinlik işlevine aktarmak mümkün değildir. Öneri, bir dizi nesne veya bileşik nesnede geçmektir.

# <a name="c"></a>[C #](#tab/csharp)

.NET'te [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) nesnelerini de kullanabilirsiniz. Aşağıdaki [örnek, C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples)ile eklenen [ValueTuples'ın](https://docs.microsoft.com/dotnet/csharp/tuples) yeni özelliklerini kullanıyor:

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<object> RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    object courseRecommendations = await context.CallActivityAsync<object>(
        "CourseRecommendations",
        (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<object> Mapper([ActivityTrigger] IDurableActivityContext inputs)
{
    // parse input for student's major and year in university
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new
    {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

#### <a name="orchestrator"></a>Orchestrator

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const location = {
        city: "Seattle",
        state: "WA"
    };
    const weather = yield context.df.callActivity("GetWeather", location);

    // ...
};
```

#### <a name="activity"></a>Etkinlik

```javascript
module.exports = async function (context, location) {
    const {city, state} = location; // destructure properties into variables

    // ...
};
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Düzenleyici kodu kısıtlamaları](durable-functions-code-constraints.md)
