---
title: Azure 'da BareMetal altyapı önizlemesine genel bakış
description: Azure 'da BareMetal altyapısının nasıl dağıtılacağı konusuna genel bakış.
ms.custom: references_regions
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: db974d9260344d1f6050235bb2a9fbaa0420659b
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829265"
---
#  <a name="what-is-baremetal-infrastructure-preview-on-azure"></a>Azure 'da BareMetal altyapı önizlemesi nedir?

Azure BareMetal altyapısı, kurumsal özel iş yüklerini geçirmeye yönelik güvenli bir çözüm sunar. BareMetal örnekleri, size atanan paylaşılmayan konak/sunucu donanımdır. Şirket içi çözümünüzü, sertifikalı donanım, lisanslama ve destek anlaşmaları gerektiren özel iş yükleri ile taşıma işlemlerini kaldırır. Azure, Konuk içi işletim sistemi (OS) izleme ve uygulama izleme sahipliğiniz dahilinde olduğu için altyapı izlemeyi ve bakımını sizin için işler.

BareMetal altyapısı, mevcut yatırımlarınızı ve mimarinizi koruyarak altyapınızı yatay olarak modernleştirin için bir yol sağlar. BareMetal altyapısı sayesinde, Azure hizmetlerine erişim ve bu hizmetleri düşük gecikme süresiyle tümleştirmenize olanak tanıyan özel iş yüklerini Azure 'a getirebilirsiniz.

## <a name="sku-availability-in-azure-regions"></a>Azure bölgelerindeki SKU kullanılabilirliği
Özelleştirilmiş ve genel amaçlı iş yükleri için BareMetal altyapısı, düzeltme 4,2 (Rev 4,2) damgalarına dayalı dört bölgeden başlayarak kullanılabilir:
- West Europe
- Kuzey Avrupa
- Doğu ABD 2
- Orta Güney ABD

>[!NOTE]
>**Rev 4,2** , var olan Rev 4 mimarisini kullanan en son yeniden markalanmış BareMetal altyapısıdır.  Rev 4, Azure sanal makinesi (VM) konaklarına daha yakından yakınlık sağlar ve Azure VM 'Leri ile BareMetal örnek birimleri arasındaki gecikmeyi azaltır. BareMetal örneklerinizi Azure portal aracılığıyla erişebilir ve yönetebilirsiniz. 

## <a name="support"></a>Destek
BareMetal altyapısı ISO 27001, ISO 27017, SOC 1 ve SOC 2 uyumludur.  Ayrıca, kendi lisansını getir (KLG) modelini kullanır: işletim sistemi, özel iş yükü ve üçüncü taraf uygulamalar.  

Kök erişim ve tam denetim elde edersiniz almaz, şu şekilde sorumluluk alacaksınız:
- Yedekleme ve kurtarma çözümleri, yüksek kullanılabilirlik ve olağanüstü durum kurtarma tasarlama ve uygulama
- İşletim sistemi ve üçüncü taraf yazılım için lisanslama, güvenlik ve destek

Microsoft 'un sorumluluğundadır:
- Özel iş yükleri için sertifikalı donanım sağlama 
- İşletim sistemini sağlama

:::image type="content" source="media/baremetal-support-model.png" alt-text="BareMetal altyapı destek modeli" border="false":::

## <a name="compute"></a>İşlem
BareMetal altyapısı, özel iş yükleri için sertifikalı birden çok SKU sunar. Kullanılabilir SKU 'Lar, daha küçük iki soketli sistemden 24 soketli sisteme kadar kullanılabilir. Özel iş yükünüz için iş yüküne özgü sertifikalı SKU 'Ları kullanın.

BareMetal örnek damgası, aşağıdaki bileşenleri birleştirir:

- **Hesaplanıyor:** Gerekli bilgi işlem yeteneğini sağlayan ve özelleştirilmiş iş yükü için sertifikalı, farklı bir Intel Xeon işlemcisi oluşturmayı temel alan sunucular.

- **Ağ:** Birleşik yüksek hızlı bir ağ dokusunda bilgi işlem, depolama ve LAN bileşenleri birbirine bağlanır.

- **Depolama alanı:** Birleşik bir ağ dokusunda erişilen bir altyapı.

BareMetal damgasının çok kiracılı altyapısı içinde müşteriler yalıtılmış kiracılarda dağıtılır. Bir kiracı dağıttığınızda Azure kaydınızda bir Azure aboneliği adı alırsınız. Bu Azure aboneliği, karemetal örneklerin faturalandırılaminden biridir.

>[!NOTE]
>BareMetal örneğinde dağıtılan bir müşteri, bir kiracıya yalıtılmış olarak alınır. Bir kiracı, diğer kiracılardan ağ, depolama ve işlem katmanında yalıtılmıştır. Farklı kiracılara atanan depolama ve işlem birimleri, her birinin veya BareMetal örneklerinde birbirleriyle iletişim kuramazlar.

