---
title: Kavramlar-Azure VMware Çözüm özel bulutlarını Izleme ve onarma
description: Azure VMware çözümünün bir Azure VMware çözümü özel bulutundaki VMware ESXi sunucularını nasıl izlediğini ve onarmadığını öğrenin.
ms.topic: conceptual
ms.custom: contperf-fy21q2
ms.date: 02/03/2021
ms.openlocfilehash: 6174df429fd9b21c7f685c8ba14e6d5c0bba4c83
ms.sourcegitcommit: 44188608edfdff861cc7e8f611694dec79b9ac7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99538967"
---
# <a name="monitor-and-repair-azure-vmware-solution-private-clouds"></a>Azure VMware Çözüm özel bulutlarını izleme ve onarma

Azure VMware çözümü, Azure VMware çözümü özel bulutundaki VMware ESXi sunucularını sürekli olarak izler. 

## <a name="what-azure-vmware-solution-monitors"></a>Azure VMware çözümünün izleyicileri

Azure VMware çözümü konaktaki hata koşulları için aşağıdakini izler:  

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

Azure VMware çözümü, kiracının özel bulutundaki bir Azure VMware Çözüm düğümünde düşme veya başarısızlık algıladığında, ana bilgisayar düzeltme sürecini tetikler. Ana bilgisayar düzeltmesi hatalı düğümü yeni bir sağlıklı düğümle değiştirmeyi içerir.  

Ana bilgisayar düzeltme süreci, kümeye yeni bir sağlıklı düğüm eklenerek başlatılır. Daha sonra, mümkün olduğunda, hatalı ana bilgisayar VMware vSphere bakım moduna konur. VMware vMotion, VM 'Leri hatalı ana bilgisayardan kümedeki diğer kullanılabilir sunuculara taşımak için kullanılır ve bu da iş yüklerinin sıfır kapalı kalma süresi dinamik geçişi için izin verebilir. Hatalı konağın bakım moduna yerleştirilebileceği senaryolarda, konak kümeden kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure VMware çözümünün özel bulutları nasıl izlediğini ve onardığına göre, şunları öğrenmek isteyebilirsiniz:

- [Azure VMware çözümü özel bulut yükseltmeleri](concepts-upgrades.md).
- [Azure VMware Çözüm kaynağını etkinleştirme](enable-azure-vmware-solution.md).
