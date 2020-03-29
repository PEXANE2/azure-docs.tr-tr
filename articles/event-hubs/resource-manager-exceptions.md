---
title: Azure Etkinlik Hub'ları - Kaynak Yöneticisi özel durumları | Microsoft Dokümanlar
description: Azure Etkinlik Hub'ları özel durumları listesi Azure Kaynak Yöneticisi tarafından ve önerilen eylemler tarafından su yüzüne çıkarılmıştır.
services: service-bus-messaging
documentationcenter: na
author: spelluru
editor: spelluru
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/08/2019
ms.author: spelluru
ms.openlocfilehash: e6ee1137fce97cbe5a64aa5287223f6ba09dcf47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74936093"
---
# <a name="azure-event-hubs---resource-manager-exceptions"></a>Azure Olay Hub'ları - Kaynak Yöneticisi özel durumları
Bu makalede, Şablonlar veya doğrudan aramalar aracılığıyla Azure Kaynak Yöneticisi kullanarak Azure Etkinlik Hub'larıyla etkileşimde bulunurken oluşturulan özel durumlar listelenir.

> [!IMPORTANT]
> Bu belge sık sık güncelleştirilir. Güncellemeler için lütfen tekrar kontrol edin.

Aşağıdaki bölümler, Azure Kaynak Yöneticisi aracılığıyla ortaya çıkan çeşitli özel durumlar/hatalar sağlar.

## <a name="error-code-conflict"></a>Hata kodu: Çakışma

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Çakışma | 40300 | EventHub türündeki maksimum kaynak sayısına ulaşıldı veya aşıldı. Gerçek: #, Max izin: # | Ad alanı, içerebileceği Olay Hub'ları sayısı için [kotasına](event-hubs-quotas.md) ulaştı. | Ad alanından kullanılmayan veya gereksiz olay hub'larını silin veya özel bir [kümeye](event-hubs-dedicated-overview.md)yükseltmeyi düşünün. |
| Çakışma | yok | Çoğaltma devam ettiği için olağanüstü durum kurtarma (DR) config silinemez. DR Config'i silmeye çalışmadan önce eşleştirmeyi başarısız edin veya kırın. | [GeoDR çoğaltma](event-hubs-geo-dr.md) devam ediyor, bu nedenle config şu anda silinemez. | Config'in engelini kaldırmak için, çoğaltma tamamlanana kadar bekleyin, bir hata başlatmayı tetikler veya GeoDR eşleştirmesini kırın. |
| Çakışma | yok | Namespace güncelleştirmesi arka uçta çakışma ile başarısız oldu. | Başka bir işlem şu anda bu ad alanında yapılıyor. | Geçerli işlem tamamlanana kadar bekleyin ve sonra yeniden deneyin. |

## <a name="error-code-429"></a>Hata kodu: 429

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| 429 | yok | Geçişte ad alanı sağlama | Başka bir işlem şu anda bu ad alanında yapılıyor. | Geçerli işlem tamamlanana kadar bekleyin ve sonra yeniden deneyin. |
| 429 | yok | Olağanüstü durum kurtarma operasyonu devam ediyor. | Şu anda bu ad alanı veya eşleştirme üzerinde bir [GeoDR](event-hubs-geo-dr.md) işlemi yapılıyor. | Geçerli GeoDR işlemi tamamlanana kadar bekleyin ve sonra yeniden deneyin. |

