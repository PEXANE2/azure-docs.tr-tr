---
title: include dosyası
description: include dosyası
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 0f528117b19a3f26b964dee7ae453fc184c8dddb
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123067"
---
Azure Compute, belirli bir donanım türüyle sınırlanmış ve tek bir müşteriye ayrılmış olan sanal makine boyutları sunar.  Bu sanal makine boyutları, uyumluluk ve düzenleme gereksinimleri gibi öğeler içeren iş yükleri nedeniyle diğer müşterilerden yüksek ölçüde yalıtıma ihtiyaç duyan iş yükleri için idealdir.  Müşteriler Ayrıca, [iç içe geçmiş sanal makineler Için Azure desteği](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)'ni kullanarak bu yalıtılmış sanal makinelerin kaynaklarını daha fazla alt bölümlere ayırmak da tercih edebilir.

Yalıtılmış bir boyut kullanılması, sanal makinenizin o belirli sunucu örneğinde çalışan tek bir tane olmasını garanti eder.  Geçerli yalıtılmış sanal makine teklifleri şunları içerir:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

Her kullanılabilir yalıtılmış boyut hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)bulabilirsiniz.