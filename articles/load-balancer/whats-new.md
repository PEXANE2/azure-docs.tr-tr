---
title: Azure Load Balancer yenilikleri
description: En son sürüm notları, bilinen sorunlar, hata düzeltmeleri, kullanım dışı işlevler ve yaklaşan değişiklikler gibi Azure Load Balancer yenilikleri öğrenin.
services: load-balancer
author: anavinahar
ms.service: load-balancer
ms.topic: overview
ms.date: 07/07/2020
ms.author: anavin
ms.openlocfilehash: 8b44dc230dbee1b29b9889a1b81e35ebe25f6b97
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87078680"
---
# <a name="whats-new-in-azure-load-balancer"></a>Azure Load Balancer yenilikler nelerdir?

Azure Load Balancer düzenli olarak güncelleştirilir. En son duyurularla güncel kalın. Bu makalede hakkında bilgi verilmektedir:

- En son yayınlar
- Bilinen sorunlar
- Hata düzeltmeleri
- Kullanım dışı işlev (varsa)

Ayrıca en son Azure Load Balancer güncelleştirmelerini bulabilir ve RSS akışına [buradan](https://azure.microsoft.com/updates/?category=networking&query=load%20balancer)abone olabilirsiniz.

## <a name="recent-releases"></a>Son yayınlar

| Tür |Ad |Açıklama  |Eklenme tarihi  |
| ------ |---------|---------|---------|
| Özellik | IP tabanlı arka uç havuzu yönetimi desteği (Önizleme) | Azure Load Balancer, bir arka uç havuzundan IPv4 veya IPv6 adresleri aracılığıyla kaynak eklemeyi ve kaldırmayı destekler. Bu, kapsayıcıların, sanal makinelerin ve Load Balancer ilişkili sanal makine ölçek kümelerinin kolay yönetilmesini mümkün. Ayrıca, ilişkili kaynaklar oluşturulmadan önce, bir arka uç havuzunun parçası olarak IP adreslerinin ayrılması sağlanır. [Daha fazla bilgi edinin](backend-pool-management.md)|Temmuz 2020 |
| Özellik| Azure Izleyici kullanarak Öngörüler Azure Load Balancer | Ağlar için Azure Izleyici 'nin bir parçası olarak oluşturulan müşteriler artık tüm Load Balancer yapılandırmalarına ve sistem durumu panolarına ait, Azure portal ölçümleriyle önceden yapılandırılmış standart yük dengeleyiciler için topik haritalar sahibi vardır. [Başlayın ve daha fazla bilgi edinin](https://azure.microsoft.com/blog/introducing-azure-load-balancer-insights-using-azure-monitor-for-networks/) | Haziran 2020 |
| Doğrulama | HA bağlantı noktaları için doğrulama ekleme | HA bağlantı noktası kurallarının ve HA olmayan bağlantı noktası kurallarının yalnızca kayan IP etkin olduğunda yapılandırılamasından emin olmak için bir doğrulama eklenmiştir. Daha önce bu yapılandırma, istendiği gibi çalışmaz, ancak işe devam etmez. İşlev değişikliği yapılmadı. [Buradan](load-balancer-ha-ports-overview.md#limitations) daha fazla bilgi edinebilirsiniz| Haziran 2020 |
| Özellik| Azure Load Balancer için IPv6 desteği (genel kullanıma sunuldu) | Azure yük dengeleyiciler için ön uç olarak IPv6 adresleriniz olabilir. [Burada çift yığın uygulaması oluşturmayı](../virtual-network/virtual-network-ipv4-ipv6-dual-stack-standard-load-balancer-powershell.md) öğrenin |Nisan 2020|
| Özellik| Boşta kalma zaman aşımı durumunda TCP sıfırlamaları (genel kullanıma sunuldu)| Daha öngörülebilir bir uygulama davranışı oluşturmak için TCP sıfırlamaları kullanın. [Daha fazla bilgi edinin](load-balancer-tcp-reset.md)| Şubat 2020 |

## <a name="next-steps"></a>Sonraki adımlar

Azure Load Balancer hakkında daha fazla bilgi için bkz. [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular](load-balancer-faqs.md).
