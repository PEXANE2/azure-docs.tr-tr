---
title: Azure Load Balancer yenilikleri
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Load Balancer yenilikleri öğrenin.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: overview
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: 4725aaaf7a2c1e0aa49ea8dbe046b720727a5d54
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86148056"
---
# <a name="whats-new-in-azure-load-balancer"></a>Azure Load Balancer yenilikler nelerdir?

Azure Load Balancer düzenli olarak güncelleştirilir. En son duyurularla güncel kalın. Bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev (varsa)

Ayrıca en son Azure Load Balancer güncelleştirmelerini bulabilir ve RSS akışına [buradan](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer)abone olabilirsiniz.

## <a name="new-features"></a>Yeni özellikler

|Özellik  |Açıklama  |Eklenme tarihi  |
|---------|---------|---------|
| IP tabanlı arka uç havuzu yönetimi desteği (Önizleme) | Azure Load Balancer, bir arka uç havuzundan IPv4 veya IPv6 adresleri aracılığıyla kaynak eklemeyi ve kaldırmayı destekler. Bu, kapsayıcıların, sanal makinelerin ve Load Balancer ilişkili sanal makine ölçek kümelerinin kolay yönetilmesini mümkün. Ayrıca, ilişkili kaynaklar oluşturulmadan önce, bir arka uç havuzunun parçası olarak IP adreslerinin ayrılması sağlanır. | Temmuz 2020 |
| Azure Izleyici kullanarak Öngörüler Azure Load Balancer | Ağlar için Azure Izleyici 'nin bir parçası olarak oluşturulan müşteriler artık tüm Load Balancer yapılandırmalarına ve sistem durumu panolarına ait, Azure portal ölçümleriyle önceden yapılandırılmış standart yük dengeleyiciler için topik haritalar sahibi vardır. [Başlayın ve daha fazla bilgi edinin](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Haziran 2020 |
| Azure Load Balancer için IPv6 desteği (genel kullanıma sunuldu) | Azure yük dengeleyiciler için ön uç olarak IPv6 adresleriniz olabilir. [Burada çift yığın uygulaması oluşturmayı](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) öğrenin |Nisan 2020|
| Boşta kalma zaman aşımı durumunda TCP sıfırlamaları (genel kullanıma sunuldu)| Daha öngörülebilir bir uygulama davranışı oluşturmak için TCP sıfırlamaları kullanın. [Daha fazla bilgi edinin](load-balancer-tcp-reset.md)| Şubat 2020 |

## <a name="next-steps"></a>Sonraki adımlar

Azure Load Balancer hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular](load-balancer-faqs.md).