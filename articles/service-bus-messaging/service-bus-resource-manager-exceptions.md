---
title: Azure Hizmet Veri Kurumu Kaynak Yöneticisi özel durumlar | Microsoft Dokümanlar
description: Azure Kaynak Yöneticisi tarafından ortaya çıkan Hizmet İçi Hizmet Veri Mes'leri listesi ve önerilen eylemler.
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
ms.openlocfilehash: 0f328651ac4422226071d2de12e9cbc787ef64be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75978265"
---
# <a name="service-bus-resource-manager-exceptions"></a>Servis Veri Günü Kaynak Yöneticisi özel durumları

Bu makalede, Şablonlar veya doğrudan aramalar aracılığıyla Azure Kaynak Yöneticisi'ni kullanarak Azure Hizmet Veri Servisi ile etkileşimde bulunurken oluşturulan özel durumlar listelenir.

> [!IMPORTANT]
> Bu belge sık sık güncelleştirilir. Güncellemeler için lütfen tekrar kontrol edin.

Aşağıda Azure Kaynak Yöneticisi aracılığıyla ortaya çıkan çeşitli özel durumlar/hatalar verilmiştir.

## <a name="error-bad-request"></a>Hata: Kötü İstek

"Hatalı İstek", Kaynak Yöneticisi tarafından alınan isteğin doğrulamada başarısız olduğunu gösterir.

| Hata kodu | Hata Alt Kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Hatalı İstek | 40000 | Alt Kod=40000. Ad alanı *'ad alanı adı'* 'Temel' Katman'ı kullandığından, *'özellik adı'* bir Sıra oluştururken ayarlanamaz. Bu işlem yalnızca 'Standart' veya 'Premium' katmanında desteklenir. | Azure Hizmet Veri Veri Kümesi Temel Katmanı'nda, aşağıdaki özellikler ayarlanamaz veya güncelleştirilemez - <ul> <li> Yinelenen Algılama Gerektirir </li> <li> AutoDeleteOnIdle </li> <li>Gerektirir Oturumu</li> <li>VarsayılanMessageTimeToLive </li> <li> Yinelenen AlgılamaGeçmişiZaman Penceresi </li> <li> EnableExpress </li> <li> ForwardTo </li> <li> Konular </li> </ul> | Bu işlevi kullanmak için Temel'den Standart veya Premium katmana yükseltmeyi düşünün. |
| Hatalı İstek | 40000 | Alt Kod=40000. Varolan bir Queue'nin (veya Konunun) 'requiresDuplicateDetection' özelliğinin değeri değiştirilemez. | Yinelenen algılama, varlık oluşturma sırasında etkinleştirilmeli/devre dışı bırakılmalı. Yinelenen algılama yapılandırma parametresi oluşturulduktan sonra değiştirilemez. | Daha önce oluşturulmuş bir sıra/konu üzerinde yinelenen algılamayı etkinleştirmek için, yinelenen algılamalı yeni bir kuyruk/konu oluşturabilir ve özgün kuyruktan yeni kuyruğa/konuya iletebilirsiniz. |
| Hatalı İstek | 40000 | Alt Kod=40000. Belirtilen değer 16384 geçersizdir. Özellik 'MaxSizeInMegabayt', aşağıdaki değerlerden biri olmalıdır: 1024;2048;3072;4096;5120. | MaxSizeInMegabayt değeri geçersizdir. | 1024, 2048, 3072, 4096, 5120 - MaxSizeInMegabayt aşağıdakilerden biri olduğundan emin olun. |
| Hatalı İstek | 40000 | Alt Kod=40000. Sıra/Konu için bölümleme değiştirilemez. | Bölümleme varlık için değiştirilemez. | Yeni bir varlık (sıra veya konu) oluşturun ve bölümleri etkinleştirin. | 
| Hatalı İstek | yok | Ad alanı *'namespace adı'* yok. | Ad alanı Azure aboneliğinizde yok. | Bu hatayı gidermek için lütfen aşağıdaki leri deneyin <ul> <li> Azure Aboneliği'nin doğru olduğundan emin olun. </li> <li> Ad alanının var olduğundan emin olun. </li> <li> Ad alanı adının doğru olduğunu doğrulayın (yazım hatası veya null dizeleri yok). </li> </ul> | 
| Hatalı İstek | 40400 | Alt Kod=40400. Otomatik yönlendirme hedef varlığı yok. | Otomatik yönlendirme hedef varlığı için hedef yok. | Kaynak oluşturulmadan önce hedef varlığın (sıra veya konu) var olması gerekir. Hedef varlığı oluşturduktan sonra yeniden deneyin. |
| Hatalı İstek | 40000 | Alt Kod=40000. Verilen kilit süresi izin verilen maksimum '5' dakikayı aşıyor. | İletinin kilitlenme süresi 1 dakika (en az) ile 5 dakika (en fazla) arasında olmalıdır. | Verilen kilit süresinin 1 dk ile 5 dakika arasında olduğundan emin olun. |
| Hatalı İstek | 40000 | Alt Kod=40000. Hem Gecikmekalıcılık hem de RequiresDuplicateDetection özelliği birlikte etkinleştirilemez. | Yineleme algılaması etkin leştirilmiş varlıklar kalıcı olmalıdır, bu nedenle kalıcılık geciktirilemez. | [Yinelenen Algılama](duplicate-detection.md) hakkında daha fazla bilgi edinin |
| Hatalı İstek | 40000 | Alt Kod=40000. Varolan bir Kuyruğun RequiresSession özelliğinin değeri değiştirilemez. | Varlık oluşturma sırasında oturumdesteği etkinleştirilmelidir. Oluşturulduktan sonra, varolan bir varlıkta (sıra veya abonelik) oturumları etkinleştiremez/devre dışı bırakamazsınız | "CgerektiSession" özelliği etkinken yeni bir sırayı (veya aboneliği) silin ve yeniden oluşturun. |
| Hatalı İstek | 40000 | Alt Kod=40000. 'URI_PATH' Servis Veri Servisi tarafından izin verilmeyen karakter(ler) içerir. Varlık bölümleri yalnızca harfleri, sayıları, dönemleri(.), tireleri(-) ve alt çizgi(_) içerebilir. | Varlık bölümleri yalnızca harfleri, sayıları, dönemleri(.), tireleri(-) ve alt çizgi(_) içerebilir. Diğer karakterler isteğin başarısız olması için neden olur. | URI Yolu'nda geçersiz karakter olmadığından emin olun. |


