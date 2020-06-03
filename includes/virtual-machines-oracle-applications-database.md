---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/01/2020
ms.author: danlep
ms.openlocfilehash: b9e275cbdaf264df3d0a40f032693fd93553e520
ms.sourcegitcommit: 69156ae3c1e22cc570dda7f7234145c8226cc162
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84317459"
---
### <a name="database-tier"></a>Veritabanı katmanı

Veritabanı katmanı, uygulamanın veritabanı örneklerini içerir. Veritabanı bir Oracle DB, Oracle RAC veya Oracle sınavı veri veritabanı sistemi olabilir. 

Oracle DB kullanmak tercih ediyorsanız, veritabanı örneği Azure Market 'te bulunan Oracle DB görüntüleri aracılığıyla Azure 'da dağıtılabilir. Alternatif olarak, Oracle DB OCı üzerinde bir PaaS modelinde dağıtmak için Azure ile OCı arasındaki Interconnect 'i de kullanabilirsiniz.

Oracle RAC için PaaS modelinde OCı kullanabilirsiniz. İki düğümlü bir RAC sistemi kullanmanız önerilir. IaaS modelinde Azure CloudSimple üzerinde Oracle RAC dağıtmak mümkün olsa da, Oracle tarafından desteklenen bir yapılandırma değildir. [Yetkili bulut ortamları için uygun Oracle programları](http://www.oracle.com/us/corporate/pricing/authorized-cloud-environments-3493562.pdf)bölümüne bakın.

Son olarak, sınava veri sistemleri için OCı bağlantısını kullanın ve bu verileri OCı 'da dağıtın. Yukarıdaki önceki mimari diyagramda, iki alt ağ arasında OCı halinde dağıtılan bir sınava veri sistemi gösterilmektedir.

Üretim senaryolarında, iki kullanılabilirlik bölgesinde (Azure 'da dağıtım yapıyorsanız) veya iki kullanılabilirlik etki alanı (OCı) üzerinde veritabanının birden çok örneğini dağıtın. Birincil ve hazır bekleyen veritabanlarını eşleştirmek için Oracle Active Data Guard 'ı kullanın.

Veritabanı katmanı yalnızca orta katmandan istekleri alır. Bir ağ güvenlik grubu ayarlamanız önerilir (veritabanını OCı 'da dağıtırsanız), yönetim nedenlerinden dolayı yalnızca orta katmandan ve bağlantı noktası 22 numaralı bağlantı noktası için bağlantı noktası 1521 ' deki isteklere izin vermek üzere gereklidir.

OCı 'da dağıtılan veritabanları için, FastConnect Devrenize bağlı bir dinamik yönlendirme ağ geçidi (DRG) ile ayrı bir sanal bulut ağı ayarlanmalıdır.