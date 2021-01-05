---
title: VMware yazılım sürümleri
description: Azure VMware çözümü için desteklenen VMware yazılım sürümleri.
ms.topic: include
ms.date: 12/29/2020
ms.openlocfilehash: c6ba2904bab6c6f44001cafed1bd4cbdeb786373
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97825088"
---
<!-- Used in faq.md and concepts-private-clouds-clusters.md -->


Azure VMware çözümü özel bulut kümelerinde kullanılan VMware yazılımının geçerli yazılım sürümleri şunlardır:

| Yazılım              |    Sürüm   |
| :---                  |     :---:    |
| VCSA/vSphere/ESXi |    6,7 U3    | 
| ESXi                  |    6,7 U3    | 
| vSAN                  |    6,7 U3    |
| NSX-T                 |      2.5     |


>[!NOTE]
>NSX-T, NSX 'in desteklenen tek sürümüdür.

Özel buluttaki herhangi bir yeni küme için, yazılım sürümü şu anda çalışmakta olan yenilikle eşleşir. Bir abonelikteki tüm yeni özel bulutta, yazılım yığınının en son sürümü yüklenir. Daha fazla bilgi için bkz. [VMware yazılım sürümü gereksinimleri](https://docs.vmware.com/en/VMware-HCX/services/user-guide/GUID-54E5293B-8707-4D29-BFE8-EE63539CC49B.html).

Özel bulut yazılım paketi yükseltmeleri, Yazılımı VMware 'den en son yazılım paketi sürümünün bir sürümü içinde tutar. Özel bulut yazılımı sürümleri, bireysel yazılım bileşenlerinin en son sürümlerinden (ESXi, NSX-T, vCenter, vSAN) farklı olabilir. [Özel bulut güncelleştirmelerinde ve güncelleştirmelerinde](../concepts-upgrades.md)açıklanan Azure VMware çözüm platformu yazılımının genel yükseltme ilkelerini ve işlemlerini bulabilirsiniz.