## <a name="error-code-429"></a>Hata kodu: 429

TıPKı HTTP'de olduğu gibi, "Hata kodu 429" "çok fazla istek" anlamına gelir. Belirli bir kaynağın (ad alanının) bu kaynaktaki çok fazla istek (veya çakışan işlemler nedeniyle) nedeniyle azaltıldığını ifade eder.

| Hata kodu | Hata Alt Kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | 50004 | Alt Kod=50004. *Ad alanınız* azaltıldığı için istek sonlandırıldı. | Gelen istek sayısı kaynağın sınırlamasını aştığında bu hata koşulu na çarpılır. | Birkaç saniye bekleyin ve tekrar deneyin. <br/> <br/> [Kotalar](service-bus-quotas.md) ve Azure [Kaynak Yöneticisi istek sınırları](../azure-resource-manager/management/request-limits-and-throttling.md) hakkında daha fazla bilgi edinin|
| 429 | 40901 | Alt Kod=40901. Çakışan bir operasyon daha devam ediyor. | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce geçerli devam eden işlemin tamamlanmasını bekleyin. |
| 429 | 40900 | Alt Kod=40900. Çakışma. Kaynağın geçerli durumunda izin verilmeyen bir işlem isteğinde bulunun. | Aynı varlık (sıra, konu, abonelik veya kural) üzerinde aynı anda işlemleri gerçekleştirmek için birden çok istek yapıldığında bu durum vurulabilir. | Birkaç saniye bekleyin ve yeniden deneyin |
| 429 | 40901 | Varlık *'varlık adı'* talebi başka bir istekle çelişti | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin |
| 429 | 40901 | *Varlık 'varlık adı'* için başka bir güncelleştirme isteği devam ediyor. | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin |
| 429 | yok | Kaynak Çakışması Oluştu. Çakışan başka bir işlem devam ediyor olabilir. Bu, başarısız çalışma için yeniden deneme ise, arka plan temizleme hala beklemede. Daha sonra tekrar deneyin. | Aynı varlığa karşı bekleyen bir işlem olduğunda bu durum vurulabilir. | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin. |


## <a name="error-code-not-found"></a>Hata kodu: Bulunamadı

Bu hata sınıfı, kaynağın bulunamadıgini gösterir.

| Hata kodu | Hata Alt Kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Bulunamadı | yok | Varlık *'varlık adı'* bulunamadı. | Operasyonun bulunamayan taraf. | Varlığın var olup olmadığını denetleyin ve işlemi yeniden deneyin. |
| Bulunamadı | yok | Bulunamadı. Operasyon diye bir şey yok. | Gerçekleştirmeye çalıştığınız işlem yok. | İşlemi kontrol edin ve yeniden deneyin. |
| Bulunamadı | yok | Gelen istek bir ad alanı ilkesi olarak tanınmıyor. | Gelen istek gövdesi geçersizdir ve bu nedenle bir istek olarak yürütülemez. | Null olmadığından emin olmak için lütfen istek gövdesini kontrol edin. | 
| Bulunamadı | yok | İleti varlığı *'varlık adı'* bulunamadı. | İşlemi yürütmeye çalıştığınız varlık bulunamadı. | Lütfen varlığın var olup olmadığını kontrol edin ve işlemi yeniden deneyin. |

## <a name="error-code-internal-server-error"></a>Hata kodu: Dahili Sunucu Hatası

Bu hata sınıfı, bir iç sunucu hatası olduğunu gösterir

| Hata kodu | Hata Alt Kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Dahili Sunucu Hatası | 50000 | Alt Kod=50000. Dahili Sunucu Hatası| Çeşitli nedenlerle olabilir. Bazı belirtiler - <ul> <li> İstemci isteği/gövdesi bozuk ve bir hataya yol açar. </li> <li> İstemci isteği, hizmetteki işleme sorunları nedeniyle zaman doldu. </li> </ul> | Bunu çözmek için <ul> <li> İstek parametrelerinin null veya yanlış biçimli olmadığından emin olun. </li> <li> İsteği yeniden deneyin. </li> </ul> |

## <a name="error-code-unauthorized"></a>Hata kodu: Yetkisiz

Bu hata sınıfı, komutu çalıştırmak için yetkilendirme nin yokluğunu gösterir.

| Hata kodu | Hata Alt Kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Yetkisiz | yok | İkincil ad alanında geçersiz işlem. İkincil ad alanı salt okunur. | İşlem, yalnızca okunan ad alanı olarak kurulumu olan ikincil ad alanına karşı gerçekleştirildi. | Birincil ad alanına karşı komutu yeniden deneyin. [İkincil ad alanı](service-bus-geo-dr.md) hakkında daha fazla bilgi edinin |
| Yetkisiz | yok | Eksik Token: Yetkilendirme üstbilgisi bulunamadı. | Bu hata, yetkilendirme geçersiz veya yanlış değerlere sahip olduğunda oluşur. | Yetkilendirme üstbilgisinde belirtilen belirteç değerinin doğru olduğundan ve null olmadığından emin olun. |