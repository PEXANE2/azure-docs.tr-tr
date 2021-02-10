---
title: Kavramlar-özel bulut güncelleştirmeleri ve yükseltmeleri
description: Azure VMware çözümünde anahtar yükseltme işlemi ve özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/02/2021
ms.openlocfilehash: 78d4b566aa9156cdddfdcd69b50ebfd1d10aa784
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "100006721"
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

VMware yazılım sürümleri hakkında daha fazla bilgi için bkz. [özel bulutlar ve kümeler kavram makalesi](concepts-private-clouds-clusters.md) ve [SSS](faq.yml).

## <a name="next-steps"></a>Sonraki adımlar

Azure VMware çözümünde anahtar yükseltme süreçlerini ve özelliklerini artık kapsadığınıza göre şunları öğrenmek isteyebilirsiniz:

- [Özel bulut oluşturma](tutorial-create-private-cloud.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
