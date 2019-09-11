---
title: Dayanıklı İşlevler görev hub 'ları-Azure
description: Bir görev hub 'ının Azure Işlevleri için Dayanıklı İşlevler uzantısı 'nda olduğunu öğrenin. Görev hub 'larını yapılandırma hakkında bilgi edinin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: b0a58251530467d788710b0584b15715a207e20f
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70734319"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler görev hub 'ları (Azure Işlevleri)

[Dayanıklı işlevler](durable-functions-overview.md) bir *görev hub 'ı* , düzenleme için kullanılan Azure depolama kaynakları için mantıksal bir kapsayıcıdır. Orchestrator ve Activity işlevleri aynı görev merkezine ait olduklarında yalnızca birbirleriyle etkileşim kurabilir.

Birden çok işlev uygulaması bir depolama hesabını paylaşıyorsa, her işlev uygulamasının ayrı bir görev hub 'ı adıyla yapılandırılması *gerekir* . Bir depolama hesabı, birden çok görev hub 'ı içerebilir. Aşağıdaki diyagramda, paylaşılan ve ayrılmış depolama hesaplarında işlev uygulaması başına bir görev hub 'ı gösterilmektedir.

![Paylaşılan ve ayrılmış depolama hesaplarını gösteren diyagram.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure depolama kaynakları

Bir görev hub 'ı aşağıdaki depolama kaynaklarından oluşur:

* Bir veya daha fazla denetim kuyruğu.
* Bir iş öğesi kuyruğu.
* Bir geçmiş tablosu.
* Tek örnek tablosu.
* Bir veya daha fazla kira blob içeren bir depolama kapsayıcısı.

Orchestrator veya etkinlik işlevleri çalıştırıldığında veya çalıştırılmak üzere zamanlandığında, bu kaynakların tümü varsayılan Azure depolama hesabında otomatik olarak oluşturulur. [Performans ve ölçeklendirme](durable-functions-perf-and-scale.md) makalesinde bu kaynakların nasıl kullanıldığı açıklanmaktadır.

## <a name="task-hub-names"></a>Görev hub 'ı adları

Görev hub 'ları, aşağıdaki örnekte gösterildiği gibi *Host. JSON* dosyasında belirtilen bir adla tanımlanır:

### <a name="hostjson-functions-1x"></a>Host. JSON (Işlevler 1. x)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

### <a name="hostjson-functions-2x"></a>Host. JSON (Işlevler 2. x)

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

Görev hub 'ları, aşağıdaki *Host. JSON* örnek dosyasında gösterildiği gibi uygulama ayarları kullanılarak da yapılandırılabilir:

### <a name="hostjson-functions-1x"></a>Host. JSON (Işlevler 1. x)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-2x"></a>Host. JSON (Işlevler 2. x)

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

Görev hub 'ı adı, `MyTaskHub` uygulama ayarının değerine ayarlanır. Aşağıda `local.settings.json` , `MyTaskHub` ayarın nasıltanımlanacağıgösterilmektedir:`samplehubname`

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

Uygulama ayarı olarak yapılandırılmış C# bir görev hub 'ı ile çalışmak Için bir [Orchestrationclientbinding](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) kullanan bir işlevin nasıl yazılacağı hakkında önceden derlenmiş bir örnek aşağıda verilmiştir:

### <a name="c"></a>C#

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] DurableOrchestrationClientBase starter,
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

### <a name="javascript"></a>JavaScript

`function.json` Dosyadaki görev hub 'ı özelliği uygulama ayarı aracılığıyla ayarlanır:
```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

Görev hub 'ı adları bir harfle başlamalı ve yalnızca harf ve sayılardan oluşmalıdır. Belirtilmemişse, varsayılan ad **Durablefunctionshub**' dır.

> [!NOTE]
> Ad, paylaşılan bir depolama hesabında birden çok görev hub 'ı olduğunda, bir görev hub 'ını diğerinden farklılaştırır. Paylaşılan bir depolama hesabını paylaşan birden çok işlev uygulamanız varsa, *Host. JSON* dosyalarındaki her bir görev hub 'ı için farklı adlar açıkça yapılandırmanız gerekir. Aksi halde, birden çok işlev uygulaması iletiler için birbirleriyle rekabet eder ve bu da tanımsız davranışa neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturmayı nasıl ele alabileceğinizi öğrenin](durable-functions-versioning.md)
