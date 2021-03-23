---
title: Azure VMware çözümü için platform güncelleştirmeleri
description: Azure VMware çözümüne yönelik platform güncelleştirmeleri hakkında bilgi edinin.
ms.topic: reference
ms.date: 03/16/2021
ms.openlocfilehash: ce25df2cb221c032f6dd430f292522fe86e69ceb
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104771779"
---
# <a name="platform-updates-for-azure-vmware-solution"></a>Azure VMware çözümü için platform güncelleştirmeleri

Azure VMware çözümünde önemli güncelleştirmeler, Mart 2021 ' den itibaren uygulanacak. Azure hizmet durumu ile bakım zaman çizelgesini içeren bir bildirim alacaksınız. Azure VMware çözümünde anahtar yükseltme işlemleri ve özellikleri hakkında daha fazla bilgi için bkz. [Azure VMware çözümü özel bulut güncelleştirmeleri ve yükseltmeleri](concepts-upgrades.md).

## <a name="march-15-2021"></a>15 Mart 2021 

- Azure VMware çözüm hizmeti, 2021 Mart ' e kadar, özel bulutunuzda vCenter Server 'ı güncelleştirme 3l sürümünü 6,7 vCenter Server.

- Bu süre boyunca VMware vCenter kullanılamaz ve VM 'Leri yönetemeyeceksiniz (durdurma, başlatma, oluşturma, silme). Özel bulut Ölçeklendirmesi (sunucu ve küme ekleme/kaldırma) de kullanılamaz. VMware yüksek kullanılabilirlik (HA), mevcut VM 'Ler için koruma sağlamak üzere çalışmaya devam edecektir. 
 
Bu vCenter sürümü hakkında daha fazla bilgi için bkz. [VMware vCenter Server 6,7 güncelleştirme 3l sürüm notları](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/vsphere-vcenter-server-67u3l-release-notes.html).

## <a name="march-4-2021"></a>4 Mart 2021

- Azure VMware çözümleri, 2021 Mart ' e kadar, var olan özel bulutlardaki ESXi 'e [VMware ESXi 6,7, Patch Release ESXi670-202011002](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/esxi670-202011002.html)' e kadar olan düzeltmeleri uygular.

- VSphere Stack için, [VMSA-2021-0002](https://www.vmware.com/security/advisories/VMSA-2021-0002.html)tarihine kadar belgelenen geçici çözümler, 15 Mart 2021 tarihine kadar de uygulanır.

>[!NOTE]
>Bu, kesintiye uğramamış ve Azure VMware hizmetleri 'ni veya iş yüklerini etkilemeyecek olmalıdır. Bakım sırasında, dvports üzerinde _kayıp ağ bağlantısı_ ve _dvports üzerinde kayıp yukarı_ bağlantı gibi çeşitli VMware uyarıları, vCenter 'da görünür ve bakım ilerledikçe otomatik olarak temizlenir.

## <a name="post-update"></a>Güncelleştirme sonrası
Tamamlandıktan sonra, VMware bileşenlerinin daha yeni sürümleri görüntülenir. Herhangi bir sorunla karşılaşırsanız veya herhangi bir sorunuz olursa destek bileti açarak destek ekibimize başvurun.





