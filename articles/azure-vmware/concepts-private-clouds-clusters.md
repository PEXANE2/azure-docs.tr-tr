---
title: Kavramlar-özel bulutlar ve kümeler
description: Azure VMware çözümü yazılım tanımlı veri merkezlerinin ve vSphere kümelerinin temel özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 10/27/2020
ms.openlocfilehash: 234e705ad3b39a575c6f8c475bede4b81efa4b96
ms.sourcegitcommit: 445ecb22233b75a829d0fcf1c9501ada2a4bdfa3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99474922"
---
#  <a name="azure-vmware-solution-private-cloud-and-cluster-concepts"></a>Azure VMware çözümü özel bulutu ve küme kavramları

Azure VMware çözümü, Azure 'da VMware tabanlı özel bulutlar sunar. Özel bulutlar adanmış ve çıplak Azure Konakları ile oluşturulmuş kümeler içerir. Azure portal, CLı veya PowerShell aracılığıyla dağıtılır ve yönetilir.  Özel bulutlarda sağlanan kümeler şunlardır VMware vSphere, vCenter, vSAN ve NSX yazılımı. Azure VMware çözümü özel bulut donanımı ve yazılım dağıtımları, Azure 'da tamamen tümleşiktir ve otomatikleştirilir.

Azure abonelikleri, Azure VMware çözümü özel bulutlar, vSAN kümeleri ve konaklar arasında mantıksal bir ilişki vardır. Diyagramda, geliştirme ve üretim ortamını temsil eden iki özel bulutla tek bir Azure aboneliği gösterilmektedir.  Bu özel bulutların her birinde iki kümeniz vardır. 

Bu makalede bu kavramların hepsi açıklanmaktadır.

![Bir müşteri aboneliğinde iki özel bulutun görüntüsü](./media/hosts-clusters-private-clouds-final.png)

>[!NOTE]
>Geliştirme ortamının en düşük olası ihtiyaçları nedeniyle, daha düşük kapasite Konakları olan daha küçük kümeler kullanın. 

## <a name="private-clouds"></a>Özel bulutlar

Özel bulutlar adanmış ve çıplak Azure Konakları ile oluşturulmuş vSAN kümelerini içerir. Her özel bulutun aynı vCenter Server ve NSX-T Yöneticisi tarafından yönetilen birden çok kümesi olabilir. Portal, CLı veya PowerShell 'de özel bulutlar dağıtabilir ve yönetebilirsiniz. 

Diğer kaynaklarda olduğu gibi, özel bulutlar bir Azure aboneliği içinden yüklenir ve yönetilir. Bir abonelik içindeki özel bulutlar sayısı ölçeklenebilir. Başlangıçta, her abonelik için bir özel bulutun sınırı vardır.

## <a name="clusters"></a>Kümeler
Oluşturulan her bir özel bulut için varsayılan olarak bir vSAN kümesi vardır. Azure portal kullanarak veya API aracılığıyla kümeleri ekleyebilir, silebilir ve ölçeklendirebilirsiniz.  Tüm kümelerin varsayılan üç ana bilgisayar boyutu vardır ve 16 ' ya kadar ana bilgisayar ölçeklendirebilirler.  Bir kümede kullanılan konaklar aynı ana bilgisayar türünde olmalıdır.

Deneme kümeleri değerlendirme için kullanılabilir ve üç ana bilgisayar ile sınırlıdır. Özel bulut başına tek bir deneme kümesi vardır. Değerlendirme süresi boyunca, deneme kümesini tek bir ana bilgisayar ile ölçeklendirebilirsiniz.

Küme yapılandırmasının veya işlemin diğer yönlerini yönetmek için vSphere ve NSX-T yöneticisini kullanırsınız. Bir kümedeki her konağın yerel depolama alanı, vSAN denetimi altındadır.

## <a name="hosts"></a>Ana bilgisayarlar

Azure VMware çözümü özel bulut kümeleri hiper yakınsama, çıplak altyapı Konakları kullanır. Aşağıdaki tabloda konağın RAM, CPU ve disk kapasiteleri gösterilmektedir. 

| Ana Bilgisayar Türü              |             CPU             |   RAM (GB)   |  vSAN NVMe önbellek katmanı (TB, RAW)  |  vSAN SSD kapasite katmanı (TB, RAW)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| High-End (BT)          |  Çift Intel 18 çekirdek 2,3 GHz  |     576      |                3.2               |                15,20               |

Kümeleri derlemek veya ölçeklendirmek için kullanılan konaklar, yalıtılmış bir konaklar havuzundan gelir. Bu konaklar donanım testlerini geçti ve tüm verileri güvenli bir şekilde sildi. 

## <a name="vmware-software-versions"></a>VMware yazılım sürümleri

[!INCLUDE [vmware-software-versions](includes/vmware-software-versions.md)]


## <a name="host-maintenance-and-lifecycle-management"></a>Konak bakımı ve yaşam döngüsü yönetimi

Konak bakımı ve yaşam döngüsü yönetimi, özel bulut kümelerinin kapasitesini veya performansını etkilemez.  Otomatik ana bilgisayar bakımı örnekleri, bellenim yükseltmeleri ve donanım onarımı ya da değişikliği içerir.

Microsoft, NSX-T Manager ve NSX-T Edge gibi NSX-T gereçlerinin yaşam döngüsü yönetiminden sorumludur. Ayrıca, katman-0 ağ geçidini oluşturma ve North-South yönlendirmeyi etkinleştirme gibi önyükleme ağ yapılandırmasından de sorumludur. NSX-T SDN yapılandırmasından sorumlu olursunuz. Örneğin, ağ kesimleri, dağıtılmış güvenlik duvarı kuralları, katman 1 ağ geçitleri ve yük dengeleyiciler.

> [!IMPORTANT]
> NSX-T Edge veya Katman-0 ağ geçidinin yapılandırmasını değiştirmeyin, çünkü bu hizmet kaybına neden olabilir.

## <a name="backup-and-restoration"></a>Yedekleme ve geri yükleme

Özel bulut vCenter ve NSX-T yapılandırmalarının saatlik bir yedekleme zamanlaması vardır.  Yedeklemeler üç gün boyunca tutulur. Bir yedekten geri yüklemeniz gerekiyorsa, geri yükleme istemek için Azure portal bir [destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support) açın.

## <a name="next-steps"></a>Sonraki adımlar

Bu Azure VMware çözümü özel bulut kavramlarını kapsadığınıza göre, şunları öğrenmek isteyebilirsiniz: 

- [Azure VMware Çözüm ağı ve karşılıklı bağlantı kavramları](concepts-networking.md).
- [Azure VMware Çözüm depolama kavramları](concepts-storage.md).

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

