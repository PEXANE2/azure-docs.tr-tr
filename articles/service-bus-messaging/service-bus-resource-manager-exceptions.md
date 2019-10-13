---
title: Azure Service Bus Kaynak Yöneticisi özel durumları | Microsoft Docs
description: Azure Resource Manager ve önerilen eylemler tarafından ortaya çıkacak Service Bus özel durumların listesi.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: darosa
editor: spelluru
ms.assetid: 3d8526fe-6e47-4119-9f3e-c56d916a98f9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2019
ms.author: aschhab
ms.openlocfilehash: 9a2d25aba03156d6d14fe5ef9aa58b3748033b85
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72296394"
---
# <a name="service-bus-resource-manager-exceptions"></a>Service Bus Kaynak Yöneticisi özel durumları

Bu makalede, Azure Resource Manager aracılığıyla şablonlar veya doğrudan çağrılar kullanılarak Azure Service Bus ile etkileşim kurulurken oluşturulan özel durumlar listelenmektedir.

> [!IMPORTANT]
> Bu belge sıkça güncelleştirilir. Lütfen güncelleştirmeler için yeniden denetleyin.

Aşağıda, Azure Resource Manager üzerinden ortaya çıkacak çeşitli özel durumlar/hatalar verilmiştir.

## <a name="error-bad-request"></a>Hata: Hatalı Istek

"Kötü Istek", Kaynak Yöneticisi tarafından alınan isteğin doğrulaması başarısız olduğunu gösterir.

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Hatalı İstek | 40000 | Alt kod = 40000. ' *Namespace Name '* ad alanı ' temel ' katmanını kullandığından, bir kuyruk oluşturulurken *' özellik adı '* özelliği ayarlanamaz. Bu işlem yalnızca ' standart ' veya ' Premium ' katmanda destekleniyor. | Azure Service Bus temel katmanda, aşağıdaki Özellikler ayarlanamaz veya güncelleştirilemez- <ul> <li> RequiresDuplicateDetection </li> <li> Oto Delete OnIdle </li> <li>RequiresSession</li> <li>DefaultMessageTimeToLive </li> <li> DuplicateDetectionHistoryTimeWindow </li> <li> EnableExpress </li> <li> ForwardTo </li> <li> Konular </li> </ul> | Bu işlevselliği kullanmak için temel bilgisayardan standart veya Premium katmana yükseltmeyi düşünün. |
| Hatalı İstek | 40000 | Alt kod = 40000. Varolan bir kuyruğun (veya konusunun) ' requiresDuplicateDetection ' özelliğinin değeri değiştirilemez. | Yinelenen algılama, varlık oluşturma sırasında etkinleştirilmelidir/devre dışı bırakılmalıdır. Yinelenen algılama yapılandırma parametresi oluşturulduktan sonra değiştirilemez. | Daha önce oluşturulmuş bir kuyrukta/konuda yinelenen saptamayı etkinleştirmek için, yinelenen algılama ile yeni bir kuyruk/konu oluşturabilir ve ardından orijinal kuyruktan yeni kuyruğa/konuya iletebilirsiniz. |
| Hatalı İstek | 40000 | Alt kod = 40000. Belirtilen 16384 değeri geçersiz. ' Maxsizeınmegabayt ' özelliği şu değerlerden biri olmalıdır: 1024; 2048; 3072; 4096; 5120. | Maxsizeınmegabayt değeri geçersiz. | Maxsizeınmegabayt 'nin aşağıdaki-1024, 2048, 3072, 4096, 5120 değerinden biri olduğundan emin olun. |
| Hatalı İstek | 40000 | Alt kod = 40000. Kuyruk/konu için bölümleme değiştirilemiyor. | Varlık için bölümleme değiştirilemez. | Yeni bir varlık (kuyruk veya konu) oluşturun ve bölümleri etkinleştirin. | 
| Hatalı İstek | yok | *' Namespace Name '* ad alanı yok. | Ad alanı Azure aboneliğinizde yok. | Bu hatayı çözmek için lütfen aşağıdaki adımları deneyin <ul> <li> Azure aboneliğinin doğru olduğundan emin olun. </li> <li> Ad alanının mevcut olduğundan emin olun. </li> <li> Ad alanı adının doğru olduğundan emin olun (yazım hatası veya null dizeler yok). </li> </ul> | 
| Hatalı İstek | 40400 | Alt kod = 40400. Otomatik iletme hedefi varlığı yok. | Oto iletme hedef varlığının hedefi yok. | Kaynak oluşturulmadan önce hedef varlık (kuyruk veya konu) var olmalıdır. Hedef varlığı oluşturduktan sonra yeniden deneyin. |
| Hatalı İstek | 40000 | Alt kod = 40000. Sağlanan kilit zamanı, izin verilen en fazla ' 5 ' dakika sınırını aşıyor. | İletinin kilitlenebileceği zaman 1 dakika (en az) ve 5 dakika (en fazla) arasında olmalıdır. | Verilen kilit zamanının 1 dakika ile 5 dakika arasında olduğundan emin olun. |
| Hatalı İstek | 40000 | Alt kod = 40000. Hem Delayedkalıcılık hem de RequiresDuplicateDetection özelliği birlikte etkinleştirilemez. | Yinelenen algılama özelliği etkin olan varlıkların kalıcı olması gerekir, bu nedenle Kalıcılık gecikmemelidir. | [Yinelenen algılama](duplicate-detection.md) hakkında daha fazla bilgi edinin |
| Hatalı İstek | 40000 | Alt kod = 40000. Mevcut bir kuyruğun RequiresSession özelliğinin değeri değiştirilemez. | Varlık oluşturma sırasında oturum desteği etkinleştirilmelidir. Oluşturulduktan sonra, var olan bir varlıkta (kuyruk veya abonelik) oturumları etkinleştiremez/devre dışı bırakamezsiniz | "RequiresSession" özelliği etkinken yeni bir kuyruğu (veya aboneliği) silin ve yeniden oluşturun. |
| Hatalı İstek | 40000 | Alt kod = 40000. ' URI_PATH ', Service Bus izin verilmeyen karakter (ler) içeriyor. Varlık kesimleri yalnızca harf, sayı, nokta (.), kısa çizgi (-) ve alt çizgi (_) içerebilir. | Varlık kesimleri yalnızca harf, sayı, nokta (.), kısa çizgi (-) ve alt çizgi (_) içerebilir. Diğer tüm karakterler isteğin başarısız olmasına neden olur. | URI yolunda geçersiz karakter bulunmadığından emin olun. |


