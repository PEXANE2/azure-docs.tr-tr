---
title: Azure Site Kurtarma ile Azure VM olağanüstü durum kurtarma matkabını çalıştırma
description: Azure Site Kurtarma hizmetini kullanarak Azure VM'leri için ikincil bir bölgeye olağanüstü durum kurtarma matkabını nasıl çalıştıracaklarınız hakkında bilgi edinin.
services: site-recovery
ms.topic: tutorial
ms.date: 01/16/2020
ms.custom: mvc
ms.openlocfilehash: b2ce157f0f192135ab0507e4aae4c0a282bda1ea
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76166188"
---
# <a name="run-a-disaster-recovery-drill-to-a-secondary-region-for-azure-vms"></a>Azure VM'leri için ikincil bir bölgeye olağanüstü durum kurtarma matkabı çalıştırma

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, bir Azure VM için yük devretme testiyle bir Azure bölgesinden diğerine nasıl olağanüstü durum kurtarma tatbikatı çalıştıracağınızı gösterir. Tatbikat, çoğaltma stratejinizi veri kaybı veya kesinti süresi olmadan doğrular ve üretim ortamınızı etkilemez. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları denetleme
> * Tek bir VM için yük devretme testi çalıştırma

> [!NOTE]
> Bu öğretici, en az adımla bir olağanüstü durum kurtarma matkabı gerçekleştirmenize yardımcı olur. Olağanüstü durum kurtarma demlemesi ile ilgili çeşitli işlevler hakkında daha fazla bilgi edinmek için Azure VM [çoğaltma,](azure-to-azure-how-to-enable-replication.md) [ağ,](azure-to-azure-about-networking.md) [otomasyon](azure-to-azure-powershell.md)veya [sorun giderme](azure-to-azure-troubleshoot-errors.md)belgelerine bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi yapmadan önce aşağıdaki öğeleri kontrol edin:

- Bir test başarısızlığını çalıştırmadan önce, olağanüstü durum kurtarma için yapılandırıldığından emin olmak için VM'nin özelliklerini kontrol etmenizi öneririz. Çoğaltma ve başarısız özellikleri görüntülemek için VM's **Operations** > **Disaster Recovery** > **Properties** gidin.
- Çoğaltmayı etkinleştirdiğinizde ayarlanan varsayılan ağ için değil, **test başarısızlığı için ayrı bir Azure VM ağı kullanmanızı öneririz.**
- Her NIC için kaynak ağ yapılandırmalarınıza bağlı olarak, olağanüstü durum kurtarma matkabını yapmadan önce **Bilgi İşlem ve Ağ'daki** test failover ayarları altında her NIC'e eklemek için **Subnet,** **Özel IP adresi,** **Genel IP,** **Ağ güvenlik grubu**veya **Yük dengeleyicisi** belirtebilirsiniz.

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

Bu örnek, VM testi başarısız olmak için Kurtarma Hizmetleri kasasının nasıl kullanılacağını gösterir.

1. Bir kasa seçin ve **Korumalı öğeler** > **Çoğaltılan öğelere** gidin ve bir VM seçin.
1. **Test Failover'da,** başarısız olmak için kullanılacak bir kurtarma noktası seçin:
   - **Son**: Site Kurtarma'daki tüm verileri işler ve en düşük RTO'yu (Kurtarma Süresi Hedefi) sağlar.
   - **En son işlenen**: VM yükü, Site Recovery tarafından işlenen en son kurtarma noktasına devredilir. Zaman damgası gösterilir. Bu seçenekle, veri işleme için zaman harcanması yapılmaz, bu nedenle düşük bir RTO sağlar.
   - **Uygulamayla tutarlı olan son**: Bu seçenek, tüm VM’lerin yükünü uygulamayla tutarlı olan en son kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Belirli kurtarma noktasına başarısız. Özel, yalnızca tek bir VM üzerinden başarısız olduğunuzda kullanılabilir ve kurtarma planıyla başarısız olduğunuzda kullanılamaz.
1. İkinci bölgedeki Azure Sanal M'lerinin başarısız olduktan sonra bağlanacakları hedef Azure sanal ağını seçin.

   > [!NOTE]
   > Test başarısız ayarları çoğaltılan öğe için önceden yapılandırılmışsa, bir Azure sanal ağ seçmek için açılan menü görünmez.

1. Başarısız olmayı başlatmak için **Tamam'ı**seçin. Kasadaki ilerlemeyi izlemek **için** > İzleme**Sitesi Kurtarma işlerine** gidin ve Test **Failover** işini seçin.
1. Başarısız lık bittikten sonra, Azure VM çoğaltması Azure portalının **Sanal Makineleri'nde**görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
1. Test başarısızolması sırasında oluşturulan VM'leri silmek için, çoğaltılan öğede veya kurtarma planında **Temizleme testi başarısız** olmasını seçin. **Notlar**’da, yük devretme testiyle ilişkili gözlemlerinizi kaydedin ve saklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim yük devretmesini çalıştırma](azure-to-azure-tutorial-failover-failback.md)
