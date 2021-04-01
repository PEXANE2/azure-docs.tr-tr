---
title: Azure VMware Çözüm VMsfrom Azure 'da yeniden başarısız oldu Azure Site Recovery
description: Olağanüstü durum kurtarma sırasında Azure 'a yük devretmeden sonra Azure VMware Çözüm özel bulutuna yeniden nasıl geri döneceğinizi öğrenin.
author: Harsha-CS
manager: rochakm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/30/2020
ms.author: harshacs
ms.custom: MVC
ms.openlocfilehash: fb14e647d3444f2f0d0cb86901f93582a18848f5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91814616"
---
# <a name="fail-back-vms-to-azure-vmware-solution-private-cloud"></a>VM 'Leri Azure VMware çözümü özel bulutuna geri dönme

Bu makalede, Azure VMware Çözüm VM 'lerinin Azure 'da [Azure Site Recovery](site-recovery-overview.md)ile Azure 'a [Yük devretmenin](avs-tutorial-failover.md) ardından Azure VMware Çözüm özel bulutuna nasıl yeniden kullanılacağı açıklanmaktadır. Yeniden çalışma sonrasında, Azure VMware Çözüm VM 'lerinin Azure 'a çoğaltmaya başlaması için çoğaltmayı etkinleştirirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

1. VMware yeniden [çalışma](failover-failback-overview.md#vmwarephysical-reprotectionfailback)hakkında bilgi edinin. 
2. Yeniden [çalışma için hazırlanma](vmware-azure-prepare-failback.md)adımlarını ve gerekli tüm bileşenlerin dağıtıldığını gözden geçirdiğinizden ve tamamladığınızdan emin olun. Bileşenler Azure 'da bir işlem sunucusu, bir ana hedef sunucu ve yeniden çalışma için bir VPN siteden siteye bağlantı (veya ExpressRoute özel eşlemesi) içerir.
3. Yeniden koruma ve yeniden çalışma [gereksinimlerini](avs-tutorial-reprotect.md#before-you-begin) tamamladığınızdan ve Azure 'Dan Azure VMware Çözüm özel bulutuna Çoğaltıldıklarından Azure VM 'lerinin [yeniden korunmasını etkinleştirmiş](avs-tutorial-reprotect.md#enable-reprotection) olduğunuzdan emin olun. VM 'Lerin tekrar yük devri için bir çoğaltılan durumda olması gerekir.




## <a name="run-a-failover-to-fail-back"></a>Yük devretme işlemini yeniden çalıştırmak için çalıştırma

1. Azure VM 'lerinin yeniden korunduğundan ve Azure VMware Çözüm özel bulutuna çoğaltıldığından emin olun.
    - Bir VM 'nin yeniden başlatılması için en az bir kurtarma noktası olması gerekir.
    - Bir kurtarma planını yeniden devretmek, plandaki tüm makinelerin en az bir kurtarma noktası olması gerekir.
2. Kasada **çoğaltılan öğeleri**> VM 'yi seçin. **Planlanmamış yük devretme**> VM 'ye sağ tıklayın.
3. **Yük devretmeyi onaylayın** bölümünde yük devretme yönünü (Azure 'dan) doğrulayın.
4. Yük devretme için kullanmak istediğiniz kurtarma noktasını seçin.
    - **En son** kurtarma noktasını kullanmanızı öneririz. Uygulamayla tutarlı nokta en son zaman noktası arkasında ve veri kaybına neden olur.
    - **En son** , kilitlenme ile tutarlı bir kurtarma noktasıdır.
    - **En son** Ile bir VM, en son kullanılabilir zaman noktasına yük devreder. Bir kurtarma planında çoklu VM tutarlılığı için bir çoğaltma grubunuz varsa, gruptaki her VM, zaman içindeki bağımsız en son noktaya yük devreder.
    - Uygulamayla tutarlı bir kurtarma noktası kullanıyorsanız, her sanal makine kullanılabilir en son noktasına geri dönebilir. Bir kurtarma planında bir çoğaltma grubu varsa, her grup ortak kullanılabilir kurtarma noktasını kurtarır.
5. Yük devretme başlar. Site Recovery Azure VM 'lerini kapatır.
6. Yük devretme tamamlandıktan sonra her şeyin beklendiği gibi çalıştığını denetleyin. Azure VM 'lerinin kapatıldığından emin olun. 
7. Her şey doğrulandıktan sonra, yük devretme işlemini tamamlamak için, **işleme**> VM 'ye sağ tıklayın. COMMIT, yük devredilen Azure VM 'yi kaldırır. 

> [!NOTE]
> Windows VM 'Leri için, Site Recovery yük devretme sırasında VMware araçlarını devre dışı bırakır. Windows VM 'nin yeniden çalışma sırasında, VMware araçları tekrar etkinleştirilir. 




## <a name="reprotect-from-azure-vmware-solution-to-azure"></a>Azure VMware çözümünden Azure 'a yeniden koruma

Yeniden çalışma gerçekleştirildikten sonra, Azure VM 'Leri silinir. VM, Azure VMware Çözüm özel bulutuna geri dönün, ancak korunmaz. VM 'Leri tekrar Azure 'a Çoğaltmaya başlamak için aşağıdaki gibi:

1. Kasa > **çoğaltılan öğeler**' i seçin ve ardından **yeniden koru**' yı seçin.
2. Verileri Azure 'a geri göndermek için kullanılan işlem sunucusunu belirtin.
3. Yeniden koru işini başlatmak için **Tamam ' ı** seçin.

> [!NOTE]
> Bir Azure VMware Çözüm VM 'si başladıktan sonra, aracının yapılandırma sunucusuna geri kaydedilmesi 15 dakikaya kadar sürer. Bu süre boyunca yeniden koruma başarısız olur ve aracının yüklenmediğini belirten bir hata iletisi döndürür. Bu durumda, birkaç dakika bekleyin ve yeniden koruyun.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma işi tamamlandıktan sonra Azure VMware Çözüm VM 'si Azure 'a çoğaltılır. Gerektiğinde, Azure 'a [başka bir yük devretme](avs-tutorial-failover.md) işlemi de çalıştırabilirsiniz.