## <a name="error-code-badrequest"></a>Hata kodu: BadRequest

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| BadRequest | 40000 | PartitionCount bir olay hub'ı için değiştirilemez. | Azure Etkinlik Hub'larının temel veya standart katmanı, bölüm değiştirmeyi desteklemez. | Temel veya standart katman ad alanınızda aranan bölüm sayısıyla yeni bir olay hub'ı oluşturun. Bölüm ölçeklendirme özel [kümeler](event-hubs-dedicated-overview.md)için desteklenir. |
| BadRequest | 40000 | MessageRetentionInDays için '#' değeri Temel katman için geçerli değildir. değeri '1' günü(ler) geçemez. | Temel katman Olay Hub'ları ad alanları yalnızca 1 güne kadar ileti bekletme yi destekler. | Bir günden fazla ileti bekletme isteniyorsa, [standart bir Olay Hub'ları ad alanı oluşturun.](event-hubs-create.md) | 
| BadRequest | yok | Belirtilen ad kullanılamıyor. | Ad alanı adları benzersiz olmalıdır ve belirtilen ad zaten alınır. | Belirtilen ada sahip varolan ad alanının sahibiyseniz, silinebilir ve bu da veri kaybına neden olur. Sonra, aynı adla yeniden deneyin. Ad alanı silinebilir güvenli değilse (veya sahibi siz değilseniz), başka bir ad alanı adı seçin. |
| BadRequest | yok | Belirtilen abonelik ad alanları kotasına ulaştı. | Aboneliğiniz, tutabileceği ad alanı sayısı için [kotaya](event-hubs-quotas.md) ulaştı. | Bu abonelikte kullanılmayan ad alanlarını silmeyi, başka bir abonelik oluşturmayı veya özel bir [kümeye](event-hubs-dedicated-overview.md)yükseltmeyi düşünün. |
| BadRequest | yok | İkincil bir ad alanını güncelleştiremezsin | [Bir GeoDR eşleştirmesinde](event-hubs-geo-dr.md)ikincil ad alanı olduğundan ad alanı güncelleştirilemez. | Uygunsa, bunun yerine bu eşleştirmede birincil ad alanında değişiklik yapın. Aksi takdirde değişiklik yapmak için GeoDR eşleştirmesini kırın. |
| BadRequest | yok | Temel SKU'da Otomatik Şişirme ayarlayamıyorum | Temel katman Olay Hub'ları ad alanlarında Otomatik Şişirme etkinleştirilenemez. | Ad alanında [Otomatik Şişirme'yi etkinleştirmek](event-hubs-auto-inflate.md) için standart katmanda olduğundan emin olun. |
| BadRequest | yok | Ad alanını oluşturmak için yeterli kapasite yok. Olay Hub'ları yöneticinize başvurun. | Seçili bölge kapasitede ve daha fazla ad alanı oluşturulamıyor. | Ad alanınızı barındıracak başka bir bölge seçin. |
| BadRequest | yok | Ad alanı 'ad alanı adı' 'Temel' katmanı kullandığından, işlem varlık türü 'ConsumerGroup' üzerinde yapılamaz.  | Temel katman Olay Hub'ları ad alanlarında bir [kota]((olay-hubs-kotalar.md#event-hubs-temel ve standart--- kotalar ve sınırları) bir tüketici grubunun (varsayılan). Daha fazla tüketici grubu oluşturmak desteklenmez. | Varsayılan tüketici grubunu ($Default) kullanmaya devam edin veya daha fazla sınagerekirse, bunun yerine standart bir katman Olay Hub'ları ad alanı kullanmayı düşünün. | 
| BadRequest | yok | Ad alanı 'ad alanı adı' yok. | Sağlanan ad alanı bulunamadı. | Ad alanı adının doğru olup olmadığını ve aboneliğinizde bulunup bulunamayabileceğini çift kez kontrol edin. Değilse, [olay hub'ları ad alanı oluşturun.](event-hubs-create.md) | 
| BadRequest | yok | Kaynağın konum özelliği, içerdiği Ad alanıyla eşleşmiyor. | Ad alanının bölgesiyle eşleşmediği için belirli bir bölgede olay hub'ı oluşturmak başarısız oldu. | Ad alanıyla aynı bölgede olay hub'ı oluşturmayı deneyin. | 

## <a name="error-code-internal-server-error"></a>Hata kodu: Dahili sunucu hatası

| Hata kodu | Hata alt kodu | Hata iletisi | Açıklama | Öneri |
| ---------- | ------------- | ------------- | ----------- | -------------- |
| Dahili Sunucu Hatası | yok | Dahili Sunucu Hatası. | Olay Hub'ları hizmetinde bir iç hata oluştu. | Başarısız işlemi yeniden deneyin. İşlem başarısız olmaya devam ederse desteğe başvurun. |