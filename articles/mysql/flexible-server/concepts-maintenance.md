---
title: Zamanlanan Bakım-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, MySQL için Azure veritabanı-esnek sunucu 'da zamanlanmış bakım özelliği açıklanır.
author: niklarin
ms.author: nlarin
ms.service: mysql
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: a2e99440a7c8f33eee9d3c9fe2276ac3868ff4b6
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91331769"
---
# <a name="scheduled-maintenance-in-azure-database-for-mysql--flexible-server"></a>MySQL için Azure Veritabanı Esnek Sunucusunda zamanlanmış bakım

MySQL için Azure veritabanı-esnek sunucu, yönetilen veritabanınızı güvenli, kararlı ve güncel tutmak için düzenli bakım gerçekleştirir. Bakım sırasında sunucu yeni özellikler, güncelleştirmeler ve düzeltme ekleri alır.

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu önizlemededir.

## <a name="select-a-maintenance-window"></a>Bakım penceresi seçin

Belirli bir gün boyunca ve o gün içinde bir zaman penceresi için bakım zamanlayabilirsiniz. Ya da sistemin sizin yerinize otomatik olarak bir gün ve zaman penceresi seçmesini sağlayabilirsiniz. Her iki durumda da, sistem herhangi bir bakım çalıştırılmadan önce beş gün sonra uyarır. Sistem ayrıca bakım başladığında ve başarıyla tamamlandığında size bilgi verir.

Yaklaşan zamanlanmış bakım bildirimleri şu olabilir:

* Belirli bir adrese e-postayla
* Azure Resource Manager rolüne e-postayla
* Mobil cihazlara kısa mesajda (SMS) gönderilir
* Azure uygulamasına bildirim olarak gönderilerek
* Sesli mesaj olarak teslim edilerek

Bakım zamanlamasıyla ilgili tercihleri belirtirken haftanın gününü ve zaman aralığını belirleyebilirsiniz. Herhangi bir değer belirtmezseniz sistem, sunucunuzun bulunduğu bölgenin saatine göre akşam 11 ile sabah 7 arasında bir zaman seçer. Azure aboneliğinizdeki her esnek sunucu için farklı zamanlamalar tanımlayabilirsiniz.

> [!IMPORTANT]
> Normalde bir sunucuda yapılacak başarılı zamanlanmış bakım olayları en az 30 gün önceden bildirilir.
>
> Ancak ciddi bir güvenlik açığı gibi kritik öneme sahip acil bir güncelleştirme söz konusu olduğunda beş günden daha kısa bir süre öncesinde bildirim yapılabilir. Son 30 gün içinde başarılı bir zamanlanmış bakım gerçekleştirilmiş olsa dahi sunucunuza kritik güncelleştirme uygulanabilir.

Zamanlama ayarlarını dilediğiniz zaman güncelleştirebilirsiniz. Esnek sunucunuz için zamanlanan bir bakım varsa ve zamanlama tercihlerini güncelleştirirseniz, geçerli olay zamanlandığı şekilde devam eder ve zamanlama ayarları değişikliği başarıyla tamamlandığında etkili olur.

Nadir durumlarda, bakım olayı sistem tarafından iptal edilebilir veya başarıyla tamamlanmayabilir. Bu durumda, sistem sırasıyla iptal edilen veya başarısız bakım olayı hakkında bir bildirim oluşturur. Bakım gerçekleştirme girişimi, geçerli zamanlama ayarlarına göre zamanlanır ve bu işlem için beş gün önceden bildirim alırsınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Bakım zamanlamasını değiştirme](how-to-maintenance-portal.md) hakkında bilgi edinin
* Azure hizmet durumunu kullanarak [gelecek bakımla ilgili bildirim alma](../../service-health/service-notifications.md) hakkında bilgi edinin
* [Yaklaşan zamanlanmış bakım olayları hakkında uyarı ayarlamayı](../../service-health/resource-health-alert-monitor-guide.md) öğrenin
