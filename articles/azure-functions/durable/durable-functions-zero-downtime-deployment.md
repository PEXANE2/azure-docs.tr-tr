---
title: Dayanıklı Işlevler için sıfır kapatma süresi dağıtımı
description: Sıfır kapalı kalma süresi dağıtımları için Dayanıklı Fonksiyonlar düzenlemenizi nasıl etkinleştirin öğrenin.
author: tsushi
ms.topic: conceptual
ms.date: 10/10/2019
ms.author: azfuncdf
ms.openlocfilehash: 8e12d58c0077084c181d111b0b017665b74b9157
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74231267"
---
# <a name="zero-downtime-deployment-for-durable-functions"></a>Dayanıklı Işlevler için sıfır kapatma süresi dağıtımı

Dayanıklı [İşlevler'in güvenilir yürütme modeli,](durable-functions-checkpointing-and-replay.md) orkestrasyonların deterministic olmasını gerektirir ve bu da güncelleştirmeleri dağıtırken göz önünde bulundurulması gereken ek bir sorun oluşturur. Bir dağıtım etkinlik işlevi imzalarında veya orkestratör mantığında değişiklikler içeriyorsa, uçuş içi orkestrasyon örnekleri başarısız olur. Bu durum, özellikle çalışma saatlerini veya günlerini temsil edebilecek uzun süreli orkestrasyon örnekleri için bir sorundur.

Bu hataların oluşmasını önlemek için iki seçeneğiniz vardır: 
- Tüm çalışan orkestrasyon örnekleri tamamlanana kadar dağıtımınızı geciktirin.
- Çalışan orkestrasyon örneklerinin işlevlerinizin varolan sürümlerini kullandığından emin olun. 

> [!NOTE]
> Bu makalede, Dayanıklı Fonksiyonlar 1.x hedefleyen işlevler uygulamaları için kılavuz sağlar. Dayanıklı Fonksiyonlar 2.x'te tanıtılan değişiklikleri hesaba katmak için güncelleştirilmemiştir. Uzantı sürümleri arasındaki farklar hakkında daha fazla bilgi için [Bkz. Dayanıklı Fonksiyonlar sürümleri.](durable-functions-versions.md)

Aşağıdaki grafik, Dayanıklı İşlevler için sıfır kesintili dağıtım elde etmek için üç ana stratejiyi karşılaştırır: 

| Strateji |  Kullanılması gereken durumlar | Artıları | Simgeler |
| -------- | ------------ | ---- | ---- |
| [Sürüm Oluşturma](#versioning) |  Sık sık kırılma yla karşılaşan [uygulamalar.](durable-functions-versioning.md) | Uygulaması kolay. |  Bellekte artırılmış işlev uygulaması boyutu ve işlev sayısı.<br/>Kod çoğaltma. |
| [Yuva yla durum denetimi](#status-check-with-slot) | 24 saatten uzun süren veya sık sık üst üste gelen orkestrasyonlara sahip olmayan bir sistem. | Basit kod tabanı.<br/>Ek işlev uygulaması yönetimi gerektirmez. | Ek depolama hesabı veya görev merkezi yönetimi gerektirir.<br/>Hiçbir orkestrasyon çalışan zaman süreleri gerektirir. |
| [Uygulama yönlendirme](#application-routing) | 24 saatten fazla süren orkestrasyonlu veya sık sık örtüşen orkestrasyonların olduğu dönemler gibi, orkestrasyonlarçalışmadığı dönemlere sahip olmayan bir sistem. | Sürekli çalışan ve çığır açan değişikliklere sahip olan sistemlerin yeni sürümlerini işler. | Akıllı bir uygulama yönlendiricisi gerektirir.<br/>Aboneliğinizin izin verdiği işlev uygulamalarının sayısını en üst düzeye çıkarabilir. Varsayılan değer 100'dür. |

## <a name="versioning"></a>Sürüm Oluşturma

İşlevlerinizin yeni sürümlerini tanımlayın ve işlev uygulamanızda eski sürümleri bırakın. Diyagramda görebileceğiniz gibi, bir işlevin sürümü adının bir parçası olur. Fonksiyonların önceki sürümleri korunduğundan, uçuş içi orkestrasyon örnekleri bunlara başvurmaya devam edebilir. Bu arada, yeni düzenleme örnekleri için istekler, orkestrasyon istemci işlevinizin bir uygulama ayarından başvurulabileceği en son sürümü çağırır.

![Sürüm stratejisi](media/durable-functions-zero-downtime-deployment/versioning-strategy.png)

Bu stratejide, her işlev kopyalanmalıdır ve diğer işlevlere başvuruları güncelleştirilmelidir. Bir komut dosyası yazarak daha kolay yapabilirsiniz. Burada geçiş komut dosyası içeren örnek bir [proje](https://github.com/TsuyoshiUshio/DurableVersioning) vereyim.

>[!NOTE]
>Bu strateji, dağıtım sırasında kapalı kalma süresini önlemek için dağıtım yuvalarını kullanır. Yeni dağıtım yuvalarının nasıl oluşturulup kullanılacağı hakkında daha ayrıntılı bilgi için [Azure İşlevleri dağıtım yuvalarına](../functions-deployment-slots.md)bakın.

## <a name="status-check-with-slot"></a>Yuva yla durum denetimi

İşlev uygulamanızın geçerli sürümü üretim yuvanızda çalışırken, işlev uygulamanızın yeni sürümünü evreleme yuvanıza dağıtın. Üretim ve evreleme yuvalarınızı değiştirmeden önce, çalışan orkestrasyon örnekleri olup olmadığını kontrol edin. Tüm orkestrasyon örnekleri tamamlandıktan sonra, takas yapabilirsiniz. Bu strateji, hiçbir orkestrasyon örneğinin uçuşta olmadığı öngörülebilir dönemler olduğunda çalışır. Bu, orkestrasyonlarInizin uzun süre çalışmadığında ve orkestrasyon yürütmeleriniz sık sık çakışmıyorsa en iyi yaklaşımdır.

### <a name="function-app-configuration"></a>Fonksiyon uygulaması yapılandırması

Bu senaryoyu ayarlamak için aşağıdaki yordamı kullanın.

1. Hazırlama ve üretim için işlev uygulamanıza [dağıtım yuvaları ekleyin.](../functions-deployment-slots.md#add-a-slot)

1. Her yuva için [AzureWebJobsStorage uygulama ayarını](../functions-app-settings.md#azurewebjobsstorage) paylaşılan bir depolama hesabının bağlantı dizesine ayarlayın. Bu depolama hesabı bağlantı dizesi Azure İşlevleri çalışma zamanı tarafından kullanılır. Bu hesap Azure İşlevleri çalışma zamanı tarafından kullanılır ve işlevin anahtarlarını yönetir.

1. Her yuva için, örneğin yeni bir uygulama `DurableManagementStorage`ayarı oluşturun. Değerini farklı depolama hesaplarının bağlantı dizesine ayarlayın. Bu depolama [hesapları, güvenilir yürütme](durable-functions-checkpointing-and-replay.md)için Dayanıklı İşlevler uzantısı tarafından kullanılır. Her yuva için ayrı bir depolama hesabı kullanın. Bu ayarı dağıtım yuvası ayarı olarak işaretlemeyin.

1. İşlev uygulamanızın [host.json dosyasının dayanıklıGörev bölümünde,](durable-functions-bindings.md#hostjson-settings)adım 3'te oluşturduğunuz uygulama ayarının adı olarak belirtin. `azureStorageConnectionStringName`

Aşağıdaki diyagram, dağıtım yuvalarının ve depolama hesaplarının açıklanan yapılandırmasını gösterir. Bu olası dağıtım öncesi senaryoda, bir işlev uygulamasının sürüm 2'si üretim yuvasında çalışırken, sürüm 1 evreleme yuvasında kalır.

![Dağıtım yuvaları ve depolama hesapları](media/durable-functions-zero-downtime-deployment/deployment-slot.png)

### <a name="hostjson-examples"></a>host.json örnekleri

Aşağıdaki JSON *parçaları, anabilgisayar.json* dosyasındaki bağlantı dizeasının örnekleridir.

#### <a name="functions-20"></a>Fonksiyonlar 2.0

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

### <a name="cicd-pipeline-configuration"></a>CI/CD boru hattı yapılandırması

CI/CD ardışık sisteminizi yalnızca işlev uygulamanızın bekleyen veya çalışan orkestrasyon örnekleri olmadığında dağıtacak şekilde yapılandırın. Azure Denetim Hatları'nı kullanırken, aşağıdaki örnekte olduğu gibi bu koşulları denetleyen bir işlev oluşturabilirsiniz:

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

Ardından, sahneleme kapısını hiçbir düzenleme çalışmayana kadar bekleyecek şekilde yapılandırın. Daha fazla bilgi için [bkz: Gates'i kullanarak dağıtım denetimi](/azure/devops/pipelines/release/approvals/gates?view=azure-devops)

![Dağıtım geçidi](media/durable-functions-zero-downtime-deployment/deployment-gate.png)

Azure Pipelines, dağıtım Başlamadan önce düzenleme örneklerini çalıştırmak için işlev uygulamanızı denetler.

![Dağıtım kapısı (çalışıyor)](media/durable-functions-zero-downtime-deployment/deployment-gate-2.png)

Artık işlev uygulamanızın yeni sürümü hazırlama yuvasına dağıtılmalıdır.

![Evreleme yuvası](media/durable-functions-zero-downtime-deployment/deployment-slot-2.png)

Son olarak, yuvaları değiştirin. 

Dağıtım yuvası ayarları olarak işaretlenmemiş uygulama ayarları da değiştirilmiştir, bu nedenle sürüm 2 uygulaması depolama hesabı A'ya göndermetutar. Düzenleme durumu depolama hesabında izlenir olduğundan, sürüm 2 uygulamasında çalışan tüm orkestrasyonlar yeni yuvada kesintisiz olarak çalışmaya devam eder.

![Dağıtım yuvası](media/durable-functions-zero-downtime-deployment/deployment-slot-3.png)

Her iki yuva için de aynı depolama hesabını kullanmak için görev hub'larınızın adlarını değiştirebilirsiniz. Bu durumda, yuvalarınızın durumunu ve uygulamanızın HubName ayarlarını yönetmeniz gerekir. Daha fazla bilgi edinmek [için, Dayanıklı Işlevler'deki Görev hub'larına](durable-functions-task-hubs.md)bakın.

## <a name="application-routing"></a>Uygulama yönlendirme

Bu strateji en karmaşık olanı. Ancak, çalışan orkestrasyonlar arasında zaman yok işlev uygulamaları için kullanılabilir.

Bu strateji için, Dayanıklı İşlevlerinizin önünde bir *uygulama yönlendiricisi* oluşturmanız gerekir. Bu yönlendirici Dayanıklı Fonksiyonlar ile uygulanabilir. Yönlendiricinin sorumluluğu:

* İşlev uygulamasını dağıtın.
* Dayanıklı Fonksiyonlar sürümünü yönetin. 
* Düzenleme isteklerini işlev uygulamalarına yönlendirin.

Bir düzenleme isteği ilk kez alındığında, yönlendirici aşağıdaki görevleri yapar:

1. Azure'da yeni bir işlev uygulaması oluşturur.
2. İşlev uygulamanızın kodunu Azure'daki yeni işlev uygulamasına dağıtir.
3. Orkestrasyon isteğini yeni uygulamaya iletin.

Yönlendirici, uygulamanızın kodunun hangi sürümünün Azure'daki hangi işlev uygulamasına dağıtıldığı durumunu yönetir.

![Uygulama yönlendirme (ilk kez)](media/durable-functions-zero-downtime-deployment/application-routing.png)

Yönlendirici, istekle birlikte gönderilen sürüme göre dağıtım ve düzenleme isteklerini uygun işlev uygulamasına yönlendirir. Yama sürümünü yok sayar.

Uygulamanızın yeni bir sürümünü bozuk bir değişiklik olmadan dağıttığınızda, yama sürümünü artımlaşabilirsiniz. Yönlendirici, mevcut işlev uygulamanıza dağılar ve kodun aynı işlev uygulamasına yönlendirilen eski ve yeni sürümleri için istekler gönderir.

![Uygulama yönlendirmesi (kırılma yok)](media/durable-functions-zero-downtime-deployment/application-routing-2.png)

Uygulamanızın yeni bir sürümünü bozuk bir değişiklikle dağıttığınızda, ana veya küçük sürümü niçin artımlaşabilirsiniz. Ardından uygulama yönlendiricisi Azure'da yeni bir işlev uygulaması oluşturur, uygulamanın yeni sürümüne yönelik istekleri ona yönlendirir. Aşağıdaki diyagramda, uygulamanın 1.0.1 sürümünde çalışan orkestrasyonlar çalışmaya devam eder, ancak 1.1.0 sürümü için istekler yeni işlev uygulamasına yönlendirilir.

![Uygulama yönlendirme (kesme değişikliği)](media/durable-functions-zero-downtime-deployment/application-routing-3.png)

Yönlendirici, 1.0.1 sürümündeki orkestrasyonlarin durumunu izler ve tüm orkestrasyonlar tamamlandıktan sonra uygulamaları kaldırır. 

### <a name="tracking-store-settings"></a>Mağaza ayarlarını izleme

Her işlev uygulaması, büyük olasılıkla ayrı depolama hesaplarında ayrı zamanlama kuyrukları kullanmalıdır. Uygulamanızın tüm sürümlerinde tüm düzenleme örneklerini sorgulamak istiyorsanız, örnek ve geçmiş tablolarını işlev uygulamalarınızda paylaşabilirsiniz. [Ana bilgisayar.json ayarları](durable-functions-bindings.md#host-json) `trackingStoreNamePrefix` dosyasındaki `trackingStoreConnectionStringName` ve ayarları yapılandırırak tabloları paylaşabilirsiniz, böylece hepsi aynı değerleri kullanır.

Daha fazla bilgi için [Azure'da Dayanıklı İşlevler'deki örnekleri yönet'e](durable-functions-instance-management.md)bakın.

![Mağaza ayarlarını izleme](media/durable-functions-zero-downtime-deployment/tracking-store-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı FonksiyonlarıN Sürümü](durable-functions-versioning.md)

