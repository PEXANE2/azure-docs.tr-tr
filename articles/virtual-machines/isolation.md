---
title: Azure 'da VM 'Ler için yalıtım
description: Azure 'da VM yalıtımının çalışması hakkında bilgi edinin.
author: ayshakeen
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 09/18/2019
ms.author: ayshak
ms.openlocfilehash: 7732f83dfd44cbf686e2d076c665c980962b19d5
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292355"
---
# <a name="virtual-machine-isolation-in-azure"></a>Azure 'da sanal makine yalıtımı

Azure Işlem, belirli bir donanım türüne yalıtılmış ve tek bir müşteriye adanmış sanal makine boyutları sunar. Yalıtılmış boyutlar canlı ve belirli donanım nesli üzerinde çalışır ve donanım oluşturma kullanımdan kaldırıldığında kullanım dışı kalır.

Yalıtılmış sanal makine boyutları, diğer müşterilerin iş yüklerinden, Toplantı uyumluluk ve mevzuat gereksinimlerini kapsayan nedenlerle yüksek derecede yalıtım gerektiren iş yükleri için idealdir.  Yalıtılmış bir boyut kullanılması, sanal makinenizin o belirli sunucu örneğinde çalışan tek bir tane olmasını garanti eder. 


Ayrıca, yalıtılmış boyut sanal makineleri büyük olduğu için müşteriler, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu VM 'lerin kaynaklarını alt bölümlere ayırarak seçebiliriz.

Geçerli yalıtılmış sanal makine teklifleri şunları içerir:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_F72s_v2

> [!NOTE]
> Yalıtılmış VM boyutları bir donanım sınırlı ömrü vardır. Ayrıntılar için lütfen aşağıya bakın

## <a name="deprecation-of-isolated-vm-sizes"></a>Yalıtılmış VM boyutlarının kullanımdan kaldırılması
Yalıtılmış VM boyutları, donanım bağlantılı boyutlardır, Azure, boyutların resmi kullanım dışı bırakılmasıyla birlikte 12 ay boyunca anımsatıcılar sağlar.  Ayrıca, Azure, müşterinin iş yüklerini üzerine taşımayı düşünebileceği bir sonraki donanım sürümümüzde güncelleştirilmiş bir yalıtılmış boyut sunar.

| Boyut | Yalıtım kullanımdan kaldırma tarihi | 
| --- | --- |
| Standard_DS15_v2<sup>1</sup> | 15 Mayıs 2020 |
| Standard_D15_v2<sup>1</sup>  | 15 Mayıs 2020 |

<sup>1</sup> Standard_DS15_v2 ve Standard_D15_v2 yalıtımı devre dışı bırakma programı hakkında ayrıntılar için bkz. SSS


## <a name="faq"></a>SSS
### <a name="q-is-the-size-going-to-get-retired-or-only-isolation-feature-is"></a>S: Bu boyut, kullanımdan kalkması veya yalnızca "yalıtım" özelliği mi olacak?
Y **: sanal**makine boyutunun "i" alt indisi yoksa yalnızca "yalıtım" özelliği devre dışı bırakılır. Yalıtım gerekmiyorsa, gerçekleştirilecek bir eylem yoktur ve VM beklendiği gibi çalışmaya devam eder. Örnekler şunlardır Standard_DS15_v2, Standard_D15_v2, Standard_M128ms vb. Sanal makine boyutu "i" alt indisi içeriyorsa, boyut kullanımdan kaldırılacak.

### <a name="q-is-there-a-downtime-when-my-vm-lands-on-a-non-isolated-hardware"></a>S: sanal makine, yalıtılmış olmayan bir donanımda yer aldığı zaman kapalı kalma süresi var mı?
**A**Y: yalıtıma gerek duyulmadığında hiçbir işlem yapılması gerekmez ve kapalı kalma süresi olmayacaktır.

### <a name="q-is-there-any-cost-delta-for-moving-to-a-non-isolated-virtual-machine"></a>S: yalıtılmış olmayan bir sanal makineye geçmek için herhangi bir maliyet Delta mı var?
Y **: Hayır**

### <a name="q-when-are-the-other-isolated-sizes-going-to-retire"></a>S: diğer yalıtılmış boyutlar ne zaman devre dışı bırakılacak?
Y **: yalıtılmış**boyutun resmi kullanım dışı bırakılmasının sonunda 12 ay boyunca anımsatıcılar sağlıyoruz.

### <a name="q-im-an-azure-service-fabric-customer-relying-on-the-silver-or-gold-durability-tiers-does-this-change-impact-me"></a>S: gümüş veya altın dayanıklılık katmanlarına bağlı bir Azure Service Fabric müşteriyim. Bu değişiklik beni etkiler mi?
Y **: Hayır**. Service Fabric [dayanıklılık katmanları](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster) tarafından belirtilen garantiler, bu değişiklikten sonra bile çalışmaya devam edecektir. Diğer nedenlerle fiziksel donanım yalıtımına ihtiyacınız varsa yukarıda açıklanan eylemlerden birini yapmanız gerekebilir. 
 
### <a name="q-what-are-the-milestones-for-d15_v2-or-ds15_v2-isolation-retirement"></a>S: D15_v2 veya DS15_v2 yalıtımı kullanımdan kaldırma için kilometre taşları nelerdir? 
**A**Y: 
 
| Tarih | Eylem |
|---|---| 
| 18 Kasım 2019 | D/DS15i_v2 (PAYG, 1 yıllık RI) kullanılabilirliği | 
| 14 Mayıs 2020 | DS15i_v2 1-yıl RI satın almak için geçen gün | 
| 15 Mayıs 2020 | D/DS15_v2 yalıtım garantisi kaldırıldı | 
| 15 Mayıs 2021 | Devre dışı bırakma D/DS15i_v2 (18 Kasım 2019 tarihinden önce 3 yıllık D/DS15_v2 dışında tüm müşteriler)| 
| 17 Kasım 2022 | 3 yıllık RIS tamamlandığında D/DS15i_v2 devre dışı bırak (18 Kasım 2019 tarihinden önce 3 yıllık bir D/DS15_v2 satın alan müşteriler için) |

## <a name="next-steps"></a>Sonraki adımlar

Müşteriler Ayrıca, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölümlere ayırmak da tercih edebilir.
