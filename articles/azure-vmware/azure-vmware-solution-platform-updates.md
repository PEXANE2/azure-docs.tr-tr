---
title: Azure VMware çözümü için platform güncelleştirmeleri
description: Azure VMware çözümüne yönelik platform güncelleştirmeleri hakkında bilgi edinin.
ms.topic: reference
ms.date: 03/16/2021
ms.openlocfilehash: 4f4c697f345cca093a83eab2f915aaf9e80ab10f
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103563139"
---
# <a name="platform-updates-for-azure-vmware-solution"></a>Azure VMware çözümü için platform güncelleştirmeleri

Azure VMware çözümünde önemli güncelleştirmeler, Mart 2021 ' den itibaren uygulanacak. Azure hizmet durumu ile bakım zaman çizelgesini içeren bir bildirim alacaksınız. Bu makalede, bu bakım işlemi sırasında ne beklediğinizi ve özel bulutunuzda yapılan değişiklikleri öğreneceksiniz.

## <a name="march-15-2021"></a>15 Mart 2021 

- Azure VMware çözüm hizmeti, 19 Mart 2021 ' e kadar 6,7 güncelleştirme 3l sürümünü vCenter Server için özel bulutunuzda vCenter Server 'ı güncelleştirmek üzere bakım işini gerçekleştirmeye çalışacak.

- Bu süre boyunca VMware vCenter kullanılamaz ve VM 'Leri yönetemeyeceksiniz (durdurma, başlatma, oluşturma, silme). VMware yüksek kullanılabilirlik (HA), mevcut VM 'Ler için koruma sağlamak üzere çalışmaya devam edecektir. Özel bulut Ölçeklendirmesi (sunucu ve küme ekleme/kaldırma) de kullanılamaz.
 
Bu vCenter sürümü hakkında daha fazla bilgi için bkz. [VMware vCenter Server 6,7 güncelleştirme 3l sürüm notları](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/vsphere-vcenter-server-67u3l-release-notes.html).

## <a name="march-4-2021"></a>4 Mart 2021

- Azure VMware çözümleri, mevcut özel bulutlarda ESXi 'ye düzeltme ekleri uygular [VMware ESXi 6,7, Patch Release ESXi670-202011002](https://docs.vmware.com/en/VMware-vSphere/6.7/rn/esxi670-202011002.html) Ila 15 Mart 2021.

- VSphere Stack için, [VMSA-2021-0002](https://www.vmware.com/security/advisories/VMSA-2021-0002.html)tarihine kadar belgelenen geçici çözümler, 15 Mart 2021 tarihine kadar de uygulanır.

>[!NOTE]
>Bu, kesintiye uğramamış ve Azure VMware hizmetleri 'ni veya iş yüklerini etkilemeyecek olmalıdır. Bakım sırasında, dvports üzerinde _kayıp ağ bağlantısı_ ve _dvports üzerinde kayıp yukarı_ bağlantı gibi çeşitli VMware uyarıları, vCenter 'da görünür ve bakım ilerledikçe otomatik olarak temizlenir.

## <a name="post-update"></a>Güncelleştirme sonrası
Tamamlandıktan sonra, VMware bileşenlerinin daha yeni sürümleri görüntülenir. Herhangi bir sorunla karşılaşırsanız veya herhangi bir sorunuz olursa destek bileti açarak destek ekibimize başvurun.



