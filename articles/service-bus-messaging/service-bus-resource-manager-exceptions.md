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
ms.openlocfilehash: e666503b9e8888e7d61445639fe0f3adeeffe55f
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71329311"
---
# <a name="service-bus-resource-manager-exceptions"></a>Service Bus Kaynak Yöneticisi özel durumları

Bu makalede, Azure Resource Manager aracılığıyla şablonlar veya doğrudan çağrılar kullanılarak Azure Service Bus ile etkileşim kurulurken oluşturulan özel durumlar listelenmektedir.

> [!IMPORTANT]
> Bu belge sıkça güncelleştirilir. Lütfen güncelleştirmeler için yeniden denetleyin.

Aşağıda, Azure Resource Manager üzerinden ortaya çıkacak çeşitli özel durumlar/hatalar verilmiştir.

## <a name="error-bad-request"></a>Hata: Bozuk İstek

"Kötü Istek", Kaynak Yöneticisi tarafından alınan isteğin doğrulaması başarısız olduğunu gösterir.

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Bozuk İstek | 40000 | Alt kod = 40000. ' *Namespace Name '* ad alanı ' temel ' katmanını kullandığından, bir kuyruk oluşturulurken *' özellik adı '* özelliği ayarlanamaz. Bu işlem yalnızca ' standart ' veya ' Premium ' katmanda destekleniyor. | Azure Service Bus temel katmanda, aşağıdaki Özellikler ayarlanamaz veya güncelleştirilemez- <ul> <li> RequiresDuplicateDetection </li> <li> Oto Delete OnIdle </li> <li>requiresSession</li> <li>DefaultMessageTimeToLive </li> <li> DuplicateDetectionHistoryTimeWindow </li> <li> EnableExpress </li> <li> ForwardTo </li> <li> Konu başlıkları </li> </ul> | Bu işlevden yararlanmak için temel bilgisayardan standart veya Premium katmana yükseltmeyi düşünün. |
| Bozuk İstek | 40000 | Alt kod = 40000. Mevcut bir kuyruğun ' requiresDuplicateDetection ' özelliğinin değeri değiştirilemez. | Yinelenen algılama, varlık oluşturma sırasında etkinleştirilmelidir/devre dışı bırakılmalıdır. Oluşturulduktan sonra, yinelenen algılama yapılandırma parametresi değiştirilemez. | Daha önce oluşturulmuş bir kuyrukta/abonelikte yinelenen saptamayı etkinleştirmek için, yinelenen algılama ile yeni bir kuyruk oluşturabilir ve ardından orijinal kuyruktan yeni kuyruğa iletebilirsiniz. |
| Bozuk İstek | 40000 | Alt kod = 40000. Belirtilen 16384 değeri geçersiz. ' Maxsizeınmegab' özelliği aşağıdaki değerlerden biri olmalıdır: 1024; 2048; 3072; 4096; 5120. | Maxsizeınmegabayt değeri geçersiz. | Maxsizeınmegabayt 'nin aşağıdaki-1024, 2048, 3072, 4096, 5120 değerinden biri olduğundan emin olun. |
| Bozuk İstek | 40000 | Alt kod = 40000. Sıra için bölümleme değiştirilemiyor. | Varlık için bölümleme değiştirilemez. | Yeni bir varlık oluşturun ve bölümleri etkinleştirin. | 
| Bozuk İstek | yok | *' Namespace Name '* ad alanı yok. | Ad alanı Azure aboneliğinizde yok. | Bu hatayı çözmek için lütfen aşağıdaki adımları deneyin <ul> <li> Azure aboneliğinin doğru olduğundan emin olun. </li> <li> Ad alanının mevcut olduğundan emin olun. </li> <li> Ad alanı adının doğru olduğundan emin olun (yazım hatası veya null dizeler yok). </li> </ul> | 
| Bozuk İstek | 40400 | Alt kod = 40400. Otomatik iletme hedefi varlığı yok. | Oto iletme hedef varlığının hedefi yok. | Kaynak oluşturulmadan önce hedef varlık (kuyruk veya konu) var olmalıdır. Hedef varlığı oluşturduktan sonra yeniden deneyin. |


## <a name="error-code-429"></a>Hata kodu: 429

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | 50004 | Alt kod = 50004. *Ad* alanınız kısıtlandığı için istek sonlandırıldı. | Bu hata koşulu, gelen istek sayısı kaynağın sınırlamasını aştığında oluşur. | Birkaç saniye bekleyip yeniden deneyin. <br/> <br/> [Kotalar](service-bus-quotas.md) ve [Azure Resource Manager istek limitleri](../azure-resource-manager/resource-manager-request-limits.md) hakkında daha fazla bilgi edinin|
| 429 | 40901 | Alt kod = 40901. Başka bir çakışan işlem devam ediyor. | Aynı kaynak/varlık üzerinde başka bir çakışan işlem devam ediyor | Yeniden denemeden önce geçerli devam eden işlemin tamamlanmasını bekleyin. |
| 429 | 40900 | Alt kod = 40900. Uzantıları. Kaynağın geçerli durumunda izin verilmeyen bir işlem isteğinde bulunuyoruz. | Aynı anda aynı varlık (kuyruk, konu, abonelik veya kural) üzerinde işlemleri gerçekleştirmek için birden çok istek yapıldığında bu durum çıkabilir | Birkaç saniye bekleyip yeniden deneyin |
| 429 | yok | Kaynak çakışması oluştu. Başka bir çakışan işlem devam ediyor olabilir. Bu, başarısız olan bir işlem için yeniden denemeye devam ederse, arka planda temizlik hala bekliyor. Daha sonra tekrar deneyin. | Aynı varlığa karşı bekleyen bir işlem olduğunda bu durum görünebilir. | Yeniden denemeden önce önceki işlemin tamamlanmasını bekleyin. |

