---
title: Azure Site Recovery hizmeti ile olağanüstü durum kurtarma için ikincil bir Azure bölgesine çoğaltılan Azure VM 'Leri geri devretmek.
description: Azure Site Recovery hizmeti ile Azure VM 'lerini nasıl geri yükleyeceğinizi öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 8eb329ae019f125b98c475b4a77d19e9cafe23fa
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053812"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Azure bölgeleri arasında Azure VM geri dönme

[Azure Site Recovery](site-recovery-overview.md) hizmeti, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve yeniden çalışma işlemlerini yönetip düzenleyerek olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu öğreticide, tek bir Azure VM 'nin nasıl geri yükleneceği açıklanmaktadır. Yük devretdikten sonra, kullanılabilir olduğunda birincil bölgeye yeniden yük devredebilmeniz gerekir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> 
> * İkincil bölgedeki VM 'yi yeniden devreder.
> * Birincil VM 'yi ikincil bölgeye yeniden koruyun.
> 
> [!NOTE]
> 
> Bu öğretici, bir hedef bölgeye birkaç VM 'nin yükünü devretmek ve en düşük özelleştirmeler ile kaynak bölgeye geri dönmek için size yardımcı olur. Daha ayrıntılı yönergeler için [Azure VM 'lerinde nasıl yapılır kılavuzlarını](https://docs.microsoft.com/azure/virtual-machines/windows/)gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

* VM durumunun **Yük devretme taahhütine**sahip olduğundan emin olun.
* Birincil bölgenin kullanılabilir olup olmadığını ve içinde yeni kaynaklar oluşturup bu kaynaklara erişebiliyor olduğunuzdan emin olun.
* Yeniden korumanın etkinleştirildiğinden emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

VM 'Ler yeniden korunduktan sonra, gerekirse birincil bölgeye yeniden yük devreedebilirsiniz.

1. Kasada **çoğaltılan öğeler**' i seçin ve ardından YENIDEN korunan VM 'yi seçin.

    ![Birincili yeniden çalışma](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. **Çoğaltılan öğeler**' de VM ' yi seçin ve ardından **Yük devretme**' yı seçin.
3. **Yük devretme**bölümünde yük devretmek için bir kurtarma noktası seçin:
    - **En son (varsayılan)** : Site Recovery hizmetindeki tüm verileri işler ve en düşük kurtarma noktası HEDEFINI (RPO) sağlar.
    - **En son işlenen**: VM 'yi, Site Recovery tarafından işlenen en son kurtarma noktasına geri döndürür.
    - **Özel**: belirli bir kurtarma noktasına yük devreder. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.
4. Yük devretmeyi tetiklemeden önce DR bölgesindeki VM 'lerin kapatılmasını denemesini Site Recovery istiyorsanız, **yük devretmeye başlamadan önce makineyi Kapat ' ı** seçin. Yük devretme işlemi, kapatılma başarısız olsa bile devam eder. 
5. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.
6. Yük devretme tamamlandıktan sonra, üzerinde oturum açarak VM 'yi doğrulayın. Kurtarma noktasını gerektiği gibi değiştirebilirsiniz.
7. Yük devretmeyi doğruladıktan sonra **Yük devretmeyi Yürüt**' ü seçin. Yürütülüyor, tüm kullanılabilir kurtarma noktalarını siler. Kurtarma noktasını Değiştir seçeneği artık kullanılamıyor.
8. VM, yük devredildi ve geri başarısız olarak göstermelidir.

    ![Birincil ve ikincil bölgelerde VM](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

> [!NOTE]
> Site Recovery uzantısı sürümü 9.28. x. x onler [güncelleştirme paketi 40](https://support.microsoft.com/help/4521530/update-rollup-40-for-azure-site-recovery) Site Recovery çalıştıran makineler için ikincil olağanüstü durum kurtarma bölgesindeki makineleri temizler, NAfter yeniden çalışma tamamlanmıştır ve VM 'ler yeniden korunur. VM 'Leri ve NIC 'leri el ile silmeniz gerekmez.

## <a name="next-steps"></a>Sonraki adımlar

Yeniden koruma akışı hakkında [daha fazla bilgi edinin](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) .
