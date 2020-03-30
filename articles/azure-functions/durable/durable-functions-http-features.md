---
title: Dayanıklı İşlevlerde HTTP özellikleri - Azure Fonksiyonları
description: Azure İşlevler için Dayanıklı İşlevler uzantısındaki tümleşik HTTP özellikleri hakkında bilgi edinin.
author: cgillum
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 29d837446960b7535b26284efdfab7a1c59ea968
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132507"
---
# <a name="http-features"></a>HTTP Özellikleri

Dayanıklı Fonksiyonlar, dayanıklı orkestrasyonları ve varlıkları HTTP iş akışlarına dahil etmeyi kolaylaştıran çeşitli özelliklere sahiptir. Bu makalede, bu özelliklerden bazıları hakkında ayrıntılı olarak gider.

## <a name="exposing-http-apis"></a>HTTP API'leri açığa çıkarma

Orkestrasyonlar ve varlıklar HTTP istekleri kullanılarak çağrılabilir ve yönetilebilir. Dayanıklı İşlevler uzantısı yerleşik HTTP API'leri ortaya çıkarır. Ayrıca, HTTP tarafından tetiklenen işlevler içinden gelen orkestrasyonlar ve varlıklarla etkileşim kurmak için API'ler sağlar.

### <a name="built-in-http-apis"></a>Dahili HTTP API'leri

Dayanıklı Işlevler uzantısı, Azure İşlevleri ana bilgisayara otomatik olarak bir dizi HTTP API ekler. Bu API'lerle, herhangi bir kod yazmadan orkestrasyonlar ve varlıklarla etkileşim kurabilir ve yönetebilirsiniz.

Aşağıdaki yerleşik HTTP API'leri desteklenir.

* [Yeni orkestrasyon başlatın](durable-functions-http-api.md#start-orchestration)
* [Sorgu düzenleme örneği](durable-functions-http-api.md#get-instance-status)
* [Düzenleme örneğini sonlandırma](durable-functions-http-api.md#terminate-instance)
* [Harici bir olayı orkestrasyona gönderme](durable-functions-http-api.md#raise-event)
* [Tasfiye orkestrasyon tarihi](durable-functions-http-api.md#purge-single-instance-history)
* [Bir kuruluşa işlem olayı gönderme](durable-functions-http-api.md#signal-entity)
* [Bir varlığın durumunu alma](durable-functions-http-api.md#get-entity)
* [Varlıkların listesini sorgula](durable-functions-http-api.md#list-entities)

Dayanıklı İşlevler uzantısı tarafından açığa çıkarılan tüm yerleşik HTTP API'lerinin tam bir açıklaması için [HTTP API'ler makalesine](durable-functions-http-api.md) bakın.

### <a name="http-api-url-discovery"></a>HTTP API URL bulma

[Orkestrasyon istemcisi bağlama,](durable-functions-bindings.md#orchestration-client) kullanışlı HTTP yanıt yükleri oluşturabilen API'leri ortaya çıkarır. Örneğin, belirli bir orkestrasyon örneği için yönetim API'lerine bağlantılar içeren bir yanıt oluşturabilir. Aşağıdaki örnekler, bu API'nin yeni bir düzenleme örneği için nasıl kullanılacağını gösteren bir HTTP tetikleyici işlevi gösterir:

# <a name="c"></a>[C #](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

# <a name="javascript"></a>[Javascript](#tab/javascript)

**index.js**

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

**fonksiyon.json**

[!code-json[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json)]

---

Daha önce gösterilen HTTP-tetik işlevlerini kullanarak bir orkestratör işlevi başlatma herhangi bir HTTP istemcisi kullanılarak yapılabilir. Aşağıdaki cURL komutu adlı `DoWork`bir orkestratör işlevi başlatır:

```bash
curl -X POST https://localhost:7071/orchestrators/DoWork -H "Content-Length: 0" -i
```

Sonraki kimliği olarak olan `abc123` bir orkestrasyon için örnek bir yanıttır. Bazı ayrıntılar netlik için kaldırıldı.

```http
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX
Retry-After: 10

{
    "id": "abc123",
    "purgeHistoryDeleteUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "sendEventPostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/raiseEvent/{eventName}?code=XXX",
    "statusQueryGetUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123?code=XXX",
    "terminatePostUri": "http://localhost:7071/runtime/webhooks/durabletask/instances/abc123/terminate?reason={text}&code=XXX"
}
```

Önceki örnekte, biten alanların her `Uri` biri yerleşik bir HTTP API'ye karşılık gelir. Hedef düzenleme örneğini yönetmek için bu API'leri kullanabilirsiniz.

> [!NOTE]
> Webhook URL'lerinin biçimi, Hangi Azure İşlevlerini çalıştırdığınıza bağlıdır. Önceki örnek, Azure İşlevler 2.0 ana bilgisayarı içindir.

Tüm yerleşik HTTP API'lerinin açıklaması için [HTTP API başvurusuna](durable-functions-http-api.md)bakın.

### <a name="async-operation-tracking"></a>Async işlem izleme

Daha önce bahsedilen HTTP yanıtı, uzun süredir devam eden VEDa İşlevlere sahip HTTP async API'leri uygulanmasına yardımcı olmak üzere tasarlanmıştır. Bu desen bazen *yoklama tüketici deseni*olarak adlandırılır. İstemci/sunucu akışı aşağıdaki gibi çalışır:

1. İstemci, bir orkestratör işlevi gibi uzun süren bir işlemi başlatmak için bir HTTP isteği yayınlar.
1. Hedef HTTP tetikleyicideğeri "statusQueryGetUri" olan bir Konum üstbilgisi ile bir HTTP 202 yanıtı döndürür.
1. İstemci URL'yi Konum başlığında yoklar. İstemci http 202 yanıtlarını konum üstbilgisiyle görmeye devam ediyor.
1. Örnek bittiğinde veya başarısız olduğunda, Konum üstbilgisindeki bitiş noktası HTTP 200'ü döndürür.

Bu protokol, bir HTTP bitiş noktasını yoklayabilen ve Konum üstbilgisini takip eden dış istemciler veya hizmetlerle uzun süren işlemlerin koordinasyonuna olanak tanır. Bu desenin hem istemci hem de sunucu uygulamaları Dayanıklı Fonksiyonlar HTTP API'lerinde yerleşiktir.

> [!NOTE]
> Varsayılan olarak, [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) tarafından sağlanan tüm HTTP tabanlı eylemler standart eşzamanlı işlem modelini destekler. Bu özellik, Logic Apps iş akışının bir parçası olarak uzun süredir çalışan dayanıklı bir işlevi gömmeyi mümkün kılar. [Azure Logic Apps iş akışı eylemlerinde ve belgeleri tetikleyen](../../logic-apps/logic-apps-workflow-actions-triggers.md)asynchronous HTTP desenleri için Logic Apps desteği hakkında daha fazla ayrıntı bulabilirsiniz.

> [!NOTE]
> Orkestrasyonlarla etkileşimler sadece HTTP tarafından tetiklenen işlevlerden değil, herhangi bir işlev türünden yapılabilir.

İstemci API'lerini kullanarak orkestrasyonların ve varlıkların nasıl yönetileceği hakkında daha fazla bilgi için [Örnek yönetimi makalesine](durable-functions-instance-management.md)bakın.

## <a name="consuming-http-apis"></a>HTTP API'leri Tüketme

[Orchestrator işlev kodu kısıtlamalarıaçıklandığı](durable-functions-code-constraints.md)gibi, orkestratör işlevleri doğrudan G /O yapamaz. Bunun yerine, genellikle G/Ç işlemleri yapan [etkinlik işlevlerini](durable-functions-types-features-overview.md#activity-functions) çağırırlar.

Dayanıklı Fonksiyonlar 2.0 ile başlayarak, orkestrasyon lar yerel olarak [düzenleme tetikleme bağlama](durable-functions-bindings.md#orchestration-trigger)kullanarak HTTP API'leri tüketebilir.

Aşağıdaki örnek kod, giden bir HTTP isteği yapan bir orkestratör işlevini gösterir:

# <a name="c"></a>[C #](#tab/csharp)

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

module.exports = df.orchestrator(function*(context){
    const url = context.df.getInput();
    const response = context.df.callHttp("GET", url)

    if (response.statusCode >= 400) {
        // handling of error codes goes here
    }
});
```

---

"HTTP'yi ara" eylemini kullanarak, orkestratör işlevlerinizde aşağıdaki eylemleri yapabilirsiniz:

* Http API'leri daha sonra belirtilen bazı sınırlamalarla doğrudan orkestrasyon işlevlerinden arayın.
* İstemci tarafındaki HTTP 202 durum yoklama modellerini otomatik olarak destekleyin.
* Diğer Azure uç noktalarına yetkili HTTP aramaları yapmak için [Azure Yönetilen Kimlikler'i](../../active-directory/managed-identities-azure-resources/overview.md) kullanın.

HTTP API'leri doğrudan orchestrator işlevlerinden tüketme olanağı, belirli bir dizi ortak senaryo için kolaylık sağlamak amacıyla tasarlanmıştır. Tüm bu özellikleri etkinlik işlevlerini kullanarak kendiniz uygulayabilirsiniz. Çoğu durumda, etkinlik işlevleri size daha fazla esneklik sağlayabilir.

### <a name="http-202-handling"></a>HTTP 202 işleme

"ÇAĞRı HTTP" API otomatik olarak anket tüketici deseni istemci tarafı uygulayabilirsiniz. Çağrılan bir API Konum üstbilgisiyle bir HTTP 202 yanıtını döndürürse, orkestratör işlevi 202'den başka bir yanıt alana kadar Konum kaynağını otomatik olarak yoklar. Bu yanıt, orchestrator işlev koduna döndürülen yanıt olacaktır.

> [!NOTE]
> Orchestrator işlevleri, [Async işlem izlemede](#async-operation-tracking)açıklandığı gibi, sunucu tarafındaki yoklama tüketici deseni de doğal olarak destekler. Bu destek, tek bir işlev uygulamasındaki orkestrasyonların diğer işlev uygulamalarındaki orkestratör işlevlerini kolayca koordine edebileceği anlamına gelir. Bu [alt-orkestrasyon](durable-functions-sub-orchestrations.md) kavramına benzer, ancak çapraz uygulama iletişimi desteği ile. Bu destek özellikle microservice tarzı uygulama geliştirme için yararlıdır.

### <a name="managed-identities"></a>Yönetilen kimlikler

Dayanıklı İşlevler, yetkilendirme için Azure Etkin Dizin (Azure AD) belirteçlerini kabul eden API'lere yapılan çağrıları destekler. Bu destek, bu belirteçleri elde etmek için [Azure yönetilen kimlikleri](../../active-directory/managed-identities-azure-resources/overview.md) kullanır.

Aşağıdaki kod bir .NET orchestrator işlevinin bir örneğidir. İşlev, Azure Kaynak Yöneticisi sanal makineleri REST [API](https://docs.microsoft.com/rest/api/compute/virtualmachines)kullanarak sanal bir makineyi yeniden başlatmak için kimlik doğrulaması çağrıları yapar.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("RestartVm")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string subscriptionId = "mySubId";
    string resourceGroup = "myRG";
    string vmName = "myVM";
    string apiVersion = "2019-03-01";
    
    // Automatically fetches an Azure AD token for resource = https://management.core.windows.net
    // and attaches it to the outgoing Azure Resource Manager API call.
    var restartRequest = new DurableHttpRequest(
        HttpMethod.Post, 
        new Uri($"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Compute/virtualMachines/{vmName}/restart?api-version={apiVersion}"),
        tokenSource: new ManagedIdentityTokenSource("https://management.core.windows.net"));
    DurableHttpResponse restartResponse = await context.CallHttpAsync(restartRequest);
    if (restartResponse.StatusCode != HttpStatusCode.OK)
    {
        throw new ArgumentException($"Failed to restart VM: {restartResponse.StatusCode}: {restartResponse.Content}");
    }
}
```

# <a name="javascript"></a>[Javascript](#tab/javascript)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const subscriptionId = "mySubId";
    const resourceGroup = "myRG";
    const vmName = "myVM";
    const apiVersion = "2019-03-01";
    const tokenSource = new df.ManagedIdentityTokenSource("https://management.core.windows.net");

    // get a list of the Azure subscriptions that I have access to
    const restartResponse = yield context.df.callHttp(
        "POST",
        `https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroup}/providers/Microsoft.Compute/virtualMachines/${vmName}/restart?api-version=${apiVersion}`,
        undefined, // no request content
        undefined, // no request headers (besides auth which is handled by the token source)
        tokenSource);

    return restartResponse;
});
```

---

Önceki örnekte, `tokenSource` parametre [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md)için Azure AD belirteçleri elde etmek için yapılandırılır. Belirteçleri kaynak URI `https://management.core.windows.net`tarafından tanımlanır. Örnek, geçerli işlev uygulamasının yerel olarak çalıştığını veya yönetilen bir kimliğe sahip bir işlev uygulaması olarak dağıtıldığını varsayar. Yerel kimlik veya yönetilen kimlik belirtilen kaynak grubunda `myRG`VM'leri yönetme iznine sahip olduğu varsayılır.

Çalışma zamanında, yapılandırılan belirteç kaynağı otomatik olarak bir OAuth 2.0 erişim belirteci döndürür. Kaynak daha sonra giden isteğin Yetkilendirme üstbilgisine taşıyıcı belirteci olarak belirteci ekler. Bu model, aşağıdaki nedenlerden dolayı HTTP isteklerine yetkilendirme üstbilgilerini el ile eklemeye yönelik bir gelişmedir:

* Belirteç yenilemesi otomatik olarak işlenir. Süresi dolmuş jetonlar hakkında endişelenmenize gerek yok.
* Belirteçler asla dayanıklı orkestrasyon durumunda depolanır.
* Belirteç edinimi yönetmek için herhangi bir kod yazmanız gerekmez.

[Önceden derlenmiş C# RestartVMs örneğinde](https://github.com/Azure/azure-functions-durable-extension/blob/v2/samples/v2/precompiled/RestartVMs.cs)daha eksiksiz bir örnek bulabilirsiniz.

Yönetilen kimlikler Azure kaynak yönetimiyle sınırlı değildir. Microsoft'un Azure hizmetleri ve iş ortaklarından gelen web uygulamaları da dahil olmak üzere Azure AD taşıyıcı belirteçlerini kabul eden tüm API'lere erişmek için yönetilen kimlikleri kullanabilirsiniz. Bir ortağın web uygulaması başka bir işlev uygulaması bile olabilir. Microsoft'un Azure AD ile kimlik doğrulamasını destekleyen Azure hizmetlerinin listesi için Azure [AD kimlik doğrulamasını destekleyen Azure hizmetlerine](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)bakın.

### <a name="limitations"></a>Sınırlamalar

HTTP API'leri aramak için yerleşik destek bir kolaylık özelliğidir. Tüm senaryolar için uygun değil.

Orchestrator işlevleri tarafından gönderilen HTTP istekleri ve yanıtları seri hale getirilmiş ve sıra iletisi olarak kalıcıdır. Bu kuyruk davranışı, HTTP çağrılarının [düzenleme tekrarı için güvenilir ve güvenli](durable-functions-orchestrations.md#reliability)olmasını sağlar. Ancak, kuyruk davranışı da sınırlamaları vardır:

* Her HTTP isteği, yerel bir HTTP istemcisi ile karşılaştırıldığında ek gecikme gerektirir.
* Sıra iletisine sığmayacak büyük istek veya yanıt iletileri, orkestrasyon performansını önemli ölçüde düşürebilir. İleti yüklerinin blob depolamasına yük boşaltma yükü, olası performans düşüşüne neden olabilir.
* Akış, yığınve ikili yükler desteklenmez.
* HTTP istemcisinin davranışını özelleştirme yeteneği sınırlıdır.

Bu sınırlamalardan herhangi biri kullanım durumunuzu etkileyebilecekse, bunun yerine giden HTTP aramaları yapmak için etkinlik işlevlerini ve dile özgü HTTP istemci kitaplıklarını kullanmayı düşünün.

> [!NOTE]
> Bir .NET geliştiricisiyseniz, bu özelliğin yerleşik .NET **HttpRequestMessage** ve **HttpResponseMessage** türleri yerine neden **Dayanıklı HttpRequest** ve **Dayanıklı HttpYanıt** türlerini kullandığını merak edebilirsiniz.
>
> Bu tasarım seçimi kasıtlıdır. Birincil nedeni, özel türleri kullanıcıların iç HTTP istemcisinin desteklenen davranışları hakkında yanlış varsayımlar yapmamasını sağlamak yardımcı olmasıdır. Dayanıklı Işlevlere özgü türler, API tasarımını basitleştirmeyi de mümkün kılar. Ayrıca daha kolay yönetilen kimlik [entegrasyonu](#managed-identities) ve [yoklama tüketici deseni](#http-202-handling)gibi özel özellikleri kullanılabilir yapabilirsiniz. 

### <a name="extensibility-net-only"></a>Genişletilebilirlik (.Yalnızca NET)

Düzenlemenin dahili HTTP istemcisinin davranışını özelleştirmek [Azure İşlevleri .NET bağımlılık enjeksiyonu](https://docs.microsoft.com/azure/azure-functions/functions-dotnet-dependency-injection)kullanılarak mümkündür. Bu yetenek küçük davranış değişiklikleri yapmak için yararlı olabilir. Sahte nesneler enjekte ederek HTTP istemcisini birim olarak test etmek de yararlı olabilir.

Aşağıdaki örnek, harici HTTP uç noktalarını arayan düzenleyici işlevleri için TLS/SSL sertifika doğrulamasını devre dışı etmek için bağımlılık enjeksiyonunun kullanılmasını gösterir.

```csharp
public class Startup : FunctionsStartup
{
    public override void Configure(IFunctionsHostBuilder builder)
    {
        // Register own factory
        builder.Services.AddSingleton<
            IDurableHttpMessageHandlerFactory,
            MyDurableHttpMessageHandlerFactory>();
    }
}

public class MyDurableHttpMessageHandlerFactory : IDurableHttpMessageHandlerFactory
{
    public HttpMessageHandler CreateHttpMessageHandler()
    {
        // Disable TLS/SSL certificate validation (not recommended in production!)
        return new HttpClientHandler
        {
            ServerCertificateCustomValidationCallback =
                HttpClientHandler.DangerousAcceptAnyServerCertificateValidator,
        };
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı varlıklar hakkında bilgi edinin](durable-functions-entities.md)
