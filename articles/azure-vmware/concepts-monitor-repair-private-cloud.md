---
title: Kavramlar-Azure VMware Çözüm özel bulutlarını Izleme ve onarma
description: Azure VMware çözümünün bir Azure VMware çözümü özel bulutundaki VMware ESXi sunucularını nasıl izlediğini ve onarmadığını öğrenin.
ms.topic: conceptual
ms.custom: contperf-fy21q2
ms.date: 02/16/2021
ms.openlocfilehash: 59319b5598be9770e82b9676a28444648230a019
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100633147"
---
# <a name="monitor-and-repair-azure-vmware-solution-private-clouds"></a>Azure VMware Çözüm özel bulutlarını izleme ve onarma

Azure VMware çözümü, Azure VMware çözümü özel bulutundaki VMware ESXi sunucularını sürekli olarak izler. 

## <a name="what-azure-vmware-solution-monitors"></a>Azure VMware çözümünün izleyicileri

Azure VMware çözümü, konakta aşağıdaki koşulları izler:  

- İşlemci durumu 
- Bellek durumu 
- Bağlantı ve güç durumu 
- Donanım fanı durumu 
- Ağ bağlantısı kaybı 
- Donanım sistem panosu durumu 
- VSAN ana bilgisayarının disk (ler) i üzerinde hatalar oluştu 
- Donanım voltajı 
- Donanım sıcaklık durumu 
- Donanım güç durumu 
- Depolama durumu 
- Bağlantı hatası 

> [!NOTE]
> Azure VMware Çözüm kiracı yöneticileri, vCenter üzerinde Azure VMware Çözüm denetim düzlemi tarafından yönetildiğinden, yukarıda tanımlanan VMware vCenter alarmları düzenlemeli veya silmemelidir. Bu uyarılar Azure VMware çözüm izleme tarafından Azure VMware Çözüm ana bilgisayarı düzeltme işlemini tetiklemek için kullanılır.

## <a name="azure-vmware-solution-host-remediation"></a>Azure VMware Çözüm ana bilgisayar düzeltmesi  

Azure VMware çözümü bir Azure VMware Çözüm düğümünde düşme veya başarısızlık algıladığında, ana bilgisayar düzeltme sürecini tetikler. Ana bilgisayar düzeltmesi hatalı düğümü yeni bir sağlıklı düğümle değiştirmeyi içerir.  

Ana bilgisayar düzeltmesi, kümeye yeni bir sağlıklı düğüm eklenerek başlar. Daha sonra, mümkün olduğunda, hatalı ana bilgisayar VMware vSphere bakım moduna konur. VMware vMotion, VM 'Leri hatalı ana bilgisayardan kümedeki diğer kullanılabilir sunuculara hareket ettirir ve bu da iş yüklerinin dinamik geçişi için sıfır kapalı kalma süresine izin verebilir. Hatalı ana bilgisayar bakım moduna yerleştirilemez, konak kümeden kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure VMware çözümünün özel bulutları nasıl izlediğini ve onardığına göre, şunları öğrenmek isteyebilirsiniz:

- [Azure VMware çözümü özel bulut yükseltmeleri](concepts-upgrades.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).
