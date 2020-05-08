---
title: Kavramlar-özel bulutlar ve kümeler
description: VMware tarafından Azure 'da VMware çözümünde Azure VMware yazılım tanımlı veri merkezlerinin ve vSphere kümelerinin temel özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: f51b76b654a43d690aa5c97fa9df99fad1f47d7a
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82740388"
---
# <a name="azure-vmware-solution-avs-preview-private-cloud-and-cluster-concepts"></a>Azure VMware çözümü (AVS) önizleme özel bulut ve küme kavramları

Azure VMware çözümü (AVS), Azure 'da VMware tabanlı özel bulutlar sunar. Özel bulutlar adanmış çıplak konaklar kümelerinden oluşturulmuştur ve Azure portal aracılığıyla dağıtılır ve yönetilir. Özel bulutlarda kümeler VMware vSphere, vCenter, vSAN ve NSX yazılımıyla birlikte sağlanır. AVS özel bulut donanımı ve yazılım dağıtımları, Azure 'da tamamen tümleşiktir ve otomatikleştirilir.

Azure abonelikleri, AVS özel bulutlar, vSAN kümeleri ve konaklar arasında mantıksal bir ilişki vardır. Diyagramda, tek bir Azure aboneliğindeki iki özel bulut gösterilir. Özel bulutlar, her biri kendi özel bulutuyla birlikte bir geliştirme ve üretim ortamını temsil eder. Bu özel bulutların her birinde iki küme vardır. Bir geliştirme ortamının olası en düşük ihtiyaçlarını göstermek için, daha düşük kapasite Konakları olan küçük kümeler kullanılır. Bu kavramların tümü aşağıdaki bölümlerde açıklanmıştır.

![Bir müşteri aboneliğinde iki özel bulutun görüntüsü](./media/hosts-clusters-private-clouds-final.png)

## <a name="private-clouds"></a>Özel bulutlar

Özel bulutlar, adanmış ve çıplak Azure Konakları ile oluşturulmuş vSAN kümelerini içerir. Her özel bulutun, hepsi aynı vCenter Server ve NSX-T Manager tarafından yönetilen birden çok kümesi olabilir. Portalda, CLı 'dan veya PowerShell ile özel bulutlar dağıtabilir ve yönetebilirsiniz. Diğer kaynaklarda olduğu gibi, özel bulutlar bir Azure aboneliği içinden yüklenir ve yönetilir.

Bir abonelik içindeki özel bulutlar sayısı ölçeklenebilir. Başlangıçta, her abonelik için bir özel bulutun sınırı vardır.

## <a name="clusters"></a>Kümeler

Her bir özel bulutta en az bir vSAN kümesi oluşturacaksınız. Özel bir bulut oluşturduğunuzda, varsayılan olarak bir küme vardır. Azure portal veya API aracılığıyla özel bir buluta ek kümeler ekleyebilirsiniz. Tüm kümelerin varsayılan üç ana bilgisayar boyutu vardır ve 3 ile 16 ana bilgisayar arasında ölçeklendirilebilir. Bir kümede kullanılan ana bilgisayarların türü aynı türde olmalıdır. Konaklar türleri sonraki bölümde açıklanmaktadır.

Deneme kümeleri, değerlendirme için kullanılabilir ve özel bulut başına üç ana bilgisayar ve tek bir deneme kümesiyle sınırlandırılmıştır. Değerlendirme süresi boyunca, deneme kümesini tek bir ana bilgisayar ile ölçeklendirebilirsiniz.

Portal veya API aracılığıyla küme oluşturur, siler ve ölçeklendirebilirsiniz. Küme yapılandırmasının veya işlemin diğer yönlerini yönetmek için vSphere ve NSX-T Yöneticisi 'Ni kullanmaya devam edersiniz. Bir kümedeki her konağın yerel depolama alanı, vSAN denetimi altındadır.

## <a name="hosts"></a>Ana bilgisayarlar

