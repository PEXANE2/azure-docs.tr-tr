---
title: Azure Işlevleri için en iyi uygulamalar | Microsoft Docs
description: Azure Işlevleri için en iyi uygulamaları ve desenleri öğrenin.
services: functions
documentationcenter: na
author: wesmc7777
manager: jeconnoc
keywords: Azure işlevleri, desenler, en iyi uygulama, işlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: azure-functions
ms.topic: conceptual
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2782781fdfd560c0c8f322e362fcf74c796664bd
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933051"
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Azure Işlevlerinin performansını ve güvenilirliğini iyileştirin

Bu makale, [sunucusuz](https://azure.microsoft.com/solutions/serverless/) işlev uygulamalarınızın performansını ve güvenilirliğini artırmaya yönelik rehberlik sağlar. 

## <a name="general-best-practices"></a>Genel en iyi uygulamalar

Azure Işlevleri 'ni kullanarak sunucusuz çözümlerinizi oluşturma ve mimarinizi geliştirme konusunda en iyi yöntemler aşağıda verilmiştir.

### <a name="avoid-long-running-functions"></a>Uzun süre çalışan işlevlerden kaçının

Büyük ve uzun süre çalışan işlevler, beklenmeyen zaman aşımı sorunlarına neden olabilir. Bir işlev, çok sayıda Node. js bağımlılığı nedeniyle büyük olabilir. Bağımlılıkları içeri aktarmak, beklenmedik zaman aşımları ile sonuçlanan daha fazla yükleme süresi de oluşmasına neden olabilir. Bağımlılıklar hem açık hem de örtük olarak yüklenir. Kodunuz tarafından yüklenen tek bir modül kendi ek modüllerini yükleyebilir.  

Mümkün olduğunda, büyük işlevleri birlikte çalışarak daha küçük işlev kümelerine yeniden düzenleyin ve yanıtları hızlı bir şekilde geri döndürün. Örneğin, bir Web kancası veya HTTP tetikleyici işlevi belirli bir zaman sınırı içinde bir bildirim yanıtı gerektirebilir; Web kancalarının anında yanıt gerektirmesi için yaygındır. HTTP tetikleyici yükünü bir kuyruk tetikleyicisi işlevi tarafından işlenmek üzere bir kuyruğa geçirebilirsiniz. Bu yaklaşım, gerçek işi ertelemenizi ve anında yanıt döndürmesini sağlar.


### <a name="cross-function-communication"></a>Çapraz işlev iletişimi

[Dayanıklı işlevler](durable/durable-functions-overview.md) ve [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , durum geçişlerini ve birden çok işlev arasındaki iletişimi yönetmek için oluşturulmuştur.

Birden çok işlevle tümleştirilecek Dayanıklı İşlevler veya Logic Apps kullanmıyorsanız, genellikle çoklu işlev iletişimi için depolama kuyrukları kullanmak en iyi uygulamadır.  Ana neden, depolama kuyrukları ve sağlanması çok daha kolaydır. 

Depolama sırasındaki tek tek mesajlar boyut olarak 64 KB ile sınırlıdır. İşlevler arasında daha büyük iletiler geçirmeniz gerekiyorsa, Standart katmanda 256 KB 'ye kadar olan ileti boyutlarını desteklemek için bir Azure Service Bus kuyruğu ve Premium katmanında en fazla 1 MB kullanılabilir.

Service Bus konular, işlemeden önce ileti filtrelemesini gerekli kıldıysanız yararlıdır.

Olay Hub 'ları, yüksek hacimli iletişimleri desteklemek için faydalıdır.


### <a name="write-functions-to-be-stateless"></a>İşlevleri durum bilgisiz olacak şekilde yaz 

İşlevler, mümkünse durum bilgisiz ve ıdempotent olmalıdır. Gerekli durum bilgilerini verileriniz ile ilişkilendirin. Örneğin, işlenen bir sıra, muhtemelen ilişkili `state` bir üyeye sahip olabilir. İşlev durum bilgisiz olmaya devam ederken bir işlev bu duruma göre bir sırayı işleyebilir. 

Idempotent işlevleri, özellikle Zamanlayıcı tetikleyicilerle önerilir. Örneğin, her gün bir kez çalışması gereken bir şeydir varsa, aynı sonuçlara sahip gün içinde herhangi bir zaman çalışabilmesi için yazın. Belirli bir gün için iş olmadığında işlev çıkabilir. Ayrıca, önceki bir çalıştırmanın tamamlanmadıysa, sonraki çalıştırmanın kaldığınız yerden devam etmesi gerekir.


### <a name="write-defensive-functions"></a>Savunma işlevlerini yaz

İşlevinizin istediğiniz zaman bir özel durumla karşılaşdığını varsayın. Bir sonraki yürütme sırasında işlevlerinizi önceki bir başarısızlık noktasından devam etme özelliğiyle tasarlayın. Aşağıdaki eylemleri gerektiren bir senaryo düşünün:

1. Bir veritabanında 10.000 satırı sorgulayın.
2. Bu satırların her biri için çizgi üzerinde daha fazla işlem yapmak üzere bir kuyruk iletisi oluşturun.
 
Nasıl karmaşık sisteminize bağlı olarak, size sahip olabilir: hatalı davranmakta söz konusu aşağı akış hizmetleriyle ağ kesintileri veya kota sınırları ulaşıldığında, vb. Tüm bunlar, işlevinizi dilediğiniz zaman etkileyebilir. İşlevlerinizi hazırlanmakta olacak şekilde tasarlamanız gerekir.

İşlenmek üzere bu öğelerin 5.000 ' i bir sıraya ekledikten sonra bir hata oluşursa, kodunuz nasıl tepki veriyor? Tamamladığınız bir küme içindeki öğeleri izleyin. Aksi halde, bir dahaki sefer yeniden ekleyebilirsiniz. Bu, iş akışınız üzerinde ciddi bir etkiye sahip olabilir. 

Bir kuyruk öğesi zaten işlendiyse, işlevinizin işlem dışı çalışmasına izin verin.

Azure Işlevleri platformunda kullandığınız bileşenler için zaten sağlanmış olan savunma ölçülerinin avantajlarından yararlanın. Örneğin, bkz. [Azure depolama kuyruğu Tetikleyicileri ve bağlamaları](functions-bindings-storage-queue.md#trigger---poison-messages)belgelerinde **Poison Queue iletilerini işleme** . 

## <a name="scalability-best-practices"></a>Ölçeklenebilirlik en iyi uygulamaları

İşlev uygulamanızın nasıl ölçeklenmesi etkileyebilecek bir dizi etken vardır. Ayrıntılar, [işlev ölçeklendirmeyle](functions-scale.md)ilgili belgelerde verilmiştir.  Aşağıda, bir işlev uygulamasının en iyi şekilde ölçeklenebilirliğini sağlamak için bazı en iyi yöntemler verilmiştir.

### <a name="share-and-manage-connections"></a>Bağlantıları paylaşma ve yönetme

Mümkün olduğunda dış kaynaklarla bağlantıları yeniden kullanın.  Bkz. [Azure işlevlerinde bağlantıları yönetme](./manage-connections.md).

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Aynı işlev uygulamasında test ve üretim kodunu karıştırmayın

İşlev uygulaması içindeki işlevler kaynakları paylaşır. Örneğin, bellek paylaşılır. Üretimde bir işlev uygulaması kullanıyorsanız, bu uygulamaya test ile ilgili işlevler ve kaynaklar eklemeyin. Üretim kodu yürütme sırasında beklenmeyen yüke neden olabilir.

Üretim işlevi uygulamalarınızda ne yüklediklerinizi dikkatli olun. Uygulamadaki her bir işlevde bellek ortalaması.

Çoklu .NET işlevlerinde başvurulan paylaşılan bir derlemeniz varsa, bunu ortak bir paylaşılan klasöre koyun. C# Betik (. CSX) kullanılıyorsa aşağıdaki örneğe benzer bir ifadeyle derlemeye başvurun: 

    #r "..\Shared\MyAssembly.dll". 

Aksi halde, işlevleri arasında farklı şekilde davranan aynı ikilinin birden çok test sürümünü yanlışlıkla dağıtmak kolaydır.

Üretim kodunda ayrıntılı günlük kullanmayın. Bu, olumsuz bir performans etkisine sahiptir.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Zaman uyumsuz kod kullan ancak çağrı engellemeyi önleyin

Zaman uyumsuz programlama önerilen en iyi uygulamadır. Ancak, her zaman bir `Result` `Task` örnek üzerinde özelliğe veya `Wait` çağırma yöntemine başvurulmaktan kaçının. Bu yaklaşım iş parçacığı tükenmesine yol açabilir.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Mümkün olduğunda toplu iş içinde ileti alma

Olay Hub 'ı gibi bazı Tetikleyiciler tek bir çağrıdan bir toplu ileti almayı sağlar.  Toplu işlem iletilerinin çok daha iyi performansı vardır.  `host.json` Dosyadaki en büyük toplu iş boyutunu, [Host. JSON başvuru belgelerinde](functions-host-json.md) açıklandığı şekilde yapılandırabilirsiniz.

İşlevler C# için türü türü kesin belirlenmiş bir dizi olarak değiştirebilirsiniz.  Örneğin, yöntem imzası yerine `EventData sensorEvent` olabilir. `EventData[] sensorEvent`  Diğer diller için, `function.json` [burada gösterildiği gibi](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10)toplu işi etkinleştirmek için ' de `many` kardinalite özelliğini açık olarak ayarlamanız gerekir.

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Eşzamanlılık daha iyi işlemek için konak davranışlarını yapılandırın

İşlev `host.json` uygulamasındaki dosya, ana bilgisayar çalışma zamanı ve tetikleyici davranışları yapılandırmasına izin verir.  İşleme davranışlarına ek olarak, bir dizi tetikleyici için eşzamanlılık yönetebilirsiniz.  Genellikle bu seçeneklerdeki değerlerin ayarlanması, her bir örneğin, çağrılan işlevlerin taleplerine uygun şekilde ölçeklendirilmesine yardımcı olabilir.

Hosts dosyasındaki ayarlar, işlevin *tek bir örneği* içinde, uygulamadaki tüm işlevler arasında geçerlidir. Örneğin, 2 HTTP işlevlerine sahip bir işlev uygulamanız varsa ve eşzamanlı istekler 25 olarak ayarlandıysa, HTTP tetikleyicisine yönelik bir istek paylaşılan 25 eşzamanlı istek için doğru sayılır.  Bu işlev uygulaması 10 örneğe ölçeklenirse, 2 işlev 250 eşzamanlı istek (10 örnek * örnek başına 25 eşzamanlı istek) etkin bir şekilde izin verir.

**HTTP eşzamanlılık ana bilgisayar seçenekleri**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Diğer konak yapılandırma seçenekleri [, ana bilgisayar yapılandırma belgesinde](functions-host-json.md)bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure Işlevlerinde bağlantıları yönetme](manage-connections.md)
* [En iyi Azure App Service uygulamalar](../app-service/app-service-best-practices.md)
