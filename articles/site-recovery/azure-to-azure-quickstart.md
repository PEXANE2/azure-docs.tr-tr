---
title: Bir Azure IaaS VM’si için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama
description: Bu hızlı başlangıçta Azure Site Recovery hizmetini kullanarak farklı Azure bölgeleri arasında Azure IaaS VM olağanüstü durum kurtarma gerçekleştirmek için gerekli adımlar gösterilmektedir.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: quickstart
ms.date: 08/28/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d23d16d1315eeaf224a5291641ab088212fcdc77
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70146947"
---
# <a name="set-up-disaster-recovery-to-a-secondary-azure-region-for-an-azure-vm"></a>Bir Azure VM’si için ikincil Azure bölgesine olağanüstü durum kurtarma ayarlama        

[Azure Site Recovery](site-recovery-overview.md) hizmeti, planlı ve plansız kesintiler sırasında iş uygulamalarınızı çalışır durumda tutarak, iş sürekliliğinize ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu hızlı başlangıçta, farklı bir Azure bölgesine çoğaltılarak bir Azure VM için olağanüstü durum kurtarmanın nasıl ayarlanacağı açıklanır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!NOTE]
> Bu makale, yeni kullanıcılar için hızlı bir yol. Varsayılan Seçenekler ve en düşük özelleştirme ile en basit yolu kullanır.  Daha kapsamlı bir anlatım için [Öğreticimizi](azure-to-azure-tutorial-enable-replication.md)gözden geçirin.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.

## <a name="enable-replication-for-the-azure-vm"></a>Azure VM için çoğaltmayı etkinleştirme

1. Azure portalında, **Sanal makineler** seçeneğine tıklayın ve çoğaltmak istediğiniz VM’yi seçin.
2. **İşlemler** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
3. **Olağanüstü durumdan kurtarma yapılandırma** > **Hedef bölge** bölümünde, çoğaltma yapacağınız hedef bölgeyi seçin.
4. Bu Hızlı Başlangıç için, diğer varsayılan ayarları kabul edin.
5. **Çoğaltmayı etkinleştir**’e tıklayın. Bu, sanal makineye yönelik çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![çoğaltmayı etkinleştir](media/azure-to-azure-quickstart/enable-replication1.png)

## <a name="verify-settings"></a>Ayarları doğrulama

Çoğaltma işlemi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. **İşlemler** menüsünde **Olağanüstü durum kurtarma** seçeneğine tıklayın.
2. Çoğaltma durumunu, oluşturulan kurtarma noktalarını ve haritadaki kaynak ve hedef bölgelerini doğrulayabilirsiniz.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Çoğaltma işlemini devre dışı bıraktığınızda, birincil bölgedeki VM çoğaltmayı durdurur:

- Kaynak çoğaltma ayarları otomatik olarak temizlenir. Çoğaltma kapsamında VM 'de yüklü Site Recovery uzantısı kaldırılmaz ve el ile kaldırılmalıdır. 
- VM için Site Recovery faturalandırma durduruluyor.

Çoğaltmayı aşağıdaki şekilde durdur

1. VM’yi seçin.
2. **Olağanüstü durum kurtarma** bölümünde **Çoğaltmayı devre dışı bırak**'a tıklayın.

   ![Çoğaltmayı devre dışı bırakma](media/azure-to-azure-quickstart/disable2-replication.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, tek bir VM’yi ikincil bir bölgeye çoğalttınız. Şimdi, bir kurtarma planı kullanarak birden fazla Azure VM 'yi çoğaltmayı deneyin.

> [!div class="nextstepaction"]
> [Azure VM’leri için olağanüstü durum kurtarmayı yapılandır](azure-to-azure-tutorial-enable-replication.md)
