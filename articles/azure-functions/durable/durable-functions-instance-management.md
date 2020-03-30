---
title: Dayanıklı İşlevlerde örnekleri yönetme - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısındaörnekleri nasıl yönetiştireceklerini öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 07a96fdd6350d8db38a92c23e510afb05f7416fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277758"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Azure'da Dayanıklı İşlevler'deki örnekleri yönetme

Azure [İşlevler için Dayanıklı İşlevler](durable-functions-overview.md) uzantısını kullanıyorsanız veya bunu yapmaya başlamak istiyorsanız, en iyi şekilde kullandığınızdan emin olun. Dayanıklı Fonksiyonlar düzenleme örneklerinizi nasıl yönetebileceğiniz hakkında daha fazla bilgi edinerek optimize edebilirsiniz. Bu makale, her örnek yönetim işleminin ayrıntılarına gider.

Örneğin, örnekleri başlatıp sonlandırabilirsiniz ve tüm örnekleri sorgulayabilme ve örnekleri filtrelerle sorgulayabilme olanağı da dahil olmak üzere örnekleri sorgulayabilirsiniz. Ayrıca, olayları örneklere gönderebilir, orkestrasyon tamamlamayı bekleyebilir ve HTTP yönetimi webhook URL'lerini alabilirsiniz. Bu makalede, örnekleri geri sarma, örnek geçmişi temizleme ve görev hub'ı silme dahil olmak üzere diğer yönetim işlemleri de kapsar.

Dayanıklı İşlevler'de, bu yönetim işlemlerinin her birini nasıl uygulamak istediğinize yönelik seçenekleriniz vardır. Bu makalede, hem .NET (C#) hem de JavaScript için [Azure İşlevler Çekirdek Araçları'nı](../functions-run-local.md) kullanan örnekler verilmektedir.

## <a name="start-instances"></a>Başlangıç örnekleri

Bir orkestrasyon örneğini başlatabilmek önemlidir. Bu genellikle başka bir işlevin tetikleyicibir Dayanıklı Fonksiyonlar bağlama kullanırken yapılır.

