---
title: Azure Site Kurtarma ile Azure'u Hyper-V olağanüstü durum kurtarma için hazırlayın
description: Azure Site Kurtarma'yı kullanarak azure'u şirket içi Hyper-V V'lerin olağanüstü kurtarma sına nasıl hazırlayacağınızı öğrenin
author: rayne-wiselman
ms.service: site-recovery
services: site-recovery
ms.topic: tutorial
ms.date: 11/14/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ef623b95e104b485c6bfc8b2f489afeca436d81e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74084185"
---
# <a name="prepare-azure-resources-for-hyper-v-disaster-recovery"></a>Azure kaynaklarını Hyper-V olağanüstü durum kurtarma için hazırlama

 [Azure Site Kurtarma,](site-recovery-overview.md) iş uygulamalarını planlı ve planlanmamış kesintiler sırasında çalışır durumda tutarak iş sürekliliğine ve olağanüstü durum kurtarma (BCDR) uygulamasına yardımcı olur. Site Recovery, şirket içi makinelerin ve Azure sanal makinelerinin çoğaltma, yük devretme ve kurtarma gibi olağanüstü durum kurtarma işlemlerini yönetir ve düzenler.

Bu öğretici, şirket içi Hyper-V VM'ler için olağanüstü durum kurtarmanın nasıl ayarlandığını açıklayan bir serinin ilkidir.

> [!NOTE]
> Bir senaryo için en basit dağıtım yolunu göstermek için öğreticiler tasarlarız. Bu öğreticiler mümkün olduğunda varsayılan seçenekleri kullanır ve olası tüm ayarları ve yolları göstermez. Daha fazla bilgi için, ilgili her senaryo için "Nasıl Yapılsın" bölümüne bakın.

Bu öğretici, şirket içi VM'leri (Hyper-V) Azure'a çoğaltmak istediğinizde Azure bileşenlerini nasıl hazırlayacağınızı gösterir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure hesabınızın çoğaltma izinlerine sahip olduğunu doğrulayın.
> * Çoğaltılan makinelerin görüntülerini depolayan bir Azure depolama hesabı oluşturun.
> * VM'ler ve diğer çoğaltma bileşenleri için meta veri ve yapılandırma bilgilerini depolayan bir Kurtarma Hizmetleri kasası oluşturun.
> * Bir Azure ağı ayarlayın. Azure VM'ler başarısız olduktan sonra oluşturulduğunda, bu ağa katılırlar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/pricing/free-trial/) bir hesap oluşturun.

## <a name="sign-in"></a>Oturum aç

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="verify-account-permissions"></a>Hesap izinlerini doğrulama

Ücretsiz bir Azure hesabı oluşturduysanız, bu aboneliğin yöneticisi sizsiniz. Yönetici değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Yeni bir sanal makineye yönelik çoğaltmayı etkinleştirmek için şunları yapma iznine sahip olmalısınız:

- Seçilen kaynak grubunda sanal makine oluşturma.
- Seçilen sanal ağda sanal makine oluşturma.
- Seçilen depolama hesabına yazma.

Bu görevleri tamamlamak için hesabınıza Sanal Makine Katılımcısı yerleşik rolü atanmalıdır. Bir kasadaki Site Kurtarma işlemlerini yönetmek için, hesabınıza Site Kurtarma Katılımcısı yerleşik rolü atanmalıdır.

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Çoğaltılan makinelerin görüntüleri Azure depolama alanında tutulur. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.

