---
title: Azure İşlevler için En İyi Uygulamalar
description: Azure İşlevleri için en iyi uygulamaları ve desenleri öğrenin.
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.topic: conceptual
ms.date: 12/17/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a41a5828a82d81c5e7e8749fee70cd15e17bb9d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277784"
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Azure İşlevleri’nin performansını ve güvenilirliğini iyileştirme

Bu makalede, [sunucusuz](https://azure.microsoft.com/solutions/serverless/) işlev uygulamalarınızın performansını ve güvenilirliğini artırmak için kılavuz lar sağlanmaktadır.  

## <a name="general-best-practices"></a>Genel en iyi uygulamalar

Azure İşlevlerini kullanarak sunucusuz çözümlerinizi nasıl oluşturduğunuza ve mimarnız olduğunuza ilgili en iyi uygulamalar aşağıda verilmiştir.

### <a name="avoid-long-running-functions"></a>Uzun çalışan işlevlerden kaçının

Büyük, uzun süren işlevler beklenmeyen zaman ayarı sorunlarına neden olabilir. Belirli bir barındırma planının zaman zaman ları hakkında daha fazla bilgi edinmek için [işlev uygulaması zaman anına](functions-scale.md#timeout)bakın. 

Birçok Düğüm.js bağımlılığı nedeniyle bir işlev büyük olabilir. Bağımlılıkların içe aktarıldaşolması da beklenmeyen zaman adaklarına neden olan daha fazla yükleme sürelerine neden olabilir. Bağımlılıklar hem açık hem de örtülü olarak yüklenir. Kodunuz tarafından yüklenen tek bir modül kendi ek modüllerini yükleyebilir. 

Mümkün olduğunda, büyük işlevleri birlikte çalışan ve yanıtları hızlı bir şekilde döndüren daha küçük işlev kümelerine dönüştürün. Örneğin, bir webhook veya HTTP tetikleyici işlevi belirli bir zaman sınırı içinde bir bildirim yanıtı gerektirebilir; webhooks için hemen bir yanıt gerektiren yaygındır. Bir kuyruk tetikleyici işlevi tarafından işlenecek bir kuyruğa HTTP tetikleyici yükü geçirebilirsiniz. Bu yaklaşım, gerçek çalışmayı ertelemenize ve anında yanıt döndürmenize olanak tanır.


### <a name="cross-function-communication"></a>Çapraz fonksiyon iletişimi

[Dayanıklı İşlevler](durable/durable-functions-overview.md) ve [Azure Mantıksal Uygulamaları,](../logic-apps/logic-apps-overview.md) durum geçişlerini ve birden çok işlev arasındaki iletişimi yönetmek için oluşturulmuştur.

Birden çok işlevle tümleştirmek için Dayanıklı İşlevler veya Mantık Uygulamaları kullanmıyorsanız, işlevler arası iletişim için depolama kuyruklarını kullanmak en iyisidir. Temel nedeni depolama kuyrukları daha ucuz ve diğer depolama seçenekleri daha sağlanması çok daha kolay olmasıdır. 

Depolama kuyruğundaki tek tek iletilerin boyutu 64 KB ile sınırlıdır. Işlevler arasında daha büyük iletiler iletmeniz gerekiyorsa, Standart katmanda 256 KB'ye ve Premium katmanda 1 MB'a kadar ileti boyutlarını desteklemek için bir Azure Hizmet Veri Servisi kuyruğu kullanılabilir.

İşlemeden önce ileti filtrelemesi gerekiyorsa Servis Veri Servisi konuları yararlıdır.

Olay hub'ları yüksek hacimli iletişimi desteklemek için yararlıdır.


### <a name="write-functions-to-be-stateless"></a>Stateless olmak için işlevleri yazma 

Fonksiyonlar mümkünse devletsiz ve idempotent olmalıdır. Gerekli durum bilgilerini verilerinizle ilişkilendirin. Örneğin, işlenen bir siparişin ilişkili `state` bir üyesi olabilir. İşlevin kendisi devletsiz kalırken, bir işlev bu duruma dayalı bir siparişi işleyebilir. 

Idempotent fonksiyonlar özellikle zamanlayıcı tetikleyicileri ile önerilir. Örneğin, kesinlikle günde bir kez çalışması gereken bir şey varsa, aynı sonuçlarla gün içinde herhangi bir zamanda çalıştırabilirsiniz, böylece yazın. Belirli bir gün için iş olmadığında işlev çıkabilir. Ayrıca, önceki bir çalıştırma tamamlayamazsa, bir sonraki çalıştırma kaldığı yerden devam etmelidir.


### <a name="write-defensive-functions"></a>Savunma işlevleri yazma

İşlevinizin herhangi bir zamanda bir özel durumla karşılaşabileceğini varsayalım. Bir sonraki yürütme sırasında önceki bir başarısız noktadan devam etme yeteneği ile işlevlerinizi tasarla. Aşağıdaki eylemleri gerektiren bir senaryo düşünün:

1. Veritabanında 10.000 satır için sorgu.
2. Satırın daha aşağısına işlemek için bu satırların her biri için bir sıra iletisi oluşturun.
 
Sisteminizin ne kadar karmaşık olduğuna bağlı olarak, aşağıdaki gibi kötü davranan akış aşağı hizmetleri, ağ kesintileri veya ulaşılan kota sınırları vb. olabilir. Tüm bunlar herhangi bir zamanda işlevinizi etkileyebilir. Buna hazırlıklı olmak için işlevlerinizi tasarlamanız gerekir.

Bu öğelerden 5.000'ini işleme için sıraya ekledikten sonra bir hata oluşursa kodunuz nasıl tepki verir? Tamamladığınız bir kümedeki öğeleri izleyin. Aksi takdirde, bir dahaki sefere tekrar ekleyebilirsiniz. Bu çift ekleme iş akışı üzerinde ciddi bir etkisi olabilir, bu yüzden [işlevlerinizi idempotent olun.](functions-idempotent.md) 

Bir sıra öğesi zaten işlendiyse, işlevinizin bir no-op olmasına izin verin.

Azure İşlevler platformunda kullandığınız bileşenler için zaten sağlanan savunma önlemlerinden yararlanın. Örneğin, [Azure Depolama Sırası tetikleyicileri ve bağlamaları](functions-bindings-storage-queue-trigger.md#poison-messages)için belgelerde zehirli **sıra iletilerini işleme'ye** bakın. 

## <a name="scalability-best-practices"></a>Ölçeklenebilirlik en iyi uygulamalar

İşlev uygulamanızın örneklerinin ölçeğini etkileyen bir dizi etken vardır. Ayrıntılar [işlev ölçekleme](functions-scale.md)için belgelerde sağlanır.  Aşağıda, bir işlev uygulamasının en iyi ölçeklenebilirliğini sağlamak için en iyi uygulamalar verebilme leri yer alıyor.

### <a name="share-and-manage-connections"></a>Bağlantıları paylaşma ve yönetme

Mümkün olduğunda harici kaynaklara bağlantıları yeniden kullanın. [Azure İşlevleri'ndeki bağlantıları nasıl yönetebilirsiniz'](./manage-connections.md)i görün.

### <a name="avoid-sharing-storage-accounts"></a>Depolama hesaplarını paylaşmaktan kaçının

Bir işlev uygulaması oluşturduğunuzda, uygulamayı bir depolama hesabıyla ilişkilendirmeniz gerekir. Depolama hesabı bağlantısı [AzureWebJobsStorage uygulama ayarında](./functions-app-settings.md#azurewebjobsstorage)korunur. 

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Aynı işlev uygulamasında test ve üretim kodunu karıştırmayın

İşlev uygulaması içindeki işlevler kaynakları paylaşır. Örneğin, bellek paylaşılır. Üretimde bir işlev uygulaması kullanıyorsanız, testle ilgili işlevleri ve kaynakları eklemeyin. Üretim kodu yürütme sırasında beklenmeyen ek yüküneden olabilir.

Üretim işlevi uygulamalarınızda ne yüklediğinize dikkat edin. Bellek, uygulamadaki her işlevde ortalamaolarak dağıtılır.

Birden çok .NET işlevinde başvurulan paylaşılan bir derlemeniz varsa, ortak paylaşılan klasöre koyun. Aksi takdirde, yanlışlıkla işlevler arasında farklı davranan aynı ikili birden çok sürümü dağıtabilirsiniz.

Olumsuz performans etkisi olan üretim kodunda ayrıntılı günlüğe kaydetme kullanmayın.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Async kodu kullanın, ancak aramaları engellemeyi kaçının

Asynchronous programlama, özellikle G/Ç işlemlerinin engellenmesi söz konusu olduğunda önerilen en iyi yöntemdir.

C#'da, bir `Result` `Task` örnekte her `Wait` zaman özelliğe başvurmaktan veya arama yöntemini kullanmaktan kaçının. Bu yaklaşım iş parçacığı yorgunluğuna yol açabilir.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="use-multiple-worker-processes"></a>Birden çok alt işlem kullanma

Varsayılan olarak, Işlevler için herhangi bir ana bilgisayar örneği tek bir alt işlem kullanır. Özellikle Python gibi tek iş parçacığı çalışma zamanlarında performansı artırmak için, ana bilgisayar başına çalışan işlem sayısını (10'a kadar) artırmak için [FUNCTIONS_WORKER_PROCESS_COUNT](functions-app-settings.md#functions_worker_process_count) kullanın. Azure İşlevleri daha sonra bu çalışanlar arasında eşzamanlı işlev çağrılarını eşit olarak dağıtmaya çalışır. 

FUNCTIONS_WORKER_PROCESS_COUNT, isteğe uygun olarak uygulamanızı ölçeklerken Işlevlerin oluşturduğu her ana bilgisayar için geçerlidir. 

### <a name="receive-messages-in-batch-whenever-possible"></a>İletileri mümkün olduğunca toplu olarak alma

Olay Hub gibi bazı tetikleyiciler, tek bir çağrıda toplu ileti almayı sağlar.  Toplu iletiler çok daha iyi performansa sahiptir.  Dosyadaki `host.json` maksimum toplu iş boyutunu [host.json başvuru belgelerinde](functions-host-json.md) ayrıntılı olarak belirtildiği şekilde yapılandırabilirsiniz

C# işlevleri için türü güçlü bir şekilde yazılan bir diziyle değiştirebilirsiniz.  Örneğin, yöntem `EventData sensorEvent` imzası yerine `EventData[] sensorEvent`olabilir.  Diğer diller için, [burada gösterildiği gibi](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10)toplu işlemi etkinleştirmek `many` için, kardinallik özelliğini açıkça ayarlamalısınız. `function.json`

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Eşzamanlılığı daha iyi işlemek için ana bilgisayar davranışlarını yapılandırma

İşlev uygulamasındaki `host.json` dosya, ana bilgisayar çalışma zamanının ve tetikleyici davranışların yapılandırmasına olanak tanır.  Toplu işleme davranışlarına ek olarak, bir dizi tetikleyici için eşzamanlılık yönetebilirsiniz. Genellikle bu seçeneklerdeki değerleri ayarlamak, her örnek çağırılan işlevlerin talepleri için uygun şekilde ölçeklendirmeyardımcı olabilir.

Host.json dosyasındaki ayarlar, uygulama içindeki tüm işlevler arasında, işlevin tek bir *örneği* içinde uygulanır. Örneğin, iki HTTP işlevi ve [`maxConcurrentRequests`](functions-bindings-http-webhook-output.md#hostjson-settings) istekleri 25 olarak ayarlanmış bir işlev uygulamanız varsa, http tetikleyicisi için bir istek paylaşılan 25 eşzamanlı isteke doğru sayılır.  Bu işlev uygulaması 10 örneğe ölçeklendirildiğinde, iki işlev etkili bir şekilde 250 eşzamanlı isteke (10 örnek * örnek başına 25 eşzamanlı istek) izin verir. 

Diğer ana bilgisayar yapılandırma seçenekleri [host.json yapılandırma makalesinde](functions-host-json.md)bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri'nde bağlantıları yönetme](manage-connections.md)
* [Azure Uygulama Hizmeti en iyi uygulamaları](../app-service/app-service-best-practices.md)