## <a name="error-code-429"></a>Hata kodu: 429

HTTP içinde olduğu gibi, "hata kodu 429", "çok fazla istek" anlamına gelir. Bu kaynakta çok fazla istek (veya çakışan işlemler nedeniyle) nedeniyle belirli bir kaynağın (ad alanı) kısıtlandığı anlamına gelir.

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | 50004 | Alt kod = 50004. *Ad* alanınız kısıtlandığı için istek sonlandırıldı. | Bu hata koşulu, gelen istek sayısı kaynağın sınırlamasını aştığında oluşur. | Birkaç saniye bekleyip yeniden deneyin. <br/> <br/> [Kotalar](service-bus-quotas.md) ve [Azure Resource Manager istek limitleri](../azure-resource-manager/resource-manager-request-limits.md) hakkında daha fazla bilgi edinin|
| 429 | 40901 | Alt kod = 40901. Başka bir çakışan işlem devam ediyor. | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce geçerli devam eden işlemin tamamlanmasını bekleyin. |
| 429 | 40900 | Alt kod = 40900. Uzantıları. Kaynağın geçerli durumunda izin verilmeyen bir işlem isteğinde bulunuyoruz. | Aynı anda aynı varlık (kuyruk, konu, abonelik veya kural) üzerinde işlemleri gerçekleştirmek için birden çok istek yapıldığında bu koşul çıkabilir. | Birkaç saniye bekleyip yeniden deneyin |
| 429 | 40901 | *' Varlık adı '* varlığındaki istek başka bir istekle çakışıyor | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin |
| 429 | 40901 | *' Varlık adı '* varlığı için başka bir güncelleştirme isteği devam ediyor. | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin |
| 429 | yok | Kaynak çakışması oluştu. Başka bir çakışan işlem devam ediyor olabilir. Bu, başarısız olan bir işlem için yeniden denemeye devam ederse, arka planda temizlik hala bekliyor. Daha sonra yeniden deneyin. | Aynı varlığa karşı bekleyen bir işlem olduğunda bu durum görünebilir. | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin. |


## <a name="error-code-not-found"></a>Hata kodu: bulunamadı

Bu hata sınıfı, kaynağın bulunamadığını gösterir.

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Bulunamadı | yok | *' Varlık adı '* varlığı bulunamadı. | İşlemin bulunamadığı varlık. | Varlığın mevcut olup olmadığını denetleyin ve işlemi yeniden deneyin. |
| Bulunamadı | yok | Bulunamadı. Işlem yok. | Gerçekleştirmeye çalıştığınız işlem yok. | İşlemi denetleyip yeniden deneyin. |
| Bulunamadı | yok | Gelen istek, bir ad alanı ilkesi PUT isteği olarak tanınmıyor. | Gelen istek gövdesi null ve bu nedenle bir PUT isteği olarak yürütülemez. | Lütfen, null olmadığından emin olmak için istek gövdesini kontrol edin. | 
| Bulunamadı | yok | *' Varlık adı '* mesajlaşma varlığı bulunamadı. | İşlemi yürütmeye çalıştığınız varlık bulunamadı. | Lütfen varlığın mevcut olup olmadığını denetleyin ve işlemi yeniden deneyin. |
