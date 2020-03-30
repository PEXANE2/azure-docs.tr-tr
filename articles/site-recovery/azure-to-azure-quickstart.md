---
title: Azure Site Kurtarma ile azure VM olağanüstü durum kurtarmayı ikincil bir bölgeye ayarlama
description: Azure Site Kurtarma hizmetini kullanarak bir Azure VM için başka bir Azure bölgesine olağanüstü durum kurtarma yı hızlandırın.
ms.topic: quickstart
ms.date: 03/27/2020
ms.custom: mvc
ms.openlocfilehash: e26c2a1f24a88dc979f4ec68de65afc618740c00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371899"
---
# <a name="quickstart-set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Hızlı başlatma: Azure VM için ikincil bir Azure bölgesine olağanüstü durum kurtarma ayarlama

[Azure Site Kurtarma](site-recovery-overview.md) hizmeti, planlı ve planlanmamış kesintiler sırasında iş uygulamalarınızı çevrimiçi tutarak iş sürekliliği nize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Kurtarma, çoğaltma, başarısız olma ve kurtarma dahil olmak üzere şirket içi makinelerin ve Azure sanal makinelerinin (VM) olağanüstü durum kurtarmalarını yönetir ve yönetir.

Bu hızlı başlatma, bir Azure VM'yi ikincil bir Azure bölgesine kopyalayarak olağanüstü durum kurtarmanın nasıl ayarlanabildiğini açıklar. Genel olarak, varsayılan ayarlar çoğaltmayı etkinleştirmek için kullanılır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliği ve VM gerekir.

- Etkin aboneliği olan bir Azure hesabınız yoksa, [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- En az 1 GB RAM'e sahip bir VM önerilir. VM oluşturma hakkında [daha fazla bilgi edinin.](/azure/virtual-machines/windows/quick-create-portal)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure VM için çoğaltmayı etkinleştirme

Aşağıdaki adımlar, VM çoğaltmayı ikincil bir konuma sağlar.

1. Azure portalında, **Ev** > **Sanal makineleri** menüsünden çoğaltmak için bir VM seçin.
1. **Operasyonlarda** **Olağanüstü Durum kurtarma**seçin.
1. **Temel ler** > **Hedef bölgesinden**hedef bölgeyi seçin.
1. Çoğaltma ayarlarını görüntülemek için **Gözden Geçir + Çoğaltma başlat'ı'nı**seçin. Herhangi bir varsayılan ı değiştirmeniz gerekiyorsa, **Gelişmiş ayarları**seçin.
1. VM çoğaltmasını sağlayan işi başlatmak için **Başlat çoğaltma'yı**seçin.

   :::image type="content" source="media/azure-to-azure-quickstart/enable-replication1.png" alt-text="Çoğaltmayı etkinleştirin.":::

## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işi bittikten sonra çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı sınayabilirsiniz.

1. Azure portalı menüsünde **Sanal makineleri** seçin ve çoğaltacağınız VM'yi seçin.
1. **Operasyonlarda** **Olağanüstü Durum kurtarma**seçin.
1. **Genel Bakış'tan** çoğaltma ayrıntılarını görüntülemek için **Temel Bilgileri**seçin. Daha fazla bilgi **Sağlık ve durum**gösterilir , **Failover hazırlık**, ve Altyapı **görünüm** haritası.

   :::image type="content" source="media/azure-to-azure-quickstart/replication-status.png" alt-text="Çoğaltma durumu.":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

VM'nin birincil bölgede çoğaltılmasını durdurmak için çoğaltmayı devre dışı bırakmanız gerekir:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir.
- Çoğaltma sırasında VM'ye yüklenen Site Kurtarma uzantısı kaldırılmaz.
- VM için Site Kurtarma faturalandırması durur.

Çoğaltmayı devre dışı atmak için şu adımları yapın:

1. Azure portalı menüsünde **Sanal makineleri** seçin ve çoğaltacağınız VM'yi seçin.
1. **Operasyonlarda** **Olağanüstü Durum kurtarma**seçin.
1. Genel **Bakış'tan**Çoğaltmayı **Devre Dışı**Nı seçin.
1. Site Kurtarma uzantısını kaldırmak için VM'nin **Ayarlar** > **Uzantıları'na**gidin.

   :::image type="content" source="media/azure-to-azure-quickstart/disable2-replication.png" alt-text="Çoğaltmayı devre dışı.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tek bir VM’yi ikincil bir bölgeye çoğalttınız. Ardından, birden çok Azure VM'si için çoğaltma ayarlayın.

> [!div class="nextstepaction"]
> [Azure VM'leri için olağanüstü durum kurtarma ayarlama](azure-to-azure-tutorial-enable-replication.md)
