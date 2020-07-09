---
title: Azure Site Recovery ile sunucu ve VM 'Leri Azure 'a geçirme
description: Şirket içi ve Azure IaaS VM 'lerinin Azure Site Recovery hizmeti kullanılarak Azure 'a nasıl geçirileceği açıklanmaktadır.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: raynew
ms.openlocfilehash: f42942d0e112aad147f78d62b5f5b43587eca9b3
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135604"
---
# <a name="about-migration"></a>Geçiş hakkında

[Azure Site Recovery](site-recovery-overview.md) hizmetinin makineleri geçirmenize nasıl yardımcı olduğunu kısaca öğrenmek için bu makaleyi okuyun. 

> [!TIP]
> Artık Azure geçişi hizmetini Azure Site Recovery hizmeti yerine VM 'Leri ve sunucuları Azure 'a geçirmek için kullanmanız gerekir. [Daha fazla bilgi edinin](../migrate/migrate-services-overview.md).


Site Recovery kullanarak geçirebileceğiniz özellikler şunlardır:

- Şirket **Içinden Azure 'A geçiş**: şirket içi Hyper-V VM 'Lerini, VMware VM 'lerini ve fiziksel sunucuları Azure 'a geçirin. Geçişten sonra şirket içi makinelerde çalışan iş yükleri Azure VM'ler üzerinde çalışacaktır. 
- **Azure içinde geçiş**: Azure VM’lerini bir Azure bölgesinden diğerine geçirin. 
- **AWS geçirme**: AWS Windows örneklerini Azure IaaS’ye geçirin. 

> [!NOTE]
> Artık Azure geçişi hizmetini kullanarak şirket içinden Azure 'a geçiş yapabilirsiniz. [Daha fazla bilgi edinin](../migrate/migrate-services-overview.md).

## <a name="what-do-we-mean-by-migration"></a>Geçiş ile kast edilen nedir?

Şirket içi ve Azure VM 'lerinin olağanüstü durum kurtarması için Site Recovery kullanmanın yanı sıra, Site Recovery hizmetini kullanarak bunları geçirebilirsiniz. Aralarındaki fark nedir?

- Olağanüstü durum kurtarma için, makineleri düzenli olarak Azure 'a çoğaltmanız gerekir. Bir kesinti oluştuğunda, makineleri birincil siteden ikincil Azure sitesine devreder ve oradan erişebilirsiniz. Birincil site yeniden kullanılabilir olduğunda, Azure 'dan yeniden yük devreolursunuz.
- Geçiş için şirket içi makineleri Azure 'a veya Azure VM 'lerini ikincil bir bölgeye çoğaltmanız gerekir. Daha sonra VM 'yi birincil siteden ikinciye devreder ve geçiş işlemini tamamlayabilirsiniz. Yeniden çalışma işlemi yapılmaz.  


## <a name="migration-scenarios"></a>Geçiş senaryoları

**Senaryo** | **Ayrıntılar**
--- | ---
**Şirket içinden Azure'a geçirme** | Şirket içi VMware VM 'Leri, Hyper-V VM 'Leri ve fiziksel sunucuları Azure 'a geçirebilirsiniz. Bunu yapmak için tam olağanüstü durum kurtarma için yaptığınız adımlarla neredeyse aynı adımları tamamlamayın. Yalnızca Azure 'dan şirket içi siteye geri yük devredemezsiniz.
**Azure bölgeleri arasında geçiş yapma** | Azure VM 'lerinden bir Azure bölgesinden diğerine geçiş yapabilirsiniz. Geçiş işlemi tamamlandıktan sonra, yalnızca geçirdiğiniz ikincil bölgede Azure VM 'Leri için olağanüstü durum kurtarmayı yapılandırabilirsiniz.
**AWS örneklerini Azure’a geçirme** | AWS örneklerini Azure VM’lerine geçirebilirsiniz. Site Recovery, AWS örneklerine geçiş amacıyla fiziksel sunucu olarak davranır. 

## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi makineleri Azure’a geçirme](migrate-tutorial-on-premises-azure.md)
- [VM’leri bir Azure bölgesinden diğerine geçirme](azure-to-azure-tutorial-migrate.md)
- [AWS örneklerini Azure’a geçirme](migrate-tutorial-aws-azure.md)
