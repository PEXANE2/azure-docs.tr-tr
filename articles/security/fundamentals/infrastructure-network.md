---
title: Azure ağ mimarisi
description: Bu makale, Microsoft Azure altyapı ağının genel bir açıklamasını sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: terrylan
ms.openlocfilehash: 3b047489f9cfa3623c11e324cf58114b707c10b7
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567885"
---
# <a name="azure-network-architecture"></a>Azure ağ mimarisi
Azure ağ mimarisi, Internet 'ten Azure veri merkezlerine bağlantı sağlar. Azure 'da dağıtılan tüm iş yükleri (IaaS, PaaS ve SaaS) Azure veri merkezi ağını kullanarak.

## <a name="network-topology"></a>Ağ topolojisi
Bir Azure veri merkezinin ağ mimarisi aşağıdaki bileşenlerden oluşur:

- Edge ağı
- Geniş alan ağı
- Bölgesel ağ geçitleri ağı
- Veri merkezi ağı

![Azure ağ diyagramı](./media/infrastructure-network/network-arch.png)

## <a name="network-components"></a>Ağ bileşenleri
Ağ bileşenlerinin kısa bir açıklaması.

- Edge ağı

   - Microsoft ağı ve diğer ağlar arasında (örneğin, Internet, kurumsal ağ) bir kaldırma noktası
   - Azure 'a Internet ve [ExpressRoute](../../expressroute/expressroute-introduction.md) eşlemesi sağlar

- Geniş alan ağı

   - Dünya çapında Microsoft Akıllı omurga ağı
   - [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/geographies/) arasında bağlantı sağlar

- Bölgesel ağ geçidi

   - Bir Azure bölgesindeki tüm veri merkezlerinin toplama noktası
   - Bir Azure bölgesi içindeki veri merkezleri arasında büyük bir bağlantı sağlar (örneğin, veri merkezi başına çok yüz terabayt)

- Veri merkezi ağı

   - Düşük abone olunan bant genişliğine sahip veri merkezi içindeki sunucular arasında bağlantı sağlar

Yukarıdaki ağ bileşenleri, her zaman açık, her zaman kullanılabilir bulut işletmelerini desteklemek için maksimum kullanılabilirliği sağlamak üzere tasarlanmıştır. Artıklık, en çok denetim protokolüne kadar olan fiziksel açıdan ağda tasarlanır ve yerleşik olarak bulunur.

## <a name="datacenter-network-resiliency"></a>Veri merkezi ağ dayanıklılığı
Veri merkezi ağını kullanarak dayanıklılık tasarımı ilkesini görelim.

Veri merkezi ağı, bulut ölçeği trafiği için yüksek çift yönlü bant genişliği sağlayan bir [Clos ağının](https://en.wikipedia.org/wiki/Clos_network)değiştirilmiş sürümüdür. Ağ, tek bir donanım arızası nedeniyle oluşan etkiyi azaltmak için çok sayıda emtia cihazı kullanılarak oluşturulur. Bu cihazlar, bir ortam olayının etkisini azaltmak için ayrı güç ve soğutma etki alanı ile farklı fiziksel konumlarda stratejik olarak bulunur.  Denetim düzleminde tüm ağ aygıtları, trafik döngüsünün geçmiş sorununu ortadan kaldıran OSı model katman 3 yönlendirme modu olarak çalışır. Farklı katmanlar arasındaki tüm yollar, eşit maliyetli çoklu yol (ECMP) yönlendirmesi kullanılarak yüksek artıklık ve bant genişliği sağlamak için etkindir.

Aşağıdaki diyagramda, veri merkezi ağının farklı ağ aygıtı katmanları tarafından oluşturulduğunu gösterilmektedir. Diyagramdaki çubuklar yedeklilik ve yüksek bant genişliği bağlantısı sağlayan ağ cihazı gruplarını temsil eder.

![Veri merkezi ağı](./media/infrastructure-network/datacenter-network.png)

## <a name="next-steps"></a>Sonraki adımlar
Microsoft 'un Azure altyapısını güvenli hale getirmeye yardımcı olma hakkında daha fazla bilgi edinmek için bkz.:

- [Azure tesisler, şirket içi ve fiziksel güvenlik](physical-security.md)
- [Azure altyapı kullanılabilirliği](infrastructure-availability.md)
- [Azure Information System bileşenleri ve sınırları](infrastructure-components.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri verileri koruması](protection-customer-data.md)
