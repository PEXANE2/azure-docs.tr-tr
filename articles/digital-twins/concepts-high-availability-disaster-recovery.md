---
title: Yüksek kullanılabilirlik ve olağanüstü durum kurtarma
titleSuffix: Azure Digital Twins
description: Olağanüstü durum kurtarma özellikleriyle yüksek düzeyde kullanılabilir Azure IoT çözümleri oluşturmanıza yardımcı olan Azure ve Azure dijital TWINS özelliklerini açıklar.
author: baanders
ms.author: baanders
ms.date: 10/14/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: ac75a5b0b59a06855b7ee88d971c269ca915e429
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763181"
---
# <a name="azure-digital-twins-high-availability-and-disaster-recovery"></a>Azure dijital TWINS yüksek kullanılabilirlik ve olağanüstü durum kurtarma

Dayanıklı IoT çözümleri için önemli bir alan olan iş sürekliliği ve olağanüstü durum kurtarma. **Yüksek kullanılabilirlik (ha)** ve **olağanüstü durum kurtarma (Dr)** için tasarlama, çözümünüz için uygun çalışma süresi hedeflerini tanımlamanıza ve elde etmenize yardımcı olabilir.

Bu makalede, Azure Digital TWINS hizmeti tarafından özellikle sunulan HA ve DR özellikleri ele alınmaktadır.

Azure dijital TWINS bu özellik seçeneklerini destekler:
* *Bölge ıçı ha* – hizmetin çalışma süresini sunmaya yönelik yerleşik yedeklilik
* *Çapraz bölge Dr* – beklenmedik bir veri merkezi hatası durumunda coğrafi eşlenmiş bir Azure bölgesine yük devretme

HA/DR için tasarlama hakkında genel Azure Kılavuzu için [*en iyi uygulamalar*](#best-practices) bölümünü de görebilirsiniz.

## <a name="intra-region-ha"></a>Bölge içi HA
 
Azure dijital TWINS, hizmet içinde artıklıkları uygulayarak bölge içi HA sağlar. Bu, çalışma süresi için [SERVICE SLA](https://azure.microsoft.com/support/legal/sla/digital-twins) 'da yansıtılır. **Bu HA özelliklerinden faydalanmak için bir Azure dijital TWINS çözümünün geliştiricilerine ek bir iş gerekmez.** Azure dijital TWINS, makul bir çalışma süresi garantisi sunmakla birlikte, herhangi bir dağıtılmış bilgi işlem platformunda olduğu gibi geçici sorunlar da beklenmeye devam edebilir. Uygun yeniden deneme ilkeleri, geçici hatalarla başa çıkmak için bir bulut uygulamasıyla etkileşime geçen bileşenlere yerleşik olmalıdır.

## <a name="cross-region-dr"></a>Çapraz bölge DR

Veri Merkezi, bölgedeki güç arızalarına veya diğer olaylara karşı genişletilmiş kesintiler yaşadığında bazı nadir durumlar olabilir. Bu tür olaylar nadir ve bu tür hatalarda, yukarıda açıklanan bölge içi HA özelliği de yardımcı olmayabilir. Azure dijital TWINS bunu Microsoft tarafından başlatılan yük devretme ile gidermektedir.

**Microsoft tarafından başlatılan yük devretme** , Microsoft 'un, etkilenen bir bölgeden gelen tüm Azure dijital TWINS örneklerinin, ilgili coğrafi eşleştirilmiş bölgeye yük devretmesine olanak sağlar. Bu işlem, varsayılan bir seçenektir (kullanıcıların herhangi bir şekilde kabul etmesidir) ve kullanıcıdan müdahale gerektirmez. Microsoft bu seçeneğin ne zaman belirlendiğini belirleme hakkını saklı tutar. Bu mekanizma, kullanıcının örneğinin yük devretme öncesinde Kullanıcı onayı içermez.

>[!NOTE]
> Bazı Azure Hizmetleri, **müşteri tarafından başlatılan yük devretme** adlı ek bir seçenek de sağlar. Bu, müşterilerin yalnızca kendi örnekleri için bir yük devretme başlatmasına olanak sağlar. ÖRNEĞIN, Dr detaylarını çalıştırmak gibi. Bu mekanizma Şu anda Azure dijital TWINS tarafından **desteklenmiyor** . 

## <a name="best-practices"></a>En iyi uygulamalar

HA/DR üzerinde en iyi yöntemler için, bu konuda aşağıdaki Azure yönergelerine bakın: 
* [*Azure Iş sürekliliği teknik kılavuzu*](/azure/architecture/framework/resiliency/overview) makalesi, iş sürekliliği ve olağanüstü durum kurtarma hakkında düşünmenize yardımcı olacak genel bir çatı açıklamaktadır. 
* [*Azure uygulamaları Için olağanüstü durum kurtarma ve yüksek kullanılabilirlik*](/azure/architecture/framework/resiliency/backup-and-recovery) sayfası, yüksek KULLANıLABILIRLIK (ha) ve olağanüstü durum kurtarma (Dr) sağlamak için Azure uygulamalarına yönelik stratejiler hakkında mimari yönergeler sağlar.

## <a name="next-steps"></a>Sonraki adımlar 

Azure dijital TWINS çözümlerini kullanmaya başlama hakkında daha fazla bilgi edinin:
 
* [*Azure Digital Twins nedir?*](overview.md)
* [*Hızlı başlangıç: örnek bir senaryoyu araştırma*](quickstart-adt-explorer.md)