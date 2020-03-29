---
title: İş sürekliliğine genel bakış
titleSuffix: Microsoft Genomics
description: Bu genel bakış, Microsoft Genomics'in iş sürekliliği ve olağanüstü durum kurtarma için sağladığı yetenekleri açıklar.
keywords: iş sürekliliği, felaket kurtarma
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: conceptual
ms.date: 04/06/2018
ms.openlocfilehash: 28a4a53851155c56e8d34981862bf52a3a2cf15b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72249186"
---
# <a name="overview-of-business-continuity-with-microsoft-genomics"></a>Microsoft Genomics ile iş sürekliliğine genel bakış
Bu genel bakış, Microsoft Genomics'in iş sürekliliği ve olağanüstü durum kurtarma için sağladığı yetenekleri açıklar. Veri kaybına neden olabilecek Bir Azure bölgesi kesintisi gibi yıkıcı olaylardan kurtarma seçenekleri hakkında bilgi edinin. 


## <a name="microsoft-genomics-features-that-support-business-continuity"></a>Microsoft Genomics iş sürekliliğini destekleyen özellikler 
Nadir olsa da, bir Azure veri merkezinin kesintisi olabilir ve bu da birkaç dakika ile birkaç saat arasında sürebilen bir iş kesintisine neden olabilir. Bir kesinti oluştuğunda, veri merkezinde çalışmakta olan tüm işler başarısız olur ve Microsoft Genomics hizmeti işleri otomatik olarak ikincil bir bölgeye yeniden göndermez. 

* Bir seçenek, veri merkezi kesintisi sona erdiğinde veri merkezinin tekrar çevrimiçi olmasını beklemektir. Kesinti kısaysa, Microsoft Genomics hizmeti başarısız işleri otomatik olarak algılar ve iş akışı otomatik olarak yeniden başlatılır.

* Başka bir seçenek, iş akışını başka bir veri bölgesinde proaktif olarak göndermektir. Microsoft Genomics örnekleri çeşitli [Azure bölgelerinde](https://azure.microsoft.com/regions/services/)dağıtır ve her bölgeye özgü örnek bağımsızdır. Microsoft Genomik örneklerinden biri bir bölgenin yerel başarısızlığıyla karşılaşırsa, Microsoft Genomics örneklerini çalıştıran diğer bölgeler işleri işlemeye devam eder. Alternatif bir bölgeye bu aktarım kullanıcının denetimi altındadır ve her zaman kullanılabilir.


### <a name="manually-failover-microsoft-genomics-workflows-to-another-region"></a>Microsoft Genomics iş akışları nın başka bir bölgeye el ile başarısız olması
Bölgesel bir veri merkezi kesintisi oluşursa, kişisel veri egemenliği ve iş sürekliliği gereksinimlerinize bağlı olarak ikincil bir bölgede Microsoft Genomics iş akışlarını göndermeyi seçebilirsiniz. Microsoft Genomics iş akışlarını el ile başarısız olmak için, bölgeye özgü farklı bir bölge kullanırsınız. Genomik hesap ve uygun bölgeye özgü Genomik ve depolama hesabı kimlik bilgileri ile iş gönderin.

Özellikle şunları yapmanız gerekir:
* Azure portalını kullanarak ikincil bölgede bir Genomik hesabı oluşturun. 
* Giriş verilerinizi ikincil bölgedeki bir depolama hesabına geçirin ve ikincil bölgede bir çıktı klasörü ayarlayın.
* İkincil bölgedeki iş akışını gönderin.

Özgün bölge geri yüklendiğinde, Microsoft Genomics hizmeti ikincil bölgeden gelen verileri özgün bölgeye geri yüklemez. Giriş ve çıktı dosyalarını ikincil bölgeden özgün bölgeye taşımayı seçebilirsiniz.  Eğer verilerini taşımayı seçerseniz, bu Genomik hizmetinin dışındadır ve veri hareketi ile ilgili tüm masraflar sizin sorumluluğunuzda olacaktır. 

### <a name="preparing-for-a-possible-region-specific-outage"></a>Olası bir bölgeye özel kesintiye hazırlık
Veri merkezi kesintisi durumunda daha hızlı iyileşme konusunda endişeleriniz varsa, Microsoft Genomik iş akışlarınızı ikincil bir bölgeye el ile yeniden göndermeniz için gereken süreyi azaltmak için birkaç adım atabilirsiniz:

* Uygun bir ikincil bölgeyi belirleyin ve o bölgede pro-aktif olarak bir Genomik hesabı oluşturun
* Verilerinizin ikincil bölgede hemen kullanılabilsin diye verilerinizi birincil ve ikincil bölgede çoğaltın. Bu, el ile veya Azure depolama alanında bulunan [coğrafi yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy) özelliği kullanılarak yapılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Microsoft Genomics hizmetini kullanırken iş sürekliliği ve olağanüstü durum kurtarma seçenekleriniz hakkında bilgi edinilmişsinizdir. Azure'da iş sürekliliği ve genel durum kurtarma hakkında daha fazla bilgi için [Azure esnekliği teknik kılavuzuna bakın.](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region) 