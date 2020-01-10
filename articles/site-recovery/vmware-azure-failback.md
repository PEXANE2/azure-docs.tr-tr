---
title: VMware VM 'lerini/fiziksel sunucuları Azure 'dan Azure Site Recovery ile geri devretmek
description: VMware VM 'Leri ve fiziksel sunucuları Azure 'a yük devretmeden sonra şirket içi siteye geri dönme işlemini öğrenin.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.date: 01/15/2019
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: cd4cc90fb102d517a47ba458619e22b8921dd498
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75495334"
---
# <a name="fail-back-vmware-vms-to-on-premises-site"></a>VMware VM 'lerini şirket içi siteye geri dönme

Bu makalede, Azure sanal makinelerinin [Azure Site Recovery](site-recovery-overview.md)ile Azure 'a [yük devretmesinin](site-recovery-failover.md) ardından Şirket içi bir siteye nasıl geri dönmesi yapılacağı açıklanır. Şirket içi çalışmaya başladıktan sonra, şirket içi VM 'Lerin Azure 'a çoğaltmaya başlaması için çoğaltmayı etkinleştirin.

## <a name="before-you-start"></a>Başlamadan önce

1. VMware yeniden [çalışma](failover-failback-overview.md#vmwarephysical-reprotectionfailback)hakkında bilgi edinin. 
2. Yeniden [çalışma için hazırlanma](vmware-azure-prepare-failback.md)adımlarını ve gerekli tüm bileşenlerin dağıtıldığını gözden geçirdiğinizden ve tamamladığınızdan emin olun. Bileşenler Azure 'da bir işlem sunucusu, bir şirket içi ana hedef sunucusu ve yeniden çalışma için bir VPN siteden siteye bağlantı (veya ExpressRoute özel eşlemesi) içerir.
3. Yeniden koruma ve yeniden çalışma [gereksinimlerini](vmware-azure-reprotect.md#before-you-begin) tamamladığınızdan ve Azure 'dan şirket içi siteye Çoğaltıldıklarından Azure VM 'lerinin [yeniden korunmasını etkinleştirmiş](vmware-azure-reprotect.md#enable-reprotection) olduğunuzdan emin olun. VM 'Lerin tekrar yük devri için bir çoğaltılan durumda olması gerekir.




## <a name="run-a-failover-to-fail-back"></a>Yük devretme işlemini yeniden çalıştırmak için çalıştırma

1. Azure VM 'lerinin yeniden korunduğundan ve şirket içi siteye çoğaltıldığından emin olun. 
    - Bir VM 'nin yeniden başlatılması için en az bir kurtarma noktası olması gerekir.
    - Bir kurtarma planını yeniden devretmek, plandaki tüm makinelerin en az bir kurtarma noktası olması gerekir.
2. Kasada **çoğaltılan öğeleri**> VM 'yi seçin. **Planlanmamış yük devretme**> VM 'ye sağ tıklayın.
3. **Yük devretmeyi onaylayın**bölümünde yük devretme yönünü (Azure 'dan) doğrulayın.
4. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin.
    - **En son** kurtarma noktasını kullanmanızı öneririz. Uygulamayla tutarlı nokta en son zaman noktası arkasında ve veri kaybına neden olur.
    - **En son** , kilitlenme ile tutarlı bir kurtarma noktasıdır.
    - **En son**Ile bir VM, en son kullanılabilir zaman noktasına yük devreder. Bir kurtarma planında çoklu VM tutarlılığı için bir çoğaltma grubunuz varsa, gruptaki her VM, zaman içindeki bağımsız en son noktaya yük devreder.
    - Uygulamayla tutarlı bir kurtarma noktası kullanıyorsanız, her sanal makine kullanılabilir en son noktasına geri dönebilir. Bir kurtarma planında bir çoğaltma grubu varsa, her grup ortak kullanılabilir kurtarma noktasını kurtarır.
5. Yük devretme başlar. Site Recovery Azure VM 'lerini kapatır.
6. Yük devretme tamamlandıktan sonra her şeyin beklendiği gibi çalıştığını denetleyin. Azure VM 'lerinin kapatıldığından emin olun. 
7. Her şey doğrulandıktan sonra, yük devretme işlemini tamamlamak için, **işleme**> VM 'ye sağ tıklayın. COMMIT, yük devredilen Azure VM 'yi kaldırır. 

> [!NOTE]
> Windows VM 'Leri için, Site Recovery yük devretme sırasında VMware araçlarını devre dışı bırakır. Windows VM 'nin yeniden çalışma sırasında, VMware araçları tekrar etkinleştirilir. 




## <a name="reprotect-from-on-premises-to-azure"></a>Şirket içinden Azure 'a yeniden koruma

Yeniden çalışma gerçekleştirildikten sonra, Azure VM 'Leri silinir. VM, şirket içi siteye geri dönün, ancak korunmaz. VM 'Leri tekrar Azure 'a Çoğaltmaya başlamak için aşağıdaki gibi:

1. Kasa > **çoğaltılan öğeler**' i seçin ve ardından **yeniden koru**' yı seçin.
2. Verileri Azure 'a geri göndermek için kullanılan işlem sunucusunu belirtin.
3. Yeniden koru işini başlatmak için **Tamam ' ı** seçin.

> [!NOTE]
> Bir şirket içi VM başladıktan sonra, aracının yapılandırma sunucusuna geri kaydedilmesi 15 dakikaya kadar sürer. Bu süre boyunca yeniden koruma başarısız olur ve aracının yüklenmediğini belirten bir hata iletisi döndürür. Bu durumda, birkaç dakika bekleyin ve yeniden koruyun.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi tamamlandıktan sonra şirket içi VM, Azure 'a çoğaltılır. Gerektiğinde, Azure 'a [başka bir yük devretme](site-recovery-failover.md) işlemi de çalıştırabilirsiniz.