`StartNewAsync` [Orkestrasyon istemcisi bağlama](durable-functions-bindings.md#orchestration-client) (.NET) veya `startNew` (JavaScript) yöntemi yeni bir örnek başlatır. Dahili olarak, bu yöntem denetim kuyruğuna bir ileti sıralar, daha sonra düzenleme tetikleyici [bağlama](durable-functions-bindings.md#orchestration-trigger)kullanan belirtilen adı ile bir işlevin başlangıcını tetikler.

Bu async işlemi, orkestrasyon işlemi başarıyla zamanlandığında tamamlanır.

Yeni bir orkestrasyon örneği başlatmak için parametreler şunlardır:

* **Adı**: Programa orchestrator işlevinin adı.
* **Giriş**: Orkestratör işlevine giriş olarak geçirilmesi gereken JSON serileştirilebilir veriler.
* **InstanceId**: (İsteğe Bağlı) Örneğin benzersiz kimliği. Bu parametreyi belirtmezseniz, yöntem rasgele bir kimlik kullanır.

> [!TIP]
> Örnek kimliği için rasgele bir tanımlayıcı kullanın. Rasgele örnek işletmeler, birden fazla VM' de orchestrator fonksiyonlarını ölçekleştirirken eşit bir yük dağıtımı sağlamanıza yardımcı olur. Rasgele olmayan örnek kimliklerini kullanmak için uygun zaman, kimliğin harici bir kaynaktan gelmesi gerektiği veya [singleton orkestratör](durable-functions-singletons.md) deseni uyguladığınız zamandır.

Aşağıdaki kod, yeni bir orkestrasyon örneği başlatan bir örnek işlevdir:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [DurableClient] IDurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

<a name="javascript-function-json"></a>Aksi belirtilmedikçe, bu sayfadaki örnekler aşağıdaki function.json ile HTTP tetikleyicisini kullanır.

**fonksiyon.json**

```json
{
  "bindings": [
    {
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": ["post"]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "name": "starter",
      "type": "durableClient",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

> [!NOTE]
> Bu örnek, Dayanıklı Fonksiyonlar sürüm 2.x'i hedefler. Sürüm 1.x'te `orchestrationClient` , `durableClient`.

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

---

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

Azure [İşlevler Çekirdek Araçları](../functions-run-local.md) `durable start-new` komutunu kullanarak doğrudan bir örnek başlatabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli) : Başlayacak fonksiyonun adı. ** `function-name` **
* (isteğe bağlı) : İşlevin satır veya JSON dosyası aracılığıyla girilen. ** `input` ** Dosyalar için, dosyaya giden yola `@`bir önek `@path/to/file.json`ekleyin, ' gibi .
* (isteğe bağlı) : Orkestrasyon örneğinin kimliği. ** `id` ** Bu parametreyi belirtmezseniz, komut rasgele bir GUID kullanır.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer AzureWebJobsStorage'dır.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan durableFunctionsHub olduğunu. Bunu, dayanıklı Task:HubName'yi kullanarak [host.json](durable-functions-bindings.md#host-json) olarak da ayarlayabilirsiniz.

> [!NOTE]
> Core Tools komutları, bunları bir işlev uygulamasının kök dizininden çalıştırdığınızı varsayar. Parametreleri `connection-string-setting` `task-hub-name` açıkça sağlarsanız, komutları herhangi bir dizinden çalıştırabilirsiniz. Bu komutları işlev uygulaması ana bilgisayarı çalıştırmadan çalıştırabiliyor olsanız da, ana bilgisayar çalışmadığı sürece bazı efektleri gözlemleyemezsiniz. Örneğin, `start-new` komut hedef görev hub'ına bir başlangıç iletisi ekler, ancak iletiyi işleyebilir çalışan bir işlev uygulaması ana bilgisayar işlemi olmadığı sürece orkestrasyon aslında çalışmaz.

Aşağıdaki komut HelloWorld adlı işlevi başlatır ve dosyanın `counter-data.json` içeriğini ona iletir:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Sorgu örnekleri

Orkestrasyonlarınızı yönetme çabanızın bir parçası olarak, büyük olasılıkla bir orkestrasyon örneğinin durumu hakkında (örneğin, normal olarak tamamlanmış veya başarısız olsa da) hakkında bilgi toplamanız gerekir.

`GetStatusAsync` (.NET) veya `getStatus` (JavaScript) yöntem [orkestrasyon istemcisi bağlama](durable-functions-bindings.md#orchestration-client) bir orkestrasyon örneği durumunu sorgular.

Parametre `instanceId` olarak (gerekli), `showHistory` `showHistoryOutput` (isteğe `showInput` bağlı), (isteğe bağlı) ve (isteğe bağlı) alır.

* **`showHistory`**: `true`Ayarlanırsa, yanıt yürütme geçmişini içerir.
* **`showHistoryOutput`**: Olarak `true`ayarlanırsa, yürütme geçmişi etkinlik çıktıları içerir.
* **`showInput`**: `false`Ayarlanırsa, yanıt işlevin girişini içermez. Varsayılan değer: `true`.

Yöntem aşağıdaki özelliklere sahip bir nesne döndürür:

* **Adı**: Orkestratör işlevinin adı.
* **InstanceId**: Orkestrasyonun örnek kimliği `instanceId` (girişle aynı olmalıdır).
* **CreatedTime**: Orkestratör işlevinin çalışmaya başladığı saat.
* **LastUpdatedTime**: Orkestrasyonun son kontrol noktası olduğu saat.
* **Giriş**: Fonksiyonun JSON değeri olarak girişi. Yanlışsa `showInput` bu alan doldurulmaz.
* **CustomStatus**: JSON formatında özel orkestrasyon durumu.
* **Çıktı**: Fonksiyonun JSON değeri olarak çıktısı (işlev tamamlanmışsa). Orkestratör işlevi başarısız olduysa, bu özellik hata ayrıntılarını içerir. Orkestratör işlevi sonlandırıldıysa, bu özellik sonlandırma nedenini (varsa) içerir.
* **RuntimeStatus**: Aşağıdaki değerlerden biri:
  * **Bekleme :** Örnek zamanlandı ancak henüz çalışmaya başlamadı.
  * **Çalışan**: Örnek çalışmaya başladı.
  * **Tamamlandı**: Örnek normal olarak tamamlandı.
  * **ContinuedAsNew**: Örnek kendini yeni bir geçmişle yeniden başlatmıştır. Bu durum geçici bir durumdur.
  * **Başarısız**: Örnek bir hata ile başarısız oldu.
  * **Sonlandırı :** Örnek aniden durduruldu.
* **Tarihçe**: Orkestrasyonun yürütme tarihi. Bu alan yalnızca ' `showHistory` olarak `true`ayarlanırsa doldurulur.

Örnek yoksa, bu yöntem `undefined` (.NET) veya (JavaScript) döndürür. `null`

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    DurableOrchestrationStatus status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

[Azure İşlevler Temel Araçları](../functions-run-local.md) `durable get-runtime-status` komutunu kullanarak doğrudan bir düzenleme örneği durumunu almak da mümkündür. Aşağıdaki parametreleri alır:

* (gerekli) : Orkestrasyon örneğinin kimliği. ** `id` **
* (isteğe bağlı) : `true`Ayarlanırsa, yanıt işlevin girişini içerir. ** `show-input` ** Varsayılan değer: `false`.
* (isteğe bağlı) : `true`Ayarlanırsa, yanıt işlevin çıktısını içerir. ** `show-output` ** Varsayılan değer: `false`.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca [host.json](durable-functions-bindings.md#host-json)ayarlanabilir , dayanıklıTask:HubName kullanılarak.

Aşağıdaki komut 0ab8c55a66644d68a3a8b20b12d209c bir orkestrasyon örneği kimliği ile bir örneğin durumunu (giriş ve çıkış dahil) alır. Komutu işlev uygulamasının `func` kök dizininden çalıştırdığınızı varsayar:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

Bir orkestrasyon örneğinin geçmişini almak için `durable get-history` komutu kullanabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli) : Orkestrasyon örneğinin kimliği. ** `id` **
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca, dayanıklıTask:HubName kullanılarak host.json olarak da ayarlanabilir.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Tüm örnekleri sorgula

Bir defada orkestrasyonunuzda bir örneği sorgulamak yerine, hepsini aynı anda sorgulamayı daha verimli bulabilirsiniz.

Tüm düzenleme `GetStatusAsync` örneklerinin durumlarını `getStatusAll` sorgulamak için (.NET) veya (JavaScript) yöntemini kullanabilirsiniz. .NET'te, iptal `CancellationToken` etmek istediğiniz de bir nesneyi geçirebilirsiniz. Yöntem, parametrelerle `GetStatusAsync` yöntemle aynı özelliklere sahip nesneleri döndürür.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

Azure [İşlevler Temel Araçları](../functions-run-local.md) `durable get-instances` komutunu kullanarak örnekleri doğrudan sorgulamak da mümkündür. Aşağıdaki parametreleri alır:

* (isteğe bağlı) : Bu komut sayfalamayı destekler. ** `top` ** Bu parametre, istek başına alınan örnek sayısına karşılık gelir. Varsayılan değer 10'dur.
* (isteğe bağlı) : Örneklerin hangi sayfaveya bölümünün alınıp alınamasını belirtmek için bir belirteçtir. ** `continuation-token` ** Her `get-instances` yürütme, bir sonraki örnek kümesine bir belirteç döndürür.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca [host.json](durable-functions-bindings.md#host-json)ayarlanabilir , dayanıklıTask:HubName kullanılarak.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Örnekleri filtrelerle sorgulama

Standart bir örnek sorgusunun sağlayabileceği tüm bilgilere gerçekten ihtiyacınız yoksa ne olur? Örneğin, sadece orkestrasyon oluşturma süresini veya orkestrasyon çalışma zamanı durumunu arıyorsanız ne olur? Filtreler uygulayarak sorgunuzu daraltabilirsiniz.

Önceden `GetStatusAsync` tanımlanmış filtreler kümesiyle eşleşen düzenleme örneklerinin listesini almak için (.NET) veya `getStatusBy` (JavaScript) yöntemini kullanın.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [DurableClient] IDurableOrchestrationClient client,
    ILogger log)
{
    var runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

Azure İşlevler Çekirdek Araçları'nda, `durable get-instances` filtreli komutu da kullanabilirsiniz. `top`Yukarıda belirtilen , , `continuation-token`, `connection-string-setting`, ve `task-hub-name` parametrelere ek olarak,`created-after`üç `created-before`filtre `runtime-status`parametresini (, ve ) kullanabilirsiniz.

* (isteğe bağlı) : Bu tarihten/saatten (UTC) sonra oluşturulan örnekleri alın. ** `created-after` ** ISO 8601 biçimlendirilmiş tarih saatleri kabul edilmiştir.
* (isteğe bağlı) : Bu tarihten/tarihten (UTC) önce oluşturulan örnekleri alın. ** `created-before` ** ISO 8601 biçimlendirilmiş tarih saatleri kabul edilmiştir.
* (isteğe bağlı) : Belirli bir statüye sahip örnekleri alın (örneğin, çalışan veya tamamlanmış). ** `runtime-status` ** Birden çok (ayrılmış alan) durum sağlayabilir.
* (isteğe bağlı) : İstek başına alınan örnek sayısı. ** `top` ** Varsayılan değer 10'dur.
* (isteğe bağlı) : Örneklerin hangi sayfaveya bölümünün alınıp alınamasını belirtmek için bir belirteçtir. ** `continuation-token` ** Her `get-instances` yürütme, bir sonraki örnek kümesine bir belirteç döndürür.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca [host.json](durable-functions-bindings.md#host-json)ayarlanabilir , dayanıklıTask:HubName kullanılarak.

Herhangi bir filtre sağlamazsanız`created-after`(, `created-before` `runtime-status`, veya , `top` komut, çalışma süresi durumu veya oluşturma süresi ne olursa olsun, yalnızca örnekleri alır.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Örnekleri sonlandırma

Çalıştırmak için çok uzun süren bir orkestrasyon örneğinvarsa veya herhangi bir nedenle tamamlanmadan önce durdurmanız gerekiyorsa, onu sonlandırma seçeneğiniz vardır.

Örnekleri sonlandırmak için düzenleme `terminate` [istemcisi bağlama](durable-functions-bindings.md#orchestration-client) `TerminateAsync` (.NET) veya (JavaScript) yöntemini kullanabilirsiniz. İki parametre, `instanceId` günlüklere ve örnek durumuna yazılmış bir ve bir `reason` dizedir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

Sona erdirilen bir `Terminated` örnek eninde sonunda devlete geçecek. Ancak, bu geçiş hemen gerçekleşmez. Bunun yerine, sonlandırma işlemi, bu örnekteki diğer işlemlerle birlikte görev hub'ında sıralanacaktır. Sonlandırılan bir [instance query](#query-instances) örneğin gerçekten `Terminated` duruma ne zaman ulaştığını bilmek için örnek sorgu API'lerini kullanabilirsiniz.

> [!NOTE]
> Örnek sonlandırma şu anda yayılmıyor. Etkinlik işlevleri ve alt orkestrasyonlar, onları çağıran orkestrasyon örneğini sonlandırıp sonlandırmadığınıza bakılmaksızın tamamlanmak üzere çalışır.

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

[Azure İşlevler Çekirdek Araçları](../functions-run-local.md) `durable terminate` komutunu kullanarak bir düzenleme örneğini doğrudan sonlandırabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli) : Sonlandırmak için orkestrasyon örneğinin kimliği. ** `id` **
* (isteğe bağlı) : Fesih nedeni. ** `reason` **
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca [host.json](durable-functions-bindings.md#host-json)ayarlanabilir , dayanıklıTask:HubName kullanılarak.

Aşağıdaki komut 0ab8c55a66644d68a3a8b220b12d209c kimliği ile bir düzenleme örneği sona erdirir:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Olayları örneklere gönderme

Bazı senaryolarda, orkestratör işlevlerinizin harici olayları bekleyebilmesi ve dinleyebilmesi önemlidir. Bu [insan etkileşimi](durable-functions-overview.md#monitoring) için bekleyen [human interaction](durable-functions-overview.md#human)monitör işlevleri ve işlevleri içerir.

`RaiseEventAsync` (.NET) yöntemini veya `raiseEvent` (JavaScript) düzenleme [istemcisi bağlama](durable-functions-bindings.md#orchestration-client)yöntemini kullanarak çalışan örneklere olay bildirimleri gönderin. Bu olayları işleyebilir örnekleri `WaitForExternalEvent` (.NET) veya bir `waitForExternalEvent` (JavaScript) arama verim bekleyen durumlardır.

`RaiseEventAsync` (.NET) ve `raiseEvent` (JavaScript) parametreleri aşağıdaki gibidir:

* **InstanceId**: Örneğin benzersiz kimliği.
* **EventName**: Gönderilecek etkinliğin adı.
* **EventData**: Örneğine göndermek için JSON-serializable yük.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

> [!NOTE]
> Belirtilen örnek kimliğiyle bir düzenleme örneği yoksa, olay iletisi atılır. Bir örnek varsa ancak henüz olayı beklemiyorsa, olay alınmaya ve işlenmeye hazır olana kadar örnek durumda depolanır.

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

[Azure İşlevler Temel Araçları](../functions-run-local.md) `durable raise-event` komutunu kullanarak bir olayı doğrudan bir orkestrasyon örneğine yükseltebilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli) : Orkestrasyon örneğinin kimliği. ** `id` **
* **`event-name`**: Yükseltecek olayın adı.
* (isteğe bağlı) : Orkestrasyon örneğine gönderilecek veriler. ** `event-data` ** Bu, bir JSON dosyasına giden yol olabilir veya verileri doğrudan komut satırında sağlayabilirsiniz.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan değer: `DurableFunctionsHub`. Ayrıca [host.json](durable-functions-bindings.md#host-json)ayarlanabilir , dayanıklıTask:HubName kullanılarak.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Orkestrasyonun tamamlanmasını bekleyin

Uzun soluklu orkestrasyonlarda, beklemek ve bir orkestrasyon sonuçlarını almak isteyebilirsiniz. Bu gibi durumlarda, orkestrasyon üzerinde bir zaman önceliği tanımlayabilmek de yararlıdır. Zaman aşımı aşılırsa, sonuçlar yerine orkestrasyonun durumu döndürülmelidir.

`WaitForCompletionOrCreateCheckStatusResponseAsync` (.NET) veya `waitForCompletionOrCreateCheckStatusResponse` (JavaScript) yöntemi, bir orkestrasyon örneğinden eşzamanlı olarak gerçek çıktıyı almak için kullanılabilir. Varsayılan olarak, bu yöntemler için `timeout`10 saniye ve `retryInterval`için 1 saniye varsayılan değer kullanır.  

Aşağıda, bu API'nin nasıl kullanılacağını gösteren bir örnek HTTP tetikleyici işlevi verilmiştir:

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpSyncStart/index.js)]

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

İşlevi aşağıdaki satırla çağırın. Zaman aralığı için 2 saniye ve yeniden deneme aralığı için 0,5 saniye kullanın:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Düzenleme örneğinden yanıt almak için gereken süreye bağlı olarak, iki durum vardır:

* Orkestrasyon örnekleri tanımlanan zaman anında (bu durumda 2 saniye) tamamlanır ve yanıt eşzamanlı olarak teslim edilen gerçek orkestrasyon örneği çıkışıdır:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2018 06:14:29 GMT
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

* Düzenleme örnekleri tanımlanan zaman anına tamamlanamaz ve yanıt [HTTP API URL keşfinde](durable-functions-http-api.md)açıklanan varsayılan örnektir:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2018 06:13:51 GMT
        Location: http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Webhook URL'lerinin biçimi, çalıştırdığınız Azure İşlemeleri ana bilgisayarının hangi sürümüne bağlı olarak farklı olabilir. Önceki örnek, Azure İşlevleri 2.0 ana bilgisayarı içindir.

## <a name="retrieve-http-management-webhook-urls"></a>HTTP yönetimi webhook URL'lerini alın

Olayları izlemek veya orkestrasyon'a yükseltmek için harici bir sistem kullanabilirsiniz. Dış [sistemler, HTTP API URL keşfinde](durable-functions-http-features.md#http-api-url-discovery)açıklanan varsayılan yanıtın bir parçası olan webhook URL'leri aracılığıyla Dayanıklı Fonksiyonlar ile iletişim kurabilir. Webhook URL'lerine alternatif olarak [orkestrasyon istemcisi bağlama](durable-functions-bindings.md#orchestration-client)kullanılarak programlanabilir. `CreateHttpManagementPayload` (.NET) veya `createHttpManagementPayload` (JavaScript) yöntemleri, bu webhook URL'lerini içeren serileştirilebilir bir nesne almak için kullanılabilir.

`CreateHttpManagementPayload` (.NET) ve `createHttpManagementPayload` (JavaScript) yöntemlerinin bir parametresi vardır:

* **instanceId**: Örneğin benzersiz kimliği.

Yöntemler aşağıdaki dize özellikleri ile bir nesne döndürer:

* **Id**: Orkestrasyonun örnek kimliği `InstanceId` (girişle aynı olmalıdır).
* **StatusQueryGetUri**: Orkestrasyon örneğinin durum URL'si.
* **SendEventPostUri**: Orkestrasyon örneğinin "yükseltme etkinliği" URL'si.
* **TerminatePostUri**: Orkestrasyon örneğinin "sonlandırma" URL'si.
* **PurgeHistoryDeleteUri**: Orkestrasyon örneğinin "tasfiye geçmişi" URL'si.

İşlevler, aşağıdaki örneklerde gösterildiği gibi, ilgili orkestrasyonlarla ilgili olayları izlemek veya yükseltmek için bu nesnelerin örneklerini dış sistemlere gönderebilir:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] IDurableActivityContext ctx,
    [DurableClient] IDurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar `DurableActivityContext` 1.x `IDurableActivityContext`için, `OrchestrationClient` `DurableClient` öznitelik yerine öznitelik kullanmanız gerekir ve `DurableOrchestrationClient` `IDurableOrchestrationClient`parametre türünü yerine kullanmanız gerekir. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

## <a name="rewind-instances-preview"></a>Örnekleri geri sarma (önizleme)

Beklenmeyen bir nedenle bir düzenleme hatanız varsa, bu amaç için oluşturulmuş bir API kullanarak örneği daha önce sağlıklı bir duruma *geri sarabilirsiniz.*

> [!NOTE]
> Bu API, uygun hata işleme ve yeniden deneme ilkelerinin yerine geçecek şekilde tasarlanmamıştır. Bunun yerine, yalnızca orkestrasyon örneklerinin beklenmeyen nedenlerle başarısız olduğu durumlarda kullanılması amaçlanmıştır. Hata işleme ve yeniden deneme ilkeleri hakkında daha fazla bilgi için [Hata işleme](durable-functions-error-handling.md) makalesine bakın.

Orkestrasyonu *Çalıştırma* `rewind` durumuna geri koymak için düzenleme [istemcisinin](durable-functions-bindings.md#orchestration-client) `RewindAsync` (.NET) veya (JavaScript) yöntemini kullanın. Bu yöntem, yürütme hatasına neden olan etkinliği veya alt yürütme hatalarını da yeniden çalıştıracaktır.

Örneğin, bir dizi [insan onayını](durable-functions-overview.md#human)içeren bir iş akışınız olduğunu varsayalım. Birisine onayının gerekli olduğunu bildiren bir dizi etkinlik işlevi olduğunu varsayalım ve gerçek zamanlı yanıtı bekleyin. Tüm onay etkinlikleri yanıtları aldıktan veya zaman dolduktan sonra, geçersiz bir veritabanı bağlantı dizesi gibi bir uygulama yanlış yapılandırması nedeniyle başka bir etkinliğin başarısız olduğunu varsayalım. Sonuç, iş akışının derinliklerinde bir düzenleme hatasıdır. `RewindAsync` (.NET) veya `rewind` (JavaScript) API ile, bir uygulama yöneticisi yapılandırma hatasını düzeltebilir ve başarısız olan düzenlemeyi hatadan hemen önce duruma geri sarabilir. İnsan etkileşimi adımlarının hiçbirinin yeniden onaylanması gerekmez ve orkestrasyon artık başarıyla tamamlanabilir.

> [!NOTE]
> *Geri sarma* özelliği, dayanıklı zamanlayıcılar kullanan geri sarma düzenleme örneklerini desteklemez.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

Ayrıca, [Azure İşlevler Temel Araçları](../functions-run-local.md) `durable rewind` komutunu kullanarak doğrudan bir düzenleme örneğini geri sarabilirsiniz. Aşağıdaki parametreleri alır:

* (gerekli) : Orkestrasyon örneğinin kimliği. ** `id` **
* (isteğe bağlı) : Orkestrasyon örneğini geri sarma nedeni. ** `reason` **
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan olarak, [ana bilgisayar.json](durable-functions-bindings.md#host-json) dosyasındaki görev merkezi adı kullanılır.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Temizleme örneği geçmişi

Bir düzenlemeyle ilişkili tüm verileri kaldırmak için örnek geçmişini temizleyebilirsiniz. Örneğin, tamamlanmış bir örnekle ilişkili azure tablo satırlarını ve büyük ileti lekelerini silmek isteyebilirsiniz. Bunu yapmak için, `PurgeInstanceHistoryAsync` [düzenleme istemcisi bağlama](durable-functions-bindings.md#orchestration-client)(.NET) veya `purgeInstanceHistory` (JavaScript) yöntemini kullanın.

Bu yöntemde iki aşırı yükleme vardır. Orkestrasyon örneğinin kimliğine göre ilk aşırı yükleme geçmişi tasfiye:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    return client.purgeInstanceHistory(instanceId);
};
```

Bkz. function.json yapılandırması için [Başlangıç örnekleri.](#javascript-function-json)

---

Sonraki örnek, belirtilen zaman aralığından sonra tamamlanan tüm düzenleme örneklerinin geçmişini temizleyen zamanlayıcı tetiklenen bir işlev gösterir. Bu durumda, 30 veya daha fazla gün önce tamamlanan tüm örnekler için verileri kaldırır. Günde bir kez saat 12:00'de yayınlanması planlanıyor:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [DurableClient] IDurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync(
        DateTime.MinValue,
        DateTime.UtcNow.AddDays(-30),  
        new List<OrchestrationStatus>
        {
            OrchestrationStatus.Completed
        });
}
```

> [!NOTE]
> Önceki C# kodu Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x için `OrchestrationClient` öznitelik yerine `DurableClient` öznitelik kullanmanız gerekir `DurableOrchestrationClient` ve `IDurableOrchestrationClient`parametre türünü yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Yöntem, `purgeInstanceHistoryBy` örnek geçmişini birden çok örnek için koşullu olarak temizlemek için kullanılabilir.

**fonksiyon.json**

```json
{
  "bindings": [
    {
      "schedule": "0 0 12 * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "durableClient",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

> [!NOTE]
> Bu örnek, Dayanıklı Fonksiyonlar sürüm 2.x'i hedefler. Sürüm 1.x'te `orchestrationClient` , `durableClient`.

**index.js**

```javascript
const df = require("durable-functions");

module.exports = async function (context, myTimer) {
    const client = df.getClient(context);
    const createdTimeFrom = new Date(0);
    const createdTimeTo = new Date().setDate(today.getDate() - 30);
    const runtimeStatuses = [ df.OrchestrationRuntimeStatus.Completed ];
    return client.purgeInstanceHistoryBy(createdTimeFrom, createdTimeTo, runtimeStatuses);
};
```

---

> [!NOTE]
> Temizleme geçmişi işleminin başarılı olabilmesi için, hedef örneğin çalışma zamanı durumunun **Tamamlanması,** **Sonlandırılması**veya **Başarısız Olması**Gerekir.

### <a name="azure-functions-core-tools"></a>Azure İşlevler Temel Araçları

[Azure İşlevler Çekirdek Araçları](../functions-run-local.md) `durable purge-history` komutunu kullanarak bir düzenleme örneğinin geçmişini temizleyebilirsiniz. Önceki bölümdeki ikinci C# örneğine benzer şekilde, belirli bir zaman aralığında oluşturulan tüm düzenleme örneklerinin geçmişini temizler. Temizlenmemiş örnekleri çalışma süresi durumuna göre daha fazla filtreleyebilirsiniz. Komutun birkaç parametresi vardır:

* (isteğe bağlı) : Bu tarihten/saatten (UTC) sonra oluşturulan örneklerin geçmişini temizle. ** `created-after` ** ISO 8601 biçimlendirilmiş tarih saatleri kabul edilmiştir.
* (isteğe bağlı) : Bu tarihten/tarihten (UTC) önce oluşturulan örneklerin geçmişini temizle. ** `created-before` ** ISO 8601 biçimlendirilmiş tarih saatleri kabul edilmiştir.
* (isteğe bağlı) : Belirli bir statüye sahip örneklerin geçmişini (örneğin, çalışan veya tamamlanmış) temizle. ** `runtime-status` ** Birden çok (ayrılmış alan) durum sağlayabilir.
* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan olarak, [ana bilgisayar.json](durable-functions-bindings.md#host-json) dosyasındaki görev merkezi adı kullanılır.

Aşağıdaki komut, 14 Kasım 2018 tarihinden önce 19:35'te (UTC) oluşturulan tüm başarısız örneklerin geçmişini siler.

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Görev merkezini silme

Azure [İşlevler Temel Araçları](../functions-run-local.md) `durable delete-task-hub` komutunu kullanarak, Azure depolama tabloları, kuyruklar ve kalıplar da dahil olmak üzere belirli bir görev merkeziyle ilişkili tüm depolama yapılarını silebilirsiniz. Komutun iki parametresi vardır:

* (isteğe bağlı) : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. ** `connection-string-setting` ** Varsayılan değer: `AzureWebJobsStorage`.
* (isteğe bağlı) : Kullanılacak Dayanıklı Fonksiyonlar görev merkezinin adı. ** `task-hub-name` ** Varsayılan olarak, [ana bilgisayar.json](durable-functions-bindings.md#host-json) dosyasındaki görev merkezi adı kullanılır.

Aşağıdaki komut, `UserTest` görev merkeziyle ilişkili tüm Azure depolama verilerini siler.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm le nasıl işleyeceğinizi öğrenin](durable-functions-versioning.md)

> [!div class="nextstepaction"]
> [Örnek yönetimi için yerleşik HTTP API başvurusu](durable-functions-http-api.md)