1. Azure [portalı](https://portal.azure.com) menüsünde, **kaynak** > **Depolama Deposu** > hesabı oluştur **'u seçin - blob, dosya, tablo, sıra.**
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin.  Seçtiğiniz ad Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca küçük harfler ve sayılar kullanmalıdır. Bu öğretici için **contosovmsacct1910171607**kullanın.
3. **Dağıtım modeli** bölümünde **Kaynak Yöneticisi**’ni seçin.
4. **Hesap türünde,** **Depolama 'yı (genel amaçlı v1)** seçin. Blob depolamayı seçmeyin.
5. **Çoğaltma** bölümünde depolama yedeklemesi için **Okuma Erişimli Coğrafi Olarak Yedekli depolama**’yı seçin. Güvenli aktarım gerekli ayarı Devre Dışı olarak bırakın.
6. **Performans** bölümünde **Standart**’ı seçin. Ardından, **Access katmanında,** **Hot**varsayılan seçeneğini seçin.
7. **Abonelik'te,** yeni depolama hesabı oluşturmak istediğiniz aboneliği seçin.
8. **Kaynak grubu** bölümünde yeni bir kaynak grubu adı girin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir kapsayıcıdır. Bu öğretici için **ContosoRG'u**kullanın.
9. **Konum'da,** depolama hesabınız için coğrafi konumu seçin. Bu öğretici için **Batı Avrupa'yı**kullanın.
10. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

   ![Depolama hesabı oluşturma](media/tutorial-prepare-azure/create-storageacct.png)

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

1. Azure portalında **+Kaynak Oluştur'u**seçin ve ardından Kurtarma Hizmetleri için Azure Marketi'nde arama yapın.
2. **Yedekleme ve Site Kurtarma (OMS) seçeneğini belirleyin.** Ardından, **Yedekleme ve Site Kurtarma** sayfasında **Oluştur'u**seçin.
1. **Kurtarma hizmetleri tonoz > Adı,** tonoz tanımlamak için bir dostu ad girin. Bu öğretici için **ContosoVMVault** adını kullanın.
2. **Kaynak grubunda,** varolan bir kaynak grubu seçin veya yeni bir tane oluşturun. Bu öğretici için **contosoRG**kullanın.
3. **Konum'da,** kasanın bulunması gereken bölgeyi seçin. Bu öğretici için **Batı Avrupa'yı**kullanın.
4. Panodan kasaya hızlı bir şekilde erişmek **için, Oluştur'a Pin'i** > **Create**seçin.

![Yeni kasa oluştur](./media/tutorial-prepare-azure/new-vault-settings.png)

Yeni kasa **Pano** > **Tüm kaynaklarda**ve ana **Kurtarma Hizmetleri kasaları** sayfasında görünür.

## <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Yük devretmeden sonra depolamadan Azure sanal makineleri oluşturulduğunda sanal makineler bu ağa katılır.

1. [Azure portalında](https://portal.azure.com)**Kaynak oluştur** > **Ağ** > **Sanal ağ** seçeneklerini belirleyin. Dağıtım modeli olarak Resource Manager’ı seçili bırakın.
2. **Ad** bölümünde bir ağ adı girin. Ad, Azure kaynak grubu içinde benzersiz olmalıdır. Bu öğretici için **ContosoASRnet**kullanın.
3. Ağı oluşturacak kaynak grubunu belirtin. Bu öğretici için, varolan kaynak grubu **contosoRG**kullanın.
4. **Adres aralığında,** ağ aralığı olarak **10.0.0.0/24** girin. Bu ağ için alt ağ yok.
5. **Abonelik** bölümünde ağın oluşturulacağı aboneliği seçin.
6. **Konum'da,** Batı **Avrupa'yı**seçin. Ağın, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.
7. Ağda hizmet bitiş noktası olmadan temel DDoS korumasının varsayılan seçeneklerini bırakın.
8. **Oluştur'u**seçin.

![Sanal ağ oluşturma](media/tutorial-prepare-azure/create-network.png)

Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra Azure portal panosunda görürsünüz.

## <a name="useful-links"></a>Yararlı bağlantılar

Şunları öğrenin:
- [Azure ağları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
- [Yönetilen diskler](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview)



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Şirket içi Hyper-V altyapısını Azure'a olağanüstü durum kurtarma için hazırlayın](hyper-v-prepare-on-premises-tutorial.md)
