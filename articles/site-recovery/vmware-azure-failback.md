---
title: Azure Site Kurtarma ile Azure'dan VMware VM'leri/fiziksel sunucularını geri başarısız
description: VMware VM'lerinin ve fiziksel sunucuların Azure'a olağanüstü kurtarma sırasında Azure'a başarısız olduktan sonra şirket içi siteye nasıl geri dönülemeyeceğinizi öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.date: 01/15/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: cd4cc90fb102d517a47ba458619e22b8921dd498
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495334"
---
# <a name="fail-back-vmware-vms-to-on-premises-site"></a>VMware VM'leri şirket içi siteye geri getirin

Bu makalede, şirket içi VM'lerin [Azure Site Kurtarma](site-recovery-overview.md)ile Azure'a geri [alamaması](site-recovery-failover.md) sonrasında Azure VM'leri şirket içi bir siteye nasıl geri dönülemeyeceği açıklanmaktadır. Şirket içi geri dönüşten sonra, şirket içi VM'lerin Azure'da çoğalmaya başlaması için çoğaltmayı etkinleştirin.

## <a name="before-you-start"></a>Başlamadan önce

1. [VMware failback](failover-failback-overview.md#vmwarephysical-reprotectionfailback)hakkında bilgi edinin. 
2. [Failback'e hazırlanmak için](vmware-azure-prepare-failback.md)adımları gözden aldığınızdan ve tamamladığınızdan ve gerekli tüm bileşenlerin dağıtıldığınızdan emin olun. Bileşenler arasında Azure'da bir işlem sunucusu, şirket içi ana hedef sunucu ve failback için VPN siteden siteye bağlantı (veya ExpressRoute özel bakış) yer alıyor.
3. Yeniden koruma ve geri ödeme [gereksinimlerini](vmware-azure-reprotect.md#before-you-begin) tamamladığınızdan ve Azure VM'lerinin [yeniden korunmasını etkinleştirdiğinizden](vmware-azure-reprotect.md#enable-reprotection) emin olun, böylece Azure'dan şirket içi siteye çoğalıyorlar. VM'ler çoğaltılmış durumda olmalıdır geri başarısız olmak için sıralanır.




## <a name="run-a-failover-to-fail-back"></a>Geri başarısız olmak için bir failover çalıştırın

1. Azure VM'lerinin şirket içinde yeniden korunduğundan ve site içinde çoğaltıldığından emin olun. 
    - Bir VM'nin geri başarısız olması için en az bir kurtarma noktasına ihtiyacı vardır.
    - Bir kurtarma planı geri başarısız olursa, o zaman plandaki tüm makineler en az bir kurtarma noktası olmalıdır.
2. **Çoğaltılan öğeleri**> kasasında VM'yi seçin. VM > **Plansız Failover'a**sağ tıklayın.
3. **Failover'ı onaylayın'da,**(Azure'dan) başarısız yönü doğrulayın.
4. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin.
    - **En Son** kurtarma noktasını kullanmanızı öneririz. Uygulama tutarlı noktası, zaman içinde en son noktanın gerisinde dir ve bazı veri kaybına neden olur.
    - **En sonuncusu,** çarpışma tutarlı bir kurtarma noktasıdır.
    - **En son**ile, bir VM zaman içinde en son kullanılabilir noktasına üzerinde başarısız olur. Bir kurtarma planı içinde çoklu VM tutarlılığı için bir çoğaltma grubuna sahipseniz, gruptaki her VM, bağımsız son noktasına kadar başarısız olur.
    - Uygulama tutarlı bir kurtarma noktası kullanırsanız, her VM en son kullanılabilir noktasına geri başarısız olur. Kurtarma planının bir çoğaltma grubu varsa, her grup kullanılabilir ortak kurtarma noktasına geri kurtarır.
5. Failover başlıyor. Site Kurtarma, Azure VM'lerini kapatır.
6. Failover tamamlandıktan sonra, her şeyin beklendiği gibi çalıştığını kontrol edin. Azure VM'lerinin kapatılıp kapatılmadığını kontrol edin. 
7. Her şey doğrulandı, sağ VM > **Commit**tıklayın , başarısız lık işlemini bitirmek için. Commit, başarısız olan Azure VM'sini kaldırır. 

> [!NOTE]
> Windows VM'ler için Site Kurtarma, başarısız lık sırasında VMware araçlarını devre dışı kılabilir. Windows VM'nin geri tepmesi sırasında, VMware araçları yeniden etkinleştirilir. 




## <a name="reprotect-from-on-premises-to-azure"></a>Şirket içi Azure'a karşı yeniden koruyun

Başarısız geri ödeme dendikten sonra Azure VM'leri silinir. VM tesis bünyesinde geri döndü, ancak korunmuyor. VM'leri yeniden Azure'a çoğaltmaya başlamak için aşağıdaki gibi:

1. Yinelenen **öğeler>** kasasında başarısız geri VM'leri seçin ve ardından **Yeniden Koruma'yı**seçin.
2. Azure'a veri göndermek için kullanılan işlem sunucusunu belirtin.
3. Yeniden koruma işine başlamak için **Tamam'ı** seçin.

> [!NOTE]
> Şirket içi VM başladıktan sonra aracının yapılandırma sunucusuna geri kaydolması 15 dakika kadar sürer. Bu süre zarfında, yeniden koruma başarısız olur ve aracının yüklü olmadığını belirten bir hata iletisi döndürür. Bu durumda, birkaç dakika bekleyin ve yeniden koruyun.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi bittikten sonra şirket içi VM Azure'a çoğalıyor. Gerektiğinde Azure'a [başka bir hata yapabilirsiniz.](site-recovery-failover.md)