Hiper yakınsama, çıplak altyapı düğümleri, AVS özel bulut kümelerinde kullanılır. Konağın RAM, CPU ve disk kapasiteleri aşağıdaki tabloda verilmiştir. 

| Ana Bilgisayar Türü              |             CPU             |   RAM (GB)   |  vSAN NVMe önbellek katmanı (TB, RAW)  |  vSAN SSD kapasite katmanı (TB, RAW)  |
| :---                   |            :---:            |    :---:     |               :---:              |                :---:               |
| Yüksek uç (BT)          |  Çift Intel 18 çekirdek 2,3 GHz  |     576      |                3,2               |                15,20               |

Kümeleri derlemek veya ölçeklendirmek için kullanılan konaklar, yalıtılmış bir ana bilgisayar havuzundan elde edilir. Bu konaklar donanım testlerini geçti ve tüm verileri Flash disklerinden güvenli bir şekilde sildi. Bir konağı kümeden kaldırdığınızda, iç diskler güvenli bir şekilde temizlenir ve konaklar, ana bilgisayarların yalıtılmış havuzuna yerleştirilir. Bir kümeye bir konak eklediğinizde, yalıtılmış havuzdan ayıklanmış bir konak kullanılır.

## <a name="vmware-software-versions"></a>VMware yazılım sürümleri

AVS özel bulut kümelerinde kullanılan VMware yazılımının geçerli yazılım sürümleri şunlardır:

| Yazılım              |    Sürüm   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6,7 U2    | 
| ESXi                  |    6,7 U2    | 
| vSAN                  |    6,7 U2    |
| NSX-T                 |      2,5     |

Özel buluttaki herhangi bir yeni küme için, yazılımın sürümü şu anda özel bulutta çalışmakta olan ile eşleşmeyecektir. Bir müşteri aboneliğinde yeni bir özel bulut için, yazılım yığınının en son sürümü yüklenir.

AVS platformu yazılımının genel yükseltme ilkeleri ve işlemleri, yükseltmeler kavramları belgesinde açıklanır.

## <a name="host-maintenance-and-lifecycle-management"></a>Konak bakımı ve yaşam döngüsü yönetimi

Konak bakımı ve yaşam döngüsü yönetimi, özel bulut kümelerinin kapasitesine veya performansına etkilenmeden yapılır. Otomatik ana bilgisayar bakımı örnekleri, bellenim yükseltmeleri ve donanım onarımı ya da değişikliği içerir.

Microsoft, NSX-T Manager ve NSX-T kenarları gibi NSX-T gereçlerinin yaşam döngüsü yönetiminden sorumludur. Microsoft, katman-0 ağ geçidini oluşturma ve Kuzey-Güney yönlendirmeyi etkinleştirme gibi önyükleme ağ yapılandırmadan da sorumludur. AVS özel bulutunuzun Yöneticisi olarak ağ kesimleri, dağıtılmış güvenlik duvarı kuralları, katman 1 ağ geçitleri ve yük dengeleyiciler gibi NSX-T SDN yapılandırmasından sorumlusunuz.

> [!IMPORTANT]
> Bir AVS yöneticisinin NSX-T kenarları veya Katman-0 ağ geçidi yapılandırmasını değiştirmemesi gerekir. Bu durum hizmet kaybına neden olabilir.

## <a name="backup-and-restoration"></a>Yedekleme ve geri yükleme

Özel bulut vCenter ve NSX-T yapılandırması saatlik olarak yedeklenir. Yedeklemeler üç gün boyunca tutulur. Yedekten geri yükleme, Azure portal bir hizmet Isteğiyle istenir.

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, [Ağ ve bağlantı tabanlı kavramlar](concepts-networking.md)öğrenirsiniz.

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VCSA versions]: https://kb.vmware.com/s/article/2143838
[ESXi versions]: https://kb.vmware.com/s/article/2143832
[vSAN versions]: https://kb.vmware.com/s/article/2150753

