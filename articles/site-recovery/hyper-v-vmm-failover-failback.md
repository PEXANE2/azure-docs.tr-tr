---
title: Azure Site Kurtarma ile ikincil bir Hyper-V sitesine failover/failback ayarlama
description: Azure Site Kurtarma ile olağanüstü durum kurtarma sırasında Hyper-V VM'ler üzerinde ikincil şirket içi sitenizde nasıl başarısız oylanın ve birincil siteye geri dönmeyi öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: d31355bcb0ce42874c19988738ba06138c7a0b7c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082595"
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-your-secondary-on-premises-site"></a>Başarısız ve ikincil şirket içi siteye çoğaltılan Hyper-V VM'leri geri başarısız

[Azure Site Kurtarma](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltılması, başarısızlığı ve başarısızlığı yönetir ve yönetir.

Bu makalede, ikincil bir VMM sitesine, Sistem Merkezi Sanal Makine Yöneticisi (VMM) bulutunda yönetilen bir Hyper-V VM üzerinde nasıl başarısız olunacak şekilde açıklanmaktadır. Yük devrettikten sonra şirket içi siteniz kullanılabilir olduğunda yeniden çalıştırabilirsiniz. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Birincil VMM bulutundan ikincil vmm bulutuna hyper-v VM üzerinden başarısız
> * Birincil siteye ikincil siteden yeniden koruyun ve geri başarısız
> * İsteğe bağlı olarak birincilden ikincile yeniden çoğaltma başlatma

## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma

Failover ve failback üç aşamadan oluşur:

1. **İkincil siteye geçeme**: Birincil siteden ikincil siteye makineleri başarısız.
2. **İkincil siteden geri başarısız :** VM'leri ikincilden birincile çoğaltma ve başarısız olmak için planlanmış bir başarısızlık çalıştırın.
3. Planlanan hatadan sonra, isteğe bağlı olarak birincil siteden ikincil siteye yeniden çoğaltma başlar.


## <a name="prerequisites"></a>Ön koşullar

- Her şeyin beklendiği gibi çalışıp çalışmadığını kontrol etmek için bir [olağanüstü durum kurtarma matkabını](hyper-v-vmm-test-failover.md) tamamladığınızdan emin olun.
- Başarısız lığı tamamlamak için, birincil ve ikincil VMM sunucularının Site Kurtarma'ya bağlı olduğundan emin olun.



## <a name="run-a-failover-from-primary-to-secondary"></a>Birincilden ikincile bir başarısızlık çalıştırma

Hyper-V VM'ler için düzenli veya planlı bir arıza çalıştırabilirsiniz.

- Beklenmeyen kesintiler için düzenli bir arıza kullanın. Bu başarısızı çalıştırdığınızda, Site Kurtarma ikincil sitede bir VM oluşturur ve bu hatayı güçlendirir. Veri kaybı, eşitlenmemiş bekleyen verilere bağlı olarak oluşabilir.
- Planlı bir arıza, bakım için veya beklenen kesinti sırasında kullanılabilir. Bu seçenek sıfır veri kaybı sağlar. Planlı bir hata tetiklendiğinde, kaynak VM'ler kapatılır. Eşitlenmemiş veriler eşitlenir ve hata üzerinde tetiklenir. 
- 
  Bu yordam, düzenli bir başarısızlık nasıl çalıştırılmayı açıklar.


1. **Ayarlar** > **Çoğaltılan öğelerde** VM > **Failover'ı**tıklatın.
1. Site Kurtarma'nın başarısızlığı tetiklemeden önce kaynak VM'lerin kapatılmasını denemesini istiyorsanız, başarısız olmadan **önce kapatma makinesini** seçin. Site Kurtarma, başarısızolmayı tetiklemeden önce, henüz ikincil siteye gönderilmemiş şirket içi verileri eşitlemeye de çalışır. Kapatma başarısız olsa bile başarısız ın devam ettiğini unutmayın. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
2. Artık ikincil VMM bulutundaki VM'yi görebilmelisin.
3. VM'yi doğruladıktan sonra, başarısızı **gerçekleştirin.** Bu, tüm kullanılabilir kurtarma noktalarını siler.

> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılamaz.  


## <a name="reverse-replicate-and-failover"></a>Ters çoğaltma ve başarısız

İkincil siteden birincil siteye çoğaltmaya başlayın ve birincil siteye geri başarısız. VM'ler birincil sitede yeniden çalışmaya başladıktan sonra, bunları ikincil siteye çoğaltabilirsiniz.  

 
1. **Ters Çoğaltma'ya**tıklayın > VM'yi tıklatın.
2. İş tamamlandıktan sonra, **Failover'daki**VM >'yi tıklatın, başarısız yönü doğrulayın (ikincil VMM bulutundan) ve kaynak ve hedef konumlarını seçin. 
4. Yük devretmeyi başlatın. Yük devretme işleminin ilerleme durumunu **İşler** sekmesinden takip edebilirsiniz.
5. Birincil VMM bulutunda, VM'nin kullanılabilir olup olmadığını kontrol edin.
6. Birincil VM'yi tekrar ikincil siteye çoğaltmaya başlamak istiyorsanız, **Ters Çoğaltma'yı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
Hyper-V [VM'leri](hyper-v-vmm-disaster-recovery.md) ikincil bir siteye çoğaltma adımı gözden geçirin.
