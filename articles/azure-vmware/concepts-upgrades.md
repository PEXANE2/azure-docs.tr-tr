---
title: Kavramlar-özel bulut güncelleştirmeleri ve yükseltmeleri
description: Azure VMware çözümünde anahtar yükseltme işlemi ve özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 6fb65a0e192fbf1f054857223b3fdaf58fc6a903
ms.sourcegitcommit: 31d242b611a2887e0af1fc501a7d808c933a6bf6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2020
ms.locfileid: "97809658"
---
# <a name="azure-vmware-solution-private-cloud-updates-and-upgrades"></a>Azure VMware çözümü özel bulut güncelleştirmeleri ve yükseltmeleri

Azure VMware çözümünün özel bulutlarının önemli avantajlarından biri, platformun sizin için korunmasını sağlar. Platform bakımı, doğrulanan Azure VMware çözümü özel bulut yazılımının en son sürümünü kullandığınızdan emin olmaya yardımcı olan, VMware tarafından doğrulanan bir yazılım paketine yönelik otomatikleştirilmiş güncelleştirmeler içerir.

Özellikle, bir Azure VMware çözümü özel bulutu şunları içerir:

- VMware ESXi hiper yöneticiyle sağlanan adanmış çıplak sunucu düğümleri 
- ESXi ve vSAN yönetimi için vCenter Server 
- VSphere iş yükü VM 'Leri için VMware NSX-T yazılım tanımlı ağ  
- VSphere iş yükü VM 'Leri için VMware vSAN veri deposu  
- İş yükü Mobility için VMware HCX  

Bu bileşenlere ek olarak, Azure VMware çözümü özel bulutu, bağlantı için gerekli olan Azure ve özel bulutu çalıştırmak için gereken kaynakları içerir. Azure VMware çözümü, hem düşük düzenleme hem de VMware bileşenlerinin sistem durumunu sürekli olarak izler. Azure VMware çözümü bir hata algıladığında, başarısız olan bileşenleri onarmak için işlem gerçekleştirir. 

## <a name="what-components-get-updated"></a>Hangi bileşenler güncelleştiriliyor?   

Azure VMware çözümü aşağıdaki VMware bileşenlerini güncelleştirir: 

- çıplak sunucu düğümlerinde çalışan vCenter Server ve ESXi 
- vSAN 
- NSX-T 

Azure VMware çözümü, sürücüler, ağ anahtarları üzerindeki yazılımlar ve çıplak düğümlerdeki bellenim gibi düşük düzenleme yazılımlarını da güncelleştirir. 

## <a name="types-of-updates"></a>Güncelleştirme türleri

Azure VMware çözümü, VMware bileşenlerine aşağıdaki güncelleştirme türlerini uygular:

- Düzeltme ekleri: VMware tarafından yayınlanan güvenlik düzeltme ekleri ve hata düzeltmeleri. 
- Güncelleştirmeler: bir veya daha fazla VMware bileşeninin Ikincil sürüm güncelleştirmeleri. 
- Yükseltmeler: bir veya daha fazla VMware bileşeninin ana sürüm güncelleştirmeleri.

Özel bulutlarınıza düzeltme ekleri uygulandıktan önce ve sonra bildirim alınacaktır. Ayrıca, özel bulutunuzda güncelleştirmeler veya yükseltmeler uygulamadan önce bir bakım penceresi zamanlamak için sizinle birlikte çalışacağız. 

## <a name="vmware-appliance-backup"></a>VMware gereç yedeklemesi 

Azure VMware çözümü, güncelleştirme yapmanın yanı sıra bu VMware bileşenlerinin bir yapılandırma yedeklemesini alır:

- vCenter Server 
- NSX-T Yöneticisi 

Azure VMware çözümü, hatanın süresiyle bunları yapılandırma yedeklemesinden geri yükleyebilir. 

VMware yazılım sürümleri hakkında daha fazla bilgi için bkz. [özel bulutlar ve kümeler kavram makalesi](concepts-private-clouds-clusters.md) ve [SSS](faq.md).

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım [özel bir bulut oluşturmaktır](tutorial-create-private-cloud.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
