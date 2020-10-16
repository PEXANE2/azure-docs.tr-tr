---
title: Kavramlar-Azure VMware Çözüm özel bulutlarını Izleme ve onarma
description: Azure VMware çözümünün bir Azure VMware çözümü özel bulutundaki VMware ESXi sunucularını nasıl izlediğini ve onarmadığını öğrenin.
ms.topic: conceptual
ms.date: 09/16/2020
ms.openlocfilehash: bee08304cd14f2aeec6995203638b5a37d9a861c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91346231"
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

[Azure VMware çözümü özel bulut yükseltmeleri](concepts-upgrades.md)hakkında bilgi edinin.  
