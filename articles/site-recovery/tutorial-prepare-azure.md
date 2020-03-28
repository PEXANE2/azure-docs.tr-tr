---
title: Azure Site Kurtarma ile Azure'u şirket içi olağanüstü durum kurtarma için hazırlayın
description: Azure Site Recovery ile şirket içi makinelerin olağanüstü durum kurtarma işlemleri için Azure’ın nasıl hazırlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ba5ecc29edbcd69324500e87add846e4395ce0a3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067559"
---
# <a name="prepare-azure-for-on-premises-disaster-recovery-to-azure"></a>Azure'u şirket içi olağanüstü durum kurtarma için Azure'a hazırlayın

Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak şirket içi VMware VM'ler, Hyper-V VM'ler veya Windows/Linux fiziksel sunucularının Azure'a olağanüstü kurtarma sını ayarlamak için Azure kaynaklarının ve bileşenlerinin nasıl hazırlanacağı açıklanmaktadır.

Bu makale, şirket içi sanal makineler için olağanüstü durum kurtarmanın nasıl ayarlanacağını gösteren serideki ilk öğreticidir. 


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure hesabının çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Kurtarma Hizmetleri kasası oluşturun. Kasada VM'lerin meta veri ve yapılandırma bilgileri ile diğer çoğaltma bileşenleri tutulur.
> * Azure sanal ağı (VNet) ayarlayın. Azure VM'ler başarısız olduktan sonra oluşturulduğunda, bu ağa katılırlar.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı talimatlar için, Site Kurtarma İçeriği Tablosunun Nasıl Yapılacağı bölümündeki makaleyi inceleyin.

## <a name="before-you-start"></a>Başlamadan önce

- [VMware,](vmware-azure-architecture.md) [Hyper-V](hyper-v-azure-architecture.md)ve fiziksel [sunucu](physical-azure-architecture.md) olağanüstü durum kurtarma mimarisini gözden geçirin.
- [VMware](vmware-azure-common-questions.md) ve [Hyper-V](hyper-v-azure-common-questions.md) için sık sorulan soruları okuyun

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun. Ardından [Azure portalında](https://portal.azure.com)oturum açın.


## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz Azure hesabınızı yeni oluşturduysanız, aboneliğinizin yöneticisisiniz ve ihtiyacınız olan izinlere sahipsiniz. Abonelik yöneticisi değilseniz, ihtiyaç duyduğunuz izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Azure depolama hesabına yazın.
- Azure yönetilen bir diske yazın.

Bu görevleri tamamlamak için hesabınıza Sanal Makine Katkıda Bulunan yerleşik rolünün atanması gerekir. Buna ek olarak, bir kasadaki Site Kurtarma işlemlerini yönetmek için hesabınıza Site Kurtarma Katılımcısı yerleşik rolü atanmalıdır.


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Azure portalı menüsünden **kaynak oluştur'u**seçin ve **Kurtarma**Pazarı'nda arama yapın.
2. Arama sonuçlarından **Yedekleme ve Site Kurtarma'yı** seçin ve Yedekleme ve Site Kurtarma sayfasında **Oluştur'u**tıklatın. 
3. Kurtarma **Hizmetleri Oluştur kasası** sayfasında **Abonelik'i**seçin. **Contoso Aboneliği**kullanıyoruz.
4. **Kaynak grubunda,** varolan bir kaynak grubu seçin veya yeni bir tane oluşturun. Bu öğretici için **contosoRG**kullanıyoruz.
5. **Vault adına,** tonoztanımlamak için dostça bir ad girin. Bu öğretici dizisi için **ContosoVMVault**’u kullanacağız.
6. **Bölge'de,** kasanın bulunması gereken bölgeyi seçin. **Batı Avrupa** kullanacağız.
7. **İncele ve oluştur**’u seçin.

   ![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

   Yeni kasa artık **Pano** > **Tüm kaynaklarda**ve ana **Kurtarma Hizmetleri kasaları** sayfasında listelenecektir.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Şirket içi makineler Azure yönetilen disklere çoğaltılır. Başarısız olduğunda, Azure VM'leri bu yönetilen disklerden oluşturulur ve bu yordamda belirttiğiniz Azure ağına katılır.

1. [Azure portalında](https://portal.azure.com)**Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin.
2. **Kaynak Yöneticisi'ni** dağıtım modeli olarak seçili tutun.
3. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğreticide **ContosoASRnet** kullanıyoruz.
4. **Adres alanında,** CDR gösteriminde sanal ağın adres aralığını girin. **10.1.0.0/24**kullanıyoruz.
5. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
6. Ağın oluşturulacağı **Kaynak grubunu** belirtin. Biz mevcut **contosoRG** kaynak grubunu kullanıyoruz.
7. **Konum'da,** Kurtarma Hizmetleri kasasının oluşturulduğu bölgeyle aynı bölgeyi seçin. Bizim öğretici olarak **Batı Avrupa'** s . Ağ kasayla aynı bölgede olmalıdır.
8. **Adres aralığında,** ağ için aralığı girin. **10.1.0.0/24**kullanıyoruz ve alt ağ kullanmıyoruz.
9. Temel DDoS korumasının varsayılan seçeneklerini, hizmet bitiş noktası veya ağda güvenlik duvarı olmadan bırakıyoruz.
9. **Oluştur'u**seçin.

   ![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra Azure portal panosunda görürsünüz.




## <a name="next-steps"></a>Sonraki adımlar

- VMware olağanüstü durum kurtarma için [şirket içi VMware altyapısını hazırlayın.](tutorial-prepare-on-premises-vmware.md)
- Hyper-V olağanüstü durum kurtarma [için, şirket içi Hyper-V sunucularını hazırlayın.](hyper-v-prepare-on-premises-tutorial.md)
- Fiziksel sunucu olağanüstü durum kurtarma için [yapılandırma sunucusu ve kaynak ortamını ayarlayın](physical-azure-disaster-recovery.md)
- Azure ağları [hakkında bilgi edinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).
- Yönetilen diskler [hakkında bilgi edinin.](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)
