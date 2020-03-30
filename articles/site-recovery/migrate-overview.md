---
title: Azure Site Kurtarma ile sunucuları ve VM'leri Azure'a geçirme
description: Azure Site Kurtarma hizmetini kullanarak şirket içi ve Azure IaaS VM'leri Azure'a nasıl geçirilenleri açıklar.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: raynew
ms.openlocfilehash: a7107eae5c798deb78d4d35eccdf4adcf5273335
ms.sourcegitcommit: 0553a8b2f255184d544ab231b231f45caf7bbbb0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80388908"
---
# <a name="about-migration"></a>Geçiş hakkında

[Azure Site Kurtarma](site-recovery-overview.md) hizmetinin makineleri geçirmenize nasıl yardımcı olduğu hakkında hızlı bir genel bakış için bu makaleyi okuyun. 

> [!TIP]
> Artık Azure Site Kurtarma hizmeti yerine VM'leri ve sunucuları Azure'a geçirmek için Azure Geçir hizmetini kullanmanız gerekir. [Daha fazla bilgi edinin](../migrate/migrate-services-overview.md).


Site Kurtarma'yı kullanarak geçirebileceğiniz ler şunlardır:

- **Şirket içinde Azure'a geçiş :** Şirket içi Hyper-V V'm'leri, VMware VM'leri ve fiziksel sunucuları Azure'a geçirin. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure VM’lerini bir Azure bölgesinden diğerine geçirin. 
- **AWS geçirme**: AWS Windows örneklerini Azure IaaS’ye geçirin. 

> [!NOTE]
> Artık Azure Geçiş hizmetini kullanarak şirket içi azure'a geçiş yapabilirsiniz. [Daha fazla bilgi edinin](../migrate/migrate-overview.md).

## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Şirket içi ve Azure VM'lerinin olağanüstü durum kurtarma sıyrıklarını kurtarma için Site Kurtarma'yı kullanmanın yanı sıra, bunları geçirmek için Site Kurtarma hizmetini kullanabilirsiniz. Aralarındaki fark nedir?

- Olağanüstü durum kurtarma için makineleri düzenli olarak Azure'a çoğaltırsınız. Bir kesinti meydana geldiğinde, makineleri birincil siteden ikincil Azure sitesine kadar başarısız olur ve bunlara buradan erişebilirsiniz. Birincil site yeniden kullanılabilir olduğunda, Azure'dan geri dönmezsiniz.
- Geçiş için, şirket içi makineleri Azure'a veya Azure VM'lerini ikincil bir bölgeye çoğaltırsınız. Sonra VM birincil siteden ikincil üzerinde başarısız ve geçiş işlemini tamamlamak. Yeniden çalışma işlemi yapılmaz.  


## <a name="migration-scenarios"></a> Geçiş senaryoları

**Senaryo** | **Şey**
--- | ---
**Şirket içinden Azure'a geçirme** | Şirket içi VMware VM'leri, Hyper-V VM'leri ve fiziksel sunucuları Azure'a geçirebilirsiniz. Bunu yapmak için, tam olağanüstü durum kurtarma için olduğu gibi hemen hemen aynı adımları tamamlayın. Azure'dan şirket içi siteye geri dönemeniz yeterlidir.
**Azure bölgeleri arasında geçiş yapma** | Azure VM'lerini bir Azure bölgesinden diğerine geçirebilirsiniz. Geçiş tamamlandıktan sonra, geçiş yaptığınız ikincil bölgede bulunan Azure VM'leri için olağanüstü durum kurtarma yı yapılandırabilirsiniz.
**AWS örneklerini Azure’a geçirme** | AWS örneklerini Azure VM’lerine geçirebilirsiniz. Site Kurtarma, AWS örneklerini geçiş amacıyla fiziksel sunucular olarak ele alır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi makineleri Azure’a geçirme](migrate-tutorial-on-premises-azure.md)
- [VM’leri bir Azure bölgesinden diğerine geçirme](azure-to-azure-tutorial-migrate.md)
- [AWS örneklerini Azure’a geçirme](migrate-tutorial-aws-azure.md)
