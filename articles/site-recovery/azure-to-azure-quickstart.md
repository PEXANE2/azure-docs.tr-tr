---
title: Azure VM olağanüstü durum kurtarmayı Azure Site Recovery ile ikincil bir bölgeye ayarlama
description: Azure Site Recovery hizmetini kullanarak Azure VM için başka bir Azure bölgesine olağanüstü durum kurtarmayı hızlıca ayarlayın.
ms.topic: quickstart
ms.date: 03/27/2020
ms.custom: mvc
ms.openlocfilehash: 8681ccaac30402927005e5e8802d7da2bddc2c5c
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135712"
---
# <a name="quickstart-set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Hızlı başlangıç: Azure VM için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çevrimiçi tutarak iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin (VM) çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarmasını yönetip yönetmektedir.

Bu hızlı başlangıçta, bir Azure VM için, ikincil bir Azure bölgesine çoğaltılarak olağanüstü durum kurtarmanın nasıl ayarlanacağı açıklanır. Genel olarak, çoğaltmayı etkinleştirmek için varsayılan ayarlar kullanılır.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlayabilmeniz için bir Azure aboneliğine ve VM 'ye ihtiyacınız vardır.

- Etkin aboneliği olan bir Azure hesabınız yoksa, [ücretsiz olarak bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- En az 1 GB RAM 'e sahip bir VM önerilir. VM oluşturma hakkında [daha fazla bilgi edinin](../virtual-machines/windows/quick-create-portal.md) .

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com) oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure VM için çoğaltmayı etkinleştirme

Aşağıdaki adımlar, VM çoğaltmasını ikincil bir konuma etkinleştirir.

1. Azure Portal, **ana**  >  **sanal makineler** menüsünde, çoğaltılacak bir VM seçin.
1. **İşlemler** ' de **olağanüstü durum kurtarma**' yı seçin.
1. **Temel bilgiler**  >  **hedef bölgesi**' nden hedef bölgeyi seçin.
1. Çoğaltma ayarlarını görüntülemek için, **gözden geçir + çoğaltmayı Başlat**' ı seçin. Herhangi bir varsayılanı değiştirmeniz gerekiyorsa, **Gelişmiş ayarlar**' ı seçin.
1. VM çoğaltmasını sağlayan işi başlatmak için **çoğaltmayı Başlat**' ı seçin.

   :::image type="content" source="media/azure-to-azure-quickstart/enable-replication1.png" alt-text="Çoğaltmayı etkinleştirin.":::

## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. Azure portal menüsünde, **sanal makineler** ' i seçin ve çoğaltılan VM 'yi seçin.
1. **İşlemler** ' de **olağanüstü durum kurtarma**' yı seçin.
1. **Genel Bakış ' a** ait çoğaltma ayrıntılarını görüntülemek için **Essentials**' ı seçin. Daha fazla ayrıntı **sistem durumu ve durumu**, **Yük devretme hazırlığı**ve **altyapı görünümü** eşlemesinde gösterilmektedir.

   :::image type="content" source="media/azure-to-azure-quickstart/replication-status.png" alt-text="Çoğaltma durumu.":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Birincil bölgedeki VM çoğaltmasını durdurmak için çoğaltmayı devre dışı bırakmanız gerekir:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir.
- Çoğaltma sırasında VM 'de yüklü Site Recovery uzantısı kaldırılmaz.
- VM için Site Recovery faturalandırma durduruluyor.

Çoğaltmayı devre dışı bırakmak için şu adımları uygulayın:

1. Azure portal menüsünde, **sanal makineler** ' i seçin ve çoğaltılan VM 'yi seçin.
1. **İşlemler** ' de **olağanüstü durum kurtarma**' yı seçin.
1. **Genel bakışta** **çoğaltmayı devre dışı bırak**' ı seçin.
1. Site Recovery uzantısını kaldırmak için VM 'nin **Ayarlar**  >  **uzantılarına**gidin.

   :::image type="content" source="media/azure-to-azure-quickstart/disable2-replication.png" alt-text="Çoğaltmayı devre dışı bırakın.":::

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tek bir VM’yi ikincil bir bölgeye çoğalttınız. Sonra, birden fazla Azure VM için çoğaltmayı ayarlayın.

> [!div class="nextstepaction"]
> [Azure VM 'Leri için olağanüstü durum kurtarmayı ayarlama](azure-to-azure-tutorial-enable-replication.md)
