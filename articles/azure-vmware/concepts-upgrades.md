---
title: Kavramlar-özel bulut güncelleştirmeleri ve yükseltmeleri
description: Azure VMware çözümünde anahtar yükseltme işlemi ve özellikleri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: d93453cbf6ad744844a04cd298cc18ad181cc0b0
ms.sourcegitcommit: 58ff80474cd8b3b30b0e29be78b8bf559ab0caa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100635001"
---
# <a name="azure-vmware-solution-private-cloud-updates-and-upgrades"></a>Azure VMware çözümü özel bulut güncelleştirmeleri ve yükseltmeleri

Azure VMware çözümünün özel bulutlarının bir avantajı, platformun sizin için korunabileceği bir avantajdır. Bakım, Azure VMware Çözüm özel bulut yazılımının en son sürümünü kullandığınızdan emin olmak için VMware tarafından doğrulanan bir yazılım paketine otomatik güncelleştirmeler içerir.

Özellikle, bir Azure VMware çözümü özel bulutu şunları içerir:

- VMware ESXi hiper yöneticiyle sağlanan adanmış çıplak sunucu düğümleri 
- ESXi ve vSAN yönetimi için vCenter Server 
- VSphere iş yükü VM 'Leri için VMware NSX-T yazılım tanımlı ağ  
- VSphere iş yükü VM 'Leri için VMware vSAN veri deposu  
- İş yükü Mobility için VMware HCX  

Azure VMware çözümü özel bulutu Ayrıca Azure 'un bağlantı için gerekli olan kaynakları ve özel bulutu çalıştırabilmesini de içerir. Azure VMware çözümü, hem düşük düzenleme hem de VMware bileşenlerinin sistem durumunu sürekli olarak izler. Azure VMware çözümü bir hata algıladığında, başarısız olan bileşenleri onarmak için işlem gerçekleştirir. 

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

Özel bulutlarınıza düzeltme ekleri uygulandıktan önce ve sonra size bildirim verilecektir. Ayrıca, özel bulutunuzda güncelleştirmeler veya yükseltmeler uygulamadan önce bir bakım penceresi zamanlamak için sizinle birlikte çalışacağız. 

## <a name="vmware-appliance-backup"></a>VMware gereç yedeklemesi 

Azure VMware çözümü Ayrıca aşağıdaki VMware bileşenlerinin bir yapılandırma yedeklemesini de alır:

- vCenter Server 
- NSX-T Yöneticisi 

Azure VMware çözümü, hatanın süresiyle bu bileşenleri yapılandırma yedeklemesinden geri yükleyebilir. 

VMware yazılım sürümleri hakkında daha fazla bilgi için bkz. [özel bulutlar ve kümeler kavram makalesi](concepts-private-clouds-clusters.md) ve [SSS](faq.yml).

## <a name="next-steps"></a>Sonraki adımlar

Azure VMware çözümünde anahtar yükseltme süreçlerini ve özelliklerini artık kapsadığınıza göre şunları öğrenmek isteyebilirsiniz:

- [Özel bulut oluşturma](tutorial-create-private-cloud.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).

<!-- LINKS - external -->

<!-- LINKS - internal -->
