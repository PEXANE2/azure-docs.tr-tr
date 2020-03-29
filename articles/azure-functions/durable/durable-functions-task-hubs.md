---
title: Dayanıklı İşlevlerde Görev Hub'ları - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısında görev hub'ınne ne olduğunu öğrenin. Görev hub'larını nasıl yapılandırıştırılamayı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: azfuncdf
ms.openlocfilehash: ffb3d590aebe80994de1e7e834a2eba5777df9a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76262495"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler 'de görev hub'ları (Azure İşlevleri)

[Dayanıklı Işlevler'deki](durable-functions-overview.md) *görev hub'ı,* düzenleme için kullanılan Azure Depolama kaynakları için mantıksal bir kapsayıcıdır. Orkestratör ve etkinlik işlevleri yalnızca aynı görev hub'ına ait olduklarında birbirleriyle etkileşimkurabilir.

Birden çok işlevli uygulama bir depolama hesabı paylaşıyorsa, her işlev *uygulaması* ayrı bir görev hub adı ile yapılandırılmalıdır. Depolama hesabı birden çok görev hub'ı içerebilir. Aşağıdaki diyagram, paylaşılan ve özel depolama hesaplarında işlev başına bir görev merkezini gösterir.

![Paylaşılan ve özel depolama hesaplarını gösteren diyagram.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Depolama kaynakları

Görev hub'ı aşağıdaki depolama kaynaklarından oluşur:

* Bir veya daha fazla denetim sırası.
* Bir iş öğesi sırası.
* Bir tarih masası.
* Bir örnek tablo.
* Bir veya daha fazla kira lekeleri içeren bir depolama konteyneri.
* Varsa, büyük ileti yüklerini içeren bir depolama kapsayıcısı.

Bu kaynakların tümü, düzenek, varlık veya etkinlik işlevleri çalıştırıldığında veya çalıştırılacak zamanlandığında varsayılan Azure Depolama hesabında otomatik olarak oluşturulur. [Performans ve Ölçek](durable-functions-perf-and-scale.md) makalesi, bu kaynakların nasıl kullanıldığını açıklar.

## <a name="task-hub-names"></a>Görev merkezi adları

Görev hub'ları, aşağıdaki örnekte gösterildiği *gibi, ana bilgisayar.json* dosyasında bildirilen bir adla tanımlanır:

### <a name="hostjson-functions-20"></a>host.json (Fonksiyonlar 2.0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

### <a name="hostjson-functions-1x"></a>host.json (Fonksiyonlar 1.x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

Görev hub'ları, aşağıdaki `host.json` örnek dosyada gösterildiği gibi uygulama ayarları kullanılarak da yapılandırılabilir:

### <a name="hostjson-functions-10"></a>host.json (Fonksiyonlar 1.0)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-20"></a>host.json (Fonksiyonlar 2.0)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

Görev merkezi `MyTaskHub` adı, uygulama ayarı değerine ayarlanır. Aşağıdaki `local.settings.json` `MyTaskHub` ayarı nasıl tanımlayın: `samplehubname`

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

Aşağıdaki kod, Uygulama Ayarı olarak yapılandırılan bir görev hub'ı ile çalışmak için [düzenleme istemcisi bağlamakullanan](durable-functions-bindings.md#orchestration-client) bir işlevin nasıl yazıldırılabildiğini gösterir:

# <a name="c"></a>[C #](#tab/csharp)

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] IDurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

> [!NOTE]
> Önceki C# örneği Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

# <a name="javascript"></a>[Javascript](#tab/javascript)

Dosyadaki `function.json` görev merkezi özelliği Uygulama Ayarı ile ayarlanır:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

---

Görev hub adları bir harfle başlamalı ve yalnızca harf ve sayılardan oluşmalıdır. Belirtilmemişse, aşağıdaki tabloda gösterildiği gibi varsayılan görev merkezi adı kullanılır:

| Dayanıklı uzatma sürümü | Varsayılan görev hub adı |
| - | - |
| 2.x | Azure'da dağıtıldığında, görev merkezi adı _işlev uygulamasının_adından türetilmiştir. Azure dışında çalışırken varsayılan görev merkezi `TestHubName`adı . |
| 1.x | Tüm ortamlar için varsayılan `DurableFunctionsHub`görev hub adı. |

Uzantı sürümleri arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

> [!NOTE]
> Ortak bir depolama hesabında birden çok görev hub'ı olduğunda, bir görev merkezini diğerinden ayıran addır. Paylaşılan bir depolama hesabını paylaşan birden çok işlevli uygulamanız varsa, *ana bilgisayar.json* dosyalarındaki her görev merkezi için açıkça farklı adlar yapılandırmanız gerekir. Aksi takdirde, birden çok işlevli uygulama iletiler için birbirleriyle rekabet eder ve bu da, orkestrasyonların beklenmedik bir şekilde veya `Pending` `Running` durumda "sıkışması" da dahil olmak üzere tanımlanmamış davranışlara neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Orkestrasyon sürümüyle nasıl başa çıkacağınızı öğrenin](durable-functions-versioning.md)