## <a name="os"></a>İşletim Sistemi
BareMetal örneğinin sağlanması sırasında, makinelere yüklemek istediğiniz işletim sistemini seçebilirsiniz. 

>[!NOTE]
>Unutmayın, BareMetal altyapısı bir KLG modelidir.

Kullanılabilir Linux işletim sistemi sürümleri şunlardır:
- Red Hat Enterprise Linux (RHEL) 7,6
- SUSE Linux Enterprise Server (SLES)
   - SLES 12 SP2
   - SLES 12 SP3
   - SLES 12 SP4
   - SLES 12 SP5
   - SLES 15 SP1

## <a name="storage"></a>Depolama
Belirli SKU türüne göre BareMetal örnekleri, belirli iş yükü türüne bağlı olarak önceden tanımlanmış NFS depolaması ile gelir. BareMetal sağladığınızda, destek isteği göndererek tahmini büyümenize göre ek depolama alanı sağlayabilirsiniz. Tüm depolama NFSv3 ve NFSv4 desteğiyle düzeltme 4,2 ' de bir All-Flash diski ile gelir. Daha yeni düzeltme 4,5 NVMe SSD kullanıma sunulacaktır. Depolama boyutlandırma hakkında daha fazla bilgi için [Baremetal iş yükü türü](../../../virtual-machines/workloads/sap/get-started.md) bölümüne bakın.

>[!NOTE]
>BareMetal için kullanılan depolama, varsayılan olarak bekleyen şifreleme sunarak FIPS 140-2 güvenlik gereksinimlerini karşılar. Veriler, disklerde güvenli bir şekilde depolanır.

## <a name="networking"></a>Ağ
Azure ağ hizmetleri 'nin mimarisi, BareMetal örneklerinde özelleştirilmiş iş yüklerinin başarılı bir şekilde dağıtılması için önemli bir bileşendir. Bu, tüm BT sistemleri Azure 'da zaten yer alıyor olabilir. Azure, Azure 'u şirket içi yazılım dağıtımlarınıza bir sanal veri merkezi gibi görünür hale getirmek için size ağ teknolojisi sunar. BareMetal örnekleri için gereken Azure ağ işlevselliği:

- Azure sanal ağları, şirket içi ağ varlıklarınıza bağlanan ExpressRoute devresine bağlanır.
- Şirket içinde Azure 'a bağlanan bir ExpressRoute devresi, en az 1 Gbps veya daha yüksek bant genişliğine sahip olmalıdır.
- Azure 'da genişletilmiş Active Directory ve DNS veya Azure 'da tamamen çalışma.

ExpressRoute 'u kullanmak, şirket içi ağınızı bir bağlantı sağlayıcısının yardımı ile özel bir bağlantı üzerinden Microsoft bulutuna genişletmenizi sağlar. **ExpressRoute Premium** 'u, coğrafi olmayan sınırlar genelinde bağlantıyı genişletebilir veya **ExpressRoute Local** ' i kullanarak istediğiniz Azure bölgesinin yakınında bulunan konum arasında uygun maliyetli veri aktarımı gerçekleştirebilirsiniz.

BareMetal örnekleri, Azure VNET sunucunuzun IP adresi aralığınızı içinde temin edilir.

:::image type="content" source="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" alt-text="Azure BareMetal altyapı diyagramı" lightbox="media/baremetal-infrastructure-portal/baremetal-infrastructure-diagram.png" border="false":::

Gösterilen mimari üç bölüme ayrılmıştır:
- **Sol:** Farklı uygulamaları çalıştıran, iş ortağı veya Equinx gibi yerel uç yönlendiriciyle bağlanan müşteri şirket içi altyapısını gösterir. Daha fazla bilgi için bkz. [bağlantı sağlayıcıları ve konumlar: Azure ExpressRoute](../../../expressroute/expressroute-locations.md).
- **Ortala:** Azure Edge ağına bağlantısı sunan Azure aboneliğiniz kullanılarak sağlanan [ExpressRoute](../../../expressroute/expressroute-introduction.md) 'ı gösterir.
- **Sağ:** Azure IaaS ' i gösterir ve bu durumda VM 'lerin Azure sanal ağınızda sağlanan uygulamalarınızı barındırmak için kullanılması gerekir.
- **Alt:** Düşük gecikme süresine sahip BareMetal bağlantısı için ExpressRoute [FastPath](../../../expressroute/about-fastpath.md) Ile etkinleştirilen ExpressRoute ağ geçidinizin kullanımını gösterir.   
   >[!TIP]
   >Bunu desteklemek için, ExpressRoute ağ geçidinizin UltraPerformance olmalıdır.  Daha fazla bilgi için bkz. [ExpressRoute sanal ağ geçitleri hakkında](../../../expressroute/expressroute-about-virtual-network-gateways.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, Azure portal aracılığıyla BareMetal örnek birimlerinin nasıl tanımlanacağına ve etkileşime gireceğini öğrenirsiniz.

> [!div class="nextstepaction"]
> [BareMetal Örnekleri Azure portal aracılığıyla yönetme](baremetal-infrastructure-portal.md)