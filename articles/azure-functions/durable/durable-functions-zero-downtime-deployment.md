---
title: Dayanıklı İşlevler için sıfır kesinti süresi dağıtımı
description: Sıfır kesinti süresi dağıtımı için Dayanıklı İşlevler düzenleme özelliğini nasıl etkinleştireceğinizi öğrenin.
services: functions
author: tsushi
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: azfuncdf
ms.openlocfilehash: b47604f2c8703ba587e98d68dc30552e5944f562
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73614495"
---
# <a name="zero-downtime-deployment-for-durable-functions"></a>Dayanıklı İşlevler için sıfır kesinti süresi dağıtımı

Dayanıklı İşlevler [güvenilir yürütme modeli](durable-functions-checkpointing-and-replay.md) , güncelleştirmelerin dağıtımı sırasında göz önünde bulundurulması gereken ek bir sınama oluşturan, düzenlemeler belirleyici olmasını gerektirir. Bir dağıtım, etkinlik işlevi imzalarında veya Orchestrator mantığındaki değişiklikler içerdiğinde uçuş düzenleme örnekleri başarısız olur. Bu durum özellikle uzun süre çalışan düzenleyiciler örneklerine yönelik bir sorundur ve bu da saatleri veya iş günlerini temsil edebilir.

Bu hataların oluşmasını engellemek için, çalışan tüm düzenleme örnekleri tamamlanana kadar dağıtımınızı geciktirmeli veya çalışan tüm düzenleme örneklerinin işlevlerinizin mevcut sürümlerini kullanmasını sağlamalısınız. Sürüm oluşturma hakkında daha fazla bilgi için bkz. [dayanıklı işlevler sürüm oluşturma](durable-functions-versioning.md).

> [!NOTE]
> Bu makalede, Dayanıklı İşlevler 1. x ' i hedefleyen işlevler uygulamaları için rehberlik sunulmaktadır. Henüz Dayanıklı İşlevler 2. x sürümünde tanıtılan değişiklikler için hesaba güncelleştirilmedi. Uzantı sürümleri arasındaki farklar hakkında daha fazla bilgi için [dayanıklı işlevler sürümler](durable-functions-versions.md) makalesine bakın.

Aşağıdaki grafik, Dayanıklı İşlevler için sıfır kesinti süresi dağıtımı elde etmek için üç ana stratejileri karşılaştırmaktadır: 

| Strateji |  Kullanılması gereken durumlar | Ları | Simgeler |
| -------- | ------------ | ---- | ---- |
| **[Sürümü](#versioning)** |  Sık karşılaşılan değişiklikler hakkında daha fazla karşılaşmeyen uygulamalar [.](durable-functions-versioning.md) | Basit uygulama. |  Bellekte ve işlev sayısında daha fazla işlev uygulaması boyutu.<br/>Kod çoğaltma. |
| **[Yuva ile durum denetimi](#status-check-with-slot)** | 24 veya daha fazla çakışan düzenleme için uzun süre çalışan bir düzenleme gerçekleştirmeyen bir sistem. | Basit kod tabanı.<br/>Ek işlev uygulama yönetimi gerektirmez. | Ek depolama hesabı veya görev merkezi yönetimi gerektirir.<br/>Hiçbir düzenleme çalışmadığı zaman dönem gerektirir. |
| **[Uygulama yönlendirme](#application-routing)** | 24 saatten fazla veya sık sık çakışan düzenleyicilerle düzenleme gerçekleştirenler gibi, düzenleme çalışmadığı zaman süreleri olmayan bir sistem. | Sürekli değişiklikler içeren düzenlemeleri çalıştıran sistemlerin yeni sürümlerini işler. | Akıllı uygulama yönlendiricisi gerektirir.<br/>Aboneliğiniz tarafından izin verilen işlev uygulamalarının sayısı üst sınırı (varsayılan 100). |

## <a name="versioning"></a>Sürüm oluşturma

İşlevlerinizin yeni sürümlerini tanımlayın ve işlev uygulamanızda eski sürümleri bırakın. Diyagramda görebileceğiniz gibi, bir işlevin sürümü adının bir parçası haline gelir. İşlevlerin önceki sürümleri korunduğundan, uçuş sırasında düzenleme örnekleri bunlara başvurmasına devam edebilir. Bu sırada, Orchestration Client işlevinizin bir uygulama ayarından başvurmasına yönelik yeni düzenleme örnekleri istekleri en son sürüm için çağrı yapılır.

![Sürüm oluşturma stratejisi](media/durable-functions-zero-downtime-deployment/versioning-strategy.png)

Bu stratejide, her işlev kopyalanmalıdır ve diğer işlevlere başvuruları güncelleştirildi. Bir komut dosyası yazarak daha kolay hale getirebilirsiniz. Geçiş betiği içeren [örnek bir proje](https://github.com/TsuyoshiUshio/DurableVersioning) aşağıda verilmiştir.

>[!NOTE]
>Bu strateji dağıtım sırasında kapalı kalma süresini önlemek için dağıtım yuvalarını kullanır. Yeni dağıtım yuvalarını oluşturma ve kullanma hakkında daha ayrıntılı bilgi için [Azure işlevleri dağıtım yuvaları](../functions-deployment-slots.md)' na bakın.

## <a name="status-check-with-slot"></a>Yuva ile durum denetimi

İşlev uygulamanızın geçerli sürümü üretim yuvasıyla çalışırken, işlev uygulamanızın yeni sürümünü hazırlama yuvasıyla dağıtın. Üretim ve hazırlama yuvalarınızı değiştirmeden önce, düzenleme örneklerinin çalışıp çalışmadığını denetleyin. Tüm düzenleme örnekleri tamamlandıktan sonra, değiştirmeyi yapabilirsiniz. Bu strateji, hiçbir düzenleme örneği kol olmadığında tahmin edilebilir Dönemleriniz olduğunda işe yarar. Bu, düzenleyiclarınız uzun süre çalışmadığı ve düzenleme yürütmeleri sıklıkla çakışmazsa en iyi yaklaşımdır.

### <a name="function-app-configuration"></a>İşlev uygulaması yapılandırması

Bu senaryoyu ayarlamak için aşağıdaki yordamı kullanabilirsiniz:

1. Hazırlama ve üretim için işlev uygulamanıza [dağıtım yuvaları ekleyin](../functions-deployment-slots.md#add-a-slot) .

1. Her yuva için, [AzureWebJobsStorage uygulama ayarını](../functions-app-settings.md#azurewebjobsstorage) paylaşılan bir depolama hesabının bağlantı dizesine ayarlayın. Bu işlem, Azure Işlevleri çalışma zamanı tarafından kullanılacaktır. Bu hesap, Azure Işlevleri çalışma zamanı tarafından kullanılır ve işlevin anahtarlarını yönetir.

1. Her yuva için yeni bir uygulama ayarı oluşturun (örn. DurableManagementStorage) ve değerini farklı depolama hesaplarının bağlantı dizesine ayarlayın. Bu depolama hesapları, [güvenilir yürütme](durable-functions-checkpointing-and-replay.md)için dayanıklı işlevler uzantısı tarafından kullanılacaktır. Her yuva için ayrı bir depolama hesabı kullanın. Bu ayarı bir dağıtım yuvası ayarı olarak işaretlemeyin.

1. İşlev uygulamanızın [Host. json dosyasının durableTask bölümünde](durable-functions-bindings.md#hostjson-settings), adım 3 ' te oluşturduğunuz uygulama ayarının adı olarak Azurestoraygeconnectionstringname belirtin.

Aşağıdaki diyagramda, dağıtım yuvaları ve depolama hesaplarının açıklanan yapılandırması gösterilmektedir. Bu olası dağıtım öncesi senaryoda, bir işlev uygulamasının sürüm 2 ' yi üretim yuvasında çalıştırıyor, 1. sürüm hazırlama yuvasında kalır.

![Dağıtım yuvası](media/durable-functions-zero-downtime-deployment/deployment-slot.png)

### <a name="hostjson-examples"></a>Host. JSON örnekleri

Aşağıdaki JSON parçaları, Host. JSON dosyasındaki bağlantı dizesi ayarına örnektir.

#### <a name="functions-20"></a>İşlevler 2,0

```json
{
  "version": 2.0,
  "extensions": {
    "durableTask": {
      "azureStorageConnectionStringName": "DurableManagementStorage"
    }
  }
}
```

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "DurableManagementStorage"
  }
}
```

### <a name="cicd-pipeline-configuration"></a>CI/CD işlem hattı yapılandırması

CI/CD işlem hattınızı yalnızca işlev uygulamanızda bekleyen veya çalışan düzenleme örnekleri olmadığında dağıtılacak şekilde yapılandırın. Azure Pipelines kullanırken, aşağıdaki örnekte olduğu gibi, bu koşulları denetleyen bir işlev oluşturabilirsiniz:

```csharp
[FunctionName("StatusCheck")]
public static async Task<IActionResult> StatusCheck(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    var runtimeStatus = new List<OrchestrationRuntimeStatus>();

    runtimeStatus.Add(OrchestrationRuntimeStatus.Pending);
    runtimeStatus.Add(OrchestrationRuntimeStatus.Running);

    var status = await client.GetStatusAsync(new DateTime(2015,10,10), null, runtimeStatus);
    return (ActionResult) new OkObjectResult(new Status() {HasRunning = (status.Count != 0)});
}
```

Ardından, hazırlama geçidini, hiçbir düzenleme çalışmadığı sürece bekleyecek şekilde yapılandırın. Daha fazla bilgi için bkz. [kapıları kullanarak yayın dağıtım denetimi](/azure/devops/pipelines/release/approvals/gates?view=azure-devops)

![Dağıtım kapısı](media/durable-functions-zero-downtime-deployment/deployment-gate.png)

Azure Pipelines, dağıtımınız başlamadan önce düzenleme örneklerini çalıştırmaya yönelik işlev uygulamanızı denetler.

![Dağıtım Kapısı (çalışıyor)](media/durable-functions-zero-downtime-deployment/deployment-gate-2.png)

Artık işlev uygulamanızın yeni sürümü hazırlama yuvasına dağıtılmalıdır.

![Dağıtım yuvası](media/durable-functions-zero-downtime-deployment/deployment-slot-2.png)

Son olarak, takas yuvaları. 

Dağıtım yuvası ayarları olarak işaretlenmemiş uygulama ayarları da takas edilir, bu nedenle sürüm 2 uygulaması, depolama hesabına A başvurusunu tutar. Düzenleme durumu depolama hesabında izlendiğinden, sürüm 2 uygulamasında çalışan her türlü düzenleme kesintiye uğramadan yeni yuvada çalışmaya devam eder.

![Dağıtım yuvası](media/durable-functions-zero-downtime-deployment/deployment-slot-3.png)

Her iki yuva için aynı depolama hesabını kullanmak için, görev hub 'larınızın adlarını değiştirebilirsiniz. Bu durumda, yuvalarınızın durumunu ve uygulamalarınızın HubName ayarlarını yönetmeniz gerekir. Daha fazla bilgi için bkz. [dayanıklı işlevler görev hub 'ları](durable-functions-task-hubs.md).

## <a name="application-routing"></a>Uygulama yönlendirme

Bu strateji en karmaşıktır. Ancak, çalışma düzenlemeleri arasında zaman olmayan işlev uygulamaları için kullanılabilir.

Bu strateji için Dayanıklı İşlevler önünde bir *uygulama yönlendirici* oluşturmanız gerekir. Bu yönlendirici Dayanıklı İşlevler ile uygulanabilir ve aşağıdaki sorumluluklara sahiptir:

* İşlev uygulaması dağıtılıyor.
* Dayanıklı İşlevler sürümünü yönetme. 
* Düzenleme isteklerini işlev uygulamalarına yönlendirme.

Bir düzenleme isteği alındığında, yönlendirici görevleri yapar:

1. Azure 'da yeni bir işlev uygulaması oluşturur.
2. İşlev uygulamanızın kodunu Azure 'daki yeni işlev uygulamasına dağıtır.
3. Düzenleme isteğini yeni uygulamaya iletir.

Yönlendirici, uygulama kodunuzun hangi sürümünün Azure 'daki işlev uygulamasına dağıtıldığını yönetir.

![Uygulama yönlendirme (ilk kez)](media/durable-functions-zero-downtime-deployment/application-routing.png)

Yönlendirici dağıtım ve düzenleme isteklerini istekle birlikte gönderilen `version` temel alarak uygun işlev uygulamasına yönlendirir ve düzeltme eki sürümü yok sayılıyor.

Uygulamanızın yeni bir sürümünü bir değişiklik *yapmadan* dağıttığınızda, düzeltme eki sürümünü artırabilirsiniz. Yönlendirici mevcut işlev uygulamanıza dağıtılır ve kodun eski ve yeni sürümleri için istek gönderir ve aynı işlev uygulamasına yönlendirilir.

![Uygulama yönlendirme (hiçbir bölme değişikliği yok)](media/durable-functions-zero-downtime-deployment/application-routing-2.png)

Uygulamanızın yeni bir sürümünü Son değişiklik ile dağıttığınızda, büyük veya küçük sürümü artırabilirsiniz. Ardından, uygulama yönlendiricisi Azure 'da yeni bir işlev uygulaması oluşturur, buna dağıtır ve uygulamanızın yeni sürümü için istekleri yönlendirir. Aşağıdaki diyagramda, uygulamanın 1.0.1 sürümünde bulunan düzenlemeler çalışır durumda, ancak 1.1.0 sürümü istekleri yeni işlev uygulamasına yönlendirilir.

![Uygulama yönlendirme (Son değişiklik)](media/durable-functions-zero-downtime-deployment/application-routing-3.png)

Yönlendirici 1.0.1 sürümündeki düzenlemeleri izler ve tüm düzenlemeler tamamlandıktan sonra uygulamaları kaldırır.  

### <a name="tracking-store-settings"></a>İzleme deposu ayarları

Her işlev uygulaması, muhtemelen ayrı depolama hesaplarında ayrı zamanlama kuyrukları kullanmalıdır. Ancak, uygulamanızın tüm sürümlerindeki tüm organize durumlarını sorgulamak istiyorsanız, işlev uygulamalarınız genelinde örnek ve geçmiş tabloları paylaşabilirsiniz. Tabloları, tümünün aynı değerleri kullanabilmesi için [Host. JSON ayarları](durable-functions-bindings.md#host-json) dosyasındaki `trackingStoreConnectionStringName` ve `trackingStoreNamePrefix` yapılandırarak paylaşabilirsiniz.

Daha ayrıntılı bilgi için [Azure 'daki dayanıklı işlevler örnekleri yönetin](durable-functions-instance-management.md).

![İzleme deposu ayarları](media/durable-functions-zero-downtime-deployment/tracking-store-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma Dayanıklı İşlevler](durable-functions-versioning.md)

