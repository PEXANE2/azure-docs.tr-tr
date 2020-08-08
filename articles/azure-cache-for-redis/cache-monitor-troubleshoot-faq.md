---
title: Redsıs izleme ve sorun giderme için Azure önbelleği
description: Redsıs için Azure önbelleğini izlemenize ve sorunlarını gidermenize yardımcı olacak yaygın soruların yanıtlarını öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/06/2020
ms.openlocfilehash: 2149d069ce9cb0b636e461d19963f413b162bc9d
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88010897"
---
# <a name="azure-cache-for-redis-monitoring-and-troubleshooting-faqs"></a>Redsıs izleme ve sorun giderme için Azure önbelleği
Bu makalede redin için Azure önbelleğini izleme ve sorunlarını giderme hakkında sık sorulan soruların yanıtları sunulmaktadır.

## <a name="common-questions-and-answers"></a>Yaygın sorular ve yanıtları
Bu bölümde aşağıdaki SSS ele alınmaktadır:

* [Nasıl yaparım? önbelleğim sistem durumunu ve performansını izlemek mi?](#how-do-i-monitor-the-health-and-performance-of-my-cache)
* [Neden zaman aşımlarını görüyorum?](#why-am-i-seeing-timeouts)
* [İstemcimin neden önbellekten bağlantısı kesildi?](#why-was-my-client-disconnected-from-the-cache)

### <a name="how-do-i-monitor-the-health-and-performance-of-my-cache"></a>Nasıl yaparım? önbelleğim sistem durumunu ve performansını izlemek mi?
Redsıs örnekleri için Microsoft Azure önbelleği [Azure Portal](https://portal.azure.com)izlenebilir. Ölçümleri görüntüleyebilir, ölçüm grafiklerini başlangıç panosuna sabitleyebilir, izleme grafiklerinin tarih ve saat aralığını özelleştirebilir, grafiklerden ölçümleri ekleyebilir ve kaldırabilir ve belirli koşullar karşılandığında uyarılar ayarlayabilirsiniz. Daha fazla bilgi için bkz. [redsıs Için Azure önbelleğini izleme](cache-how-to-monitor.md).

Redsıs **Kaynak menüsü** Için Azure önbelleği, önbelleğinizi izlemek ve sorunlarını gidermek için çeşitli araçlar içerir.

* **Sorunları tanılama ve** çözme, yaygın sorunlar ve bunları çözmeye yönelik stratejiler hakkında bilgi sağlar.
* **Kaynak sistem durumu** , kaynağınızı izler ve beklendiği gibi çalışıp çalışmadığını bildirir. Azure Kaynak sistem durumu hizmeti hakkında daha fazla bilgi için bkz. [Azure Kaynak durumu genel bakış](../resource-health/resource-health-overview.md).
* **Yeni destek isteği** önbelleğiniz için bir destek isteği açma seçenekleri sağlar.

Bu araçlar, Redsıs örnekleri için Azure önbelleğinizin durumunu izlemenize ve önbelleğe alma uygulamalarınızı yönetmenize yardımcı olur. Daha fazla bilgi için [Azure önbelleğini redsıs için yapılandırma](cache-configure.md)konusunun "destek & sorun giderme ayarları" bölümüne bakın.

### <a name="why-am-i-seeing-timeouts"></a>Neden zaman aşımlarını görüyorum?
Redsıs ile konuşmak için kullandığınız istemcide zaman aşımları meydana gelir. Redsıs sunucusuna bir komut gönderildiğinde, komut kuyruğa alınır ve Reddir sunucusu, sonunda komutu seçer ve yürütür. Ancak, bu işlem sırasında istemci zaman aşımına uğrar ve arama tarafında bir özel durum ortaya çıkar. Zaman aşımı sorunlarını giderme hakkında daha fazla bilgi için bkz. [istemci tarafı sorun giderme](cache-troubleshoot-client.md) ve [StackExchange. redsıs zaman aşımı özel durumları](cache-troubleshoot-timeouts.md#stackexchangeredis-timeout-exceptions).

### <a name="why-was-my-client-disconnected-from-the-cache"></a>İstemcimin neden önbellekten bağlantısı kesildi?
Önbellek bağlantısının kesilmesi için bazı yaygın nedenler aşağıda verilmiştir.

* İstemci tarafı nedenleri
  * İstemci uygulama yeniden dağıtıldı.
  * İstemci uygulaması bir ölçeklendirme işlemi gerçekleştirdi.
    * Cloud Services veya Web Apps durumunda bu, otomatik ölçeklendirme nedeniyle olabilir.
  * İstemci tarafındaki ağ katmanı değişti.
  * İstemcide veya istemci ile sunucu arasındaki ağ düğümlerinde geçici hatalar oluştu.
  * Bant genişliği eşik sınırlarına ulaşıldı.
  * CPU bağlantılı işlemlerin tamamlanması çok uzun sürdü.
* Sunucu tarafı nedenleri
  * Standart önbellek sunumunda, Redsıs hizmeti için Azure önbelleği, birincil düğümden çoğaltma düğümüne yük devretme işlemi başlattı.
  * Azure, önbelleğin dağıtıldığı örneğe düzeltme eki eklendi
    * Bu, Redsıs sunucu güncelleştirmeleri veya genel VM bakımı için olabilir.


## <a name="next-steps"></a>Sonraki adımlar

Redsıs örnekleri için Azure önbelleğinizi izleme ve sorun giderme hakkında daha fazla bilgi için bkz. [Reda Için Azure önbelleğini izleme](cache-how-to-monitor.md) ve çeşitli sorun giderme kılavuzu.

[Redsıs SSS için diğer Azure önbelleği](cache-faq.md)hakkında bilgi edinin.
