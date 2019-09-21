---
title: Dayanıklı İşlevler-Azure Işlevlerinde HTTP özellikleri
description: Azure Işlevleri için Dayanıklı İşlevler uzantısı 'ndaki tümleşik HTTP özellikleri hakkında bilgi edinin.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 5a9143643b1a1cabb32903933dbd68d665d0424f
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71171133"
---
# <a name="http-features"></a>HTTP özellikleri

Dayanıklı İşlevler, dayanıklı düzenlemeleri ve varlıkları HTTP iş akışlarına eklemenizi kolaylaştıran çeşitli özelliklere sahiptir. Bu makale, bu özelliklerden bazılarının ayrıntılarına gider.

## <a name="exposing-http-apis"></a>HTTP API 'Lerini gösterme

Düzenlemeler ve varlıklar, HTTP istekleri kullanılarak çağrılabilir ve yönetilebilir. Dayanıklı İşlevler uzantısı yerleşik HTTP API 'Lerini kullanıma sunar ve ayrıca, HTTP ile tetiklenen işlevlerin içindeki düzenleyiciler ve varlıklarla etkileşim kurmak için API 'Ler sağlar.

### <a name="built-in-http-apis"></a>Yerleşik HTTP API 'Leri

Dayanıklı İşlevler uzantısı, Azure Işlevleri konağına otomatik olarak bir dizi HTTP API 'si ekler. Bu API 'Ler, herhangi bir kod yazmadan düzenleyiciler ve varlıklarla etkileşime girebilmeniz ve bunları yönetmenize olanak tanır.

Aşağıdaki yerleşik HTTP API 'Leri desteklenir.

* [Yeni düzenleme Başlat](durable-functions-http-api.md#start-orchestration)
* [Sorgu düzenleme örneği](durable-functions-http-api.md#get-instance-status)
* [Düzenleme örneğini Sonlandır](durable-functions-http-api.md#terminate-instance)
* [Bir Orchestration 'a dış olay gönderme](durable-functions-http-api.md#raise-event)
* [Düzenleme geçmişini temizle](durable-functions-http-api.md#purge-single-instance-history)
* [Bir varlığa işlem olayı gönderme](durable-functions-http-api.md#signal-entity)
* [Bir varlığın durumunu sorgulama](durable-functions-http-api.md#query-entity)

Dayanıklı İşlevler uzantısı tarafından kullanıma sunulan tüm yerleşik HTTP API 'Lerinin tam bir açıklaması için bkz. [http API 'leri](durable-functions-http-api.md) makalesi.

### <a name="http-api-url-discovery"></a>HTTP API URL keşfi

[Orchestration istemci bağlaması](durable-functions-bindings.md#orchestration-client) , güvenli http yanıt yükleri oluşturmak Için kullanılabilecek API 'leri kullanıma sunar. Örneğin, belirli bir düzenleme örneği için yönetim API 'Lerinin bağlantılarını içeren bir yanıt oluşturabilir. Aşağıda, bu API 'nin yeni bir Orchestration örneği için nasıl kullanılacağını gösteren örnek bir HTTP tetikleyici işlevi verilmiştir:

#### <a name="precompiled-c"></a>DerlemesiC#

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpStart.cs)]

#### <a name="c-script"></a>C#SCRIPT

[!code-csharp[Main](~/samples-durable-functions/samples/csx/HttpStart/run.csx)]

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/index.js)]

#### <a name="functionjson"></a>function. JSON

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/HttpStart/function.json)]

Yukarıda gösterilen HTTP tetikleyici işlevlerini kullanarak bir Orchestrator işlevinin başlatılması, herhangi bir HTTP istemcisi kullanılarak yapılabilir. Aşağıdaki kıvrımlı komutu adlı `DoWork`bir Orchestrator işlevi başlatır.

```bash
curl -X POST https://localhost:7071/orchestrators/DoWork -H "Content-Length: 0" -i
```

Aşağıda kimliği `abc123` olarak bir düzenleme için örnek yanıt verilmiştir (açıklık için bazı ayrıntılar kaldırılmıştır):

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

Önceki örnekteki alanların her biri, yerleşik HTTP API 'lerine karşılık gelir. `~Uri` Bu API 'Ler hedef Orchestration örneğini yönetmek için kullanılabilir.

> [!NOTE]
> Web kancası URL 'Lerinin biçimi, çalıştırdığınız Azure Işlevleri ana bilgisayarı sürümüne bağlı olarak farklılık gösterebilir. Yukarıdaki örnek, Azure Işlevleri 2,0 konağına yöneliktir.

Tüm yerleşik HTTP API 'Lerinin açıklaması için bkz. [http API başvurusu](durable-functions-http-api.md) belgeleri.

### <a name="async-operation-tracking"></a>Zaman uyumsuz işlem izleme

Daha önce bahsedilen HTTP yanıtı, Dayanıklı İşlevler uzun süre çalışan HTTP zaman uyumsuz API 'Leri uygulamaya yardımcı olacak şekilde tasarlanmıştır. Bu model bazen *yoklama tüketici stili*olarak adlandırılır. İstemci/sunucu akışı aşağıdaki gibi çalışmaktadır:

1. İstemci, Orchestrator işlevi gibi uzun süre çalışan bir işlemi başlatmak için bir HTTP isteği yayınlar.
2. Hedef http tetikleyicisi, bir `Location` üst `statusQueryGetUri` bilgiyle değeri olan bir http 202 yanıtı döndürür.
3. İstemci `Location` başlıktaki URL 'yi yoklar. Bir `Location` üst bilgiyle http 202 yanıtlarını görmeyi sürdürür.
4. Örnek tamamlandığında (veya başarısız olursa), `Location` üstbilgideki uç nokta http 200 döndürür.

Bu protokol, `Location` bir HTTP uç noktası yoklamayı ve üstbilgiyi takip eden dış istemcilerle veya hizmetlerle uzun süreli işlemlerin koordine etmesine olanak tanır. Bu düzenin istemci ve sunucu uygulamaları Dayanıklı İşlevler HTTP API 'Lerinde yerleşik olarak bulunur.

> [!NOTE]
> Varsayılan olarak, [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) tarafından sunulan tüm HTTP tabanlı eylemler standart zaman uyumsuz işlem modelini destekler. Bu özellik, uzun süreli dayanıklı bir işlevi Logic Apps bir iş akışının parçası olarak katıştırmayı mümkün kılar. Zaman uyumsuz HTTP desenlerine yönelik Logic Apps desteği hakkında daha fazla ayrıntı [Azure Logic Apps iş akışı eylemleri ve Tetikleyiciler belgelerinde](../../logic-apps/logic-apps-workflow-actions-triggers.md)bulunabilir.

> [!NOTE]
> Düzenleyiciyle etkileşim kurmak yalnızca HTTP tarafından tetiklenen işlevlerden değil herhangi bir işlev türünden yapılabilir.

İstemci API 'Leri kullanarak düzenleme ve varlıkların nasıl yönetileceği hakkında daha fazla bilgi için bkz. [örnek yönetimi](durable-functions-instance-management.md) makalesi.

## <a name="consuming-http-apis"></a>HTTP API 'Lerini kullanma

Orchestrator işlevlerinin, [Orchestrator işlev kodu kısıtlamalarında](durable-functions-code-constraints.md)açıklandığı gibi doğrudan g/ç yapmasına izin verilmez. Bunun yerine, Orchestrator işlevleri genellikle g/ç işlemleri gerçekleştiren [etkinlik işlevlerini](durable-functions-types-features-overview.md#activity-functions) çağırır.

Dayanıklı İşlevler 2,0 ' den başlayarak, düzenlemeler [Orchestration tetikleyicisi bağlamasını](durable-functions-bindings.md#orchestration-trigger)kullanarak HTTP API 'lerini yerel olarak kullanabilir.

> [!NOTE]
> HTTP uç noktalarını doğrudan Orchestrator işlevlerinden çağırma özelliği JavaScript 'te henüz kullanılamıyor.

Aşağıdaki örnek kodda, C# `CallHttpAsync` .NET API kullanarak giden http isteği oluşturan bir Orchestrator işlevi gösterilmektedir:

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

"HTTP çağrısı" eylemi, Orchestrator işlevlerinizin aşağıdaki işlemleri yapmanızı sağlar:

* HTTP API 'Lerini doğrudan düzenleme işlevlerinden (daha sonra bahsedilen bazı sınırlamalarla) çağırın.
* Otomatik destek istemci tarafı HTTP 202 durum yoklama desenleri.
* Diğer Azure uç noktalarına yetkili HTTP çağrıları yapmak için [Azure yönetilen kimliklerini](../../active-directory/managed-identities-azure-resources/overview.md) kullanın.

HTTP API 'Lerini doğrudan Orchestrator işlevlerinden kullanma özelliği, belirli bir yaygın senaryolar kümesi için kolaylık olarak tasarlanmıştır. Bu özellikleri, etkinlik işlevlerini kullanarak kendiniz uygulamanız mümkündür. Birçok durumda, etkinlik işlevleri size daha fazla esneklik verebilir.

### <a name="http-202-handling"></a>HTTP 202 işleme

"HTTP çağrısı" API 'SI, *yoklama tüketici deseninin*istemci tarafını otomatik olarak uygulayabilir. Çağrılan bir API, `Location` üst bilgiyle bir http 202 yanıtı döndürürse, bir 202 olmayan yanıt geri dönene kadar Orchestrator işlevi `Location` kaynağı otomatik olarak yoklayacaktır. 202 olmayan yanıt, Orchestrator işlev koduna döndürülen yanıt olacaktır.

> [!NOTE]
> Orchestrator işlevleri, [zaman uyumsuz işlem izleme](#async-operation-tracking)bölümünde açıklandığı gibi, sunucu tarafı *yoklamayı tüketici modelini*de yerel olarak destekler. Bu, bir işlev uygulamasındaki düzenlemeler diğer işlev uygulamalarındaki Orchestrator işlevlerini kolayca koordine edebileceğiniz anlamına gelir. Bu, [alt düzenleme](durable-functions-sub-orchestrations.md) kavramıyla benzerdir, ancak uygulamalar arası iletişime yönelik destek sağlar. Bu, özellikle mikro hizmet stili uygulama geliştirmesi için yararlıdır.

### <a name="managed-identities"></a>Yönetilen kimlikler

Dayanıklı İşlevler yerel olarak yetkilendirme için Azure AD belirteçlerini kabul eden API 'Leri çağırmayı destekler. Bu destek, bu belirteçleri almak için [Azure yönetilen kimliklerini](../../active-directory/managed-identities-azure-resources/overview.md) kullanır.

Aşağıdaki kod, Azure Resource Manager [sanal makinelerini REST API](https://docs.microsoft.com/rest/api/compute/virtualmachines)kullanarak bir sanal makineyi yeniden başlatmak için kimliği doğrulanmış çağrılar yapan .net Orchestrator işlevine bir örnektir.

```csharp
[FunctionName("RestartVm")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    string subscriptionId = "mySubId";
    string resourceGroup = "myRG";
    string vmName = "myVM";
    
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

Önceki örnekte, `tokenSource` parametresi [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) için Azure AD belirteçlerini ("Kaynak URI 'si" `https://management.core.windows.net`tarafından tanımlanır) almak üzere yapılandırılmıştır. Örnek, geçerli işlev uygulamasının yerel olarak çalıştığını ya da yönetilen bir kimliğe sahip bir Azure Işlevleri uygulaması olarak dağıtıldığını varsayar. Yerel kimliğin veya yönetilen kimliğin belirtilen kaynak grubundaki `myRG`VM 'leri yönetme izni olduğu varsayılır.

Çalışma zamanında, yapılandırılmış belirteç kaynağı otomatik olarak bir OAuth 2,0 erişim belirteci döndürür ve bunu giden isteğin `Authorization` üstbilgisine bir taşıyıcı belirteci olarak ekler. Bu model, HTTP isteklerine yetkilendirme üst bilgilerinin el ile eklenmesine yönelik bir iyileştirmedir çünkü:

* Belirteç yenileme otomatik olarak işlenir. Süre dolmadan önce endişelenmeniz gerekmez.
* Belirteçler hiçbir şekilde dayanıklı düzenleme durumunda depolanmaz.
* Belirteç alımı ile başa çıkmak için herhangi bir kod yazmanız gerekmez.

[Önceden derlenmiş C# "RestartVMs" örneklerinde](https://github.com/Azure/azure-functions-durable-extension/blob/v2/samples/v2/precompiled/RestartVMs.cs)daha kapsamlı bir örnek bulunabilir.

Yönetilen kimlikler Azure Kaynak yönetimi ile sınırlı değildir. Yönetilen kimlikler, birinci taraf Azure hizmetleri veya üçüncü taraf web uygulamaları dahil olmak üzere Azure AD taşıyıcı belirteçlerini kabul eden herhangi bir API 'ye erişmek için kullanılabilir. Üçüncü taraf Web uygulaması başka bir işlev uygulaması bile olabilir. Azure AD ile kimlik doğrulamasını destekleyen birinci taraf Azure hizmetlerinin bir listesi için bkz. Azure [ad kimlik doğrulamasını destekleyen Azure hizmetleri](../../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

### <a name="limitations"></a>Sınırlamalar

HTTP API 'Lerini çağırmaya yönelik yerleşik destek, kullanışlı bir özelliktir ve tüm senaryolar için uygun değildir. Orchestrator işlevleri tarafından gönderilen HTTP istekleri ve yanıtları, kuyruk iletileri olarak serileştirilir ve kalıcıdır. Bu sıraya alma davranışı, [Orchestration Replay IÇIN http çağrılarının güvenilir ve güvenli](durable-functions-orchestrations.md#reliability)olmasını sağlar. Ancak, bu sıraya alma davranışı şu şekilde olabilir:

* Her HTTP isteği, yerel bir HTTP istemcisiyle karşılaştırıldığında ek gecikme süresi içerir.
* Bir kuyruk iletisine sığamayacak olan büyük istek veya Yanıt iletileri, düzenleme performansını önemli ölçüde düşürebilir. Olası performans düşüşü, ileti yüklerini blob depolamaya boşaltma yükünün olmamasıdır.
* Akış, öbekli ve ikili yükleri desteklenmez.
* HTTP istemcisinin davranışını özelleştirme yeteneği sınırlıdır.

Bu sınırlamaların herhangi biri kullanım durumunu etkileyebileceğinden, giden HTTP çağrıları yapmak için etkinlik işlevlerini ve dile özgü HTTP istemci kitaplıklarını kullanmayı göz önünde bulundurun.

> [!NOTE]
> .Net geliştiricisiyseniz, bu özelliğin `DurableHttpRequest` neden yerleşik .net `HttpRequestMessage` ve `DurableHttpResponse` `HttpResponseMessage`türlerini kullandığını merak ediyor olabilirsiniz. Bu tasarım seçeneği bilerek yapıldı. Birincil neden, özel türlerin, kullanıcıların iç HTTP istemcisinin desteklenen davranışları hakkında yanlış varsayımlar olmamasını sağlamaya yardımcı olur. Dayanıklı özel türler Ayrıca API tasarımını basitleştirecek ve [yönetilen kimlik tümleştirmesi](#managed-identities) ve [yoklama tüketicisi](#http-202-handling)gibi özel özellikleri daha kolay bir şekilde kolaylaştırabilmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı varlıklar hakkında bilgi edinin](durable-functions-entities.md)
