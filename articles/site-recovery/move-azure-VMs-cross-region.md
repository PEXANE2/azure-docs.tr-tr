---
title: Azure Site Kurtarma ile Azure VM'lerini başka bir bölgeye taşıyın
description: Azure IaaS VM'lerini bir Azure bölgesinden diğerine taşımak için Azure Site Kurtarma'yı kullanın.
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: dc37cb6fa05a2be56de7bf5536d7274190257d85
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78303928"
---
# <a name="move-azure-vms-to-another-azure-region"></a>Azure VM'leri başka bir Azure bölgesine taşıma

Güvenilirliği, kullanılabilirliği, yönetimi veya yönetimi geliştirmek için Azure altyapısını hizmet (IaaS) sanal makineleri bir bölgeden diğerine taşımak isteyebilirsiniz. Bu öğretici, Azure Site Kurtarma'yı kullanarak VM'leri başka bir bölgeye nasıl taşıyabileceğinizi gösterir. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları doğrulama
> * Kaynak VM'leri hazırlama
> * Hedef bölgeyi hazırlama
> * Verileri hedef bölgeye kopyalama
> * Yapılandırmayı test edin
> * Hareketi gerçekleştirin
> * Kaynakları kaynak bölgeden atma


> [!IMPORTANT]
> Bu makalede, Azure VM'leri olduğu *gibi*bir bölgeden diğerine nasıl taşınır' açıklanmaktadır. Hedefiniz, VM'leri kullanılabilirlik bölgelerine taşıyarak altyapınızın kullanılabilirliğini artırmaksa, azure [vm'lerini Kullanılabilirlik Bölgelerine Taşı](move-azure-vms-avset-azone.md)bölümüne bakın.

## <a name="prerequisites"></a>Ön koşullar

- Geçmek istediğiniz kaynak Azure bölgesinde Azure VM'lerin olduğundan emin *olun.*
- Kaynak bölge hedef bölge birleşimi seçiminizi [destekleyip](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)hedef bölgeyi dikkatle seçtiğinizi doğrulayın.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın. Ücretsiz Azure hesabınızı yeni oluşturduysanız, aboneliğinizin *yöneticisisiniz.* Yönetici değilseniz, gereksinim duyduğunuz izinleri almak için yöneticiyle birlikte çalışın:
  -  Bir VM için çoğaltmayı etkinleştirmek ve Site Kurtarma'yı kullanarak verileri hedefe kopyalamak için Azure kaynaklarınızda bir VM oluşturma iznine sahip olmalısınız. Sanal Makine Katılımcısı yerleşik rolü bu izinlere sahiptir. İzinlerle şunları yapabilirsiniz:
        - Seçilen kaynak grubunda sanal makine oluşturma.
        - Seçilen sanal ağda sanal makine oluşturma.
        - Seçilen depolama hesabına yazma.

  - Ayrıca, Site Kurtarma işlemlerini yönetmek için izinlere de ihtiyacınız vardır. Site Kurtarma Katılımcısı rolü, Bir Azure Kurtarma Hizmetleri kasasında Site Kurtarma işlemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak VM'leri hazırlama

1. Taşımayı planladığınız Azure VM'lerinin en son kök sertifikalarına sahip olup olmadığını kontrol edin. Yoksa, güvenlik kısıtlamaları nedeniyle hedef bölgeye veri kopyasını etkinleştiremezsiniz.

    - Windows VM'ler için, tüm güvenilen kök sertifikaların makinede olması için en son Windows güncelleştirmelerini yükleyin. Bağlantısı kesilen bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini izleyin.
    - Linux VM'leri için, en son güvenilir kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtımınızın rehberliğini izleyin.
2. Taşımayı planladığınız VM'ler için ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanmadığınızdan emin olun.
3. Taşımak istediğiniz bir VM'nin internet erişimi yoksa ve giden erişimi denetlemek için bir güvenlik duvarı proxy'si kullanıyorsa, [gereksinimleri](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms)denetleyin.
4. Kaynak ağ düzenini ve yük dengeleyicileri, ağ güvenlik grupları ve doğrulama için genel IP adresleri dahil (ancak bunlarla sınırlı olmamak üzere) kullanmakta olduğunuz tüm kaynakları belgeleyin.

## <a name="prepare-the-target-region"></a>Hedef bölgeyi hazırlama

1. Azure aboneliğinizde, olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturabileceğinizi doğrulayın. Gerekirse gerekli kotayı etkinleştirmek için Desteğe başvurun.

2. Aboneliğinizin kaynak VM'lerinizi desteklemek için yeterli kaynağa sahip olduğundan emin olun. Verileri hedefe kopyalamak için Site Kurtarma'yı kullanıyorsanız, hedef Sanal Taşıtlar için aynı boyutu veya kullanılabilirdeki en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımladığınız her bileşen için bir hedef kaynak oluşturduğunuzdan emin olun. Bu, VM'lerinizin hedef bölgedeki tüm işlevselliğe ve özelliklere sahip olmasını sağlar.

   Azure Site Kurtarma, kaynak VM için çoğaltmayı etkinleştirdiğinizde otomatik olarak sanal bir ağ ve depolama hesabı keşfeder ve oluşturur. Ayrıca, bu kaynakları önceden oluşturabilir ve etkinleştirme çoğaltma adımının bir parçası olarak VM'ye atayabilirsiniz. Ancak, hedef bölgede başka kaynaklar oluşturmanız gerekir. Kaynak VM yapılandırmanızı temel alan en yaygın kullanılan ağ kaynaklarını oluşturmak için aşağıdaki belgelere bakın:

   - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
   - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer)
   - [Genel IP](../virtual-network/virtual-network-public-ip-address.md)
    
   Diğer ağ bileşenleri için [Azure ağ belgelerine](https://docs.microsoft.com/azure/?pivot=products&panel=network)bakın. 

4. Taşımayı gerçekleştirmeden önce yapılandırmayı sınamak için, hedef bölgede el ile [üretim dışı bir ağ oluşturun.](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) Kurulumun test edilmesi, üretim ortamıyla en az etkileşim oluşturur ve bunu öneririz.
    
## <a name="copy-data-to-the-target-region"></a>Verileri hedef bölgeye kopyalama
Aşağıdaki adımlar, verileri hedef bölgeye kopyalamak için Azure Site Kurtarma'yı kullanır.

### <a name="create-the-vault-in-any-region-except-the-source"></a>Kaynak dışında herhangi bir bölgede kasa oluşturun

1. [Azure portalı](https://portal.azure.com) > **Kurtarma Hizmetleri'nde**oturum açın.
2. Kaynak > **Yönetim Araçları** > **Yedekleme ve Site Kurtarma** **Oluştur'u**seçin.
3. **Adı**için, dostu adı **ContosoVMVault**belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri denetlemek için [Azure Sitesi Kurtarma Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/site-recovery/)bakın.
6. Kurtarma Hizmetleri kasaları için **Genel Bakış** > **ConsotoVMVault** > **+Replicate**'yi seçin.
7. **Kaynak**için **Azure'u**seçin.
8. **Kaynak konumu**için, VM'lerinizin şu anda çalıştığı kaynak Azure bölgesini seçin.
9. Azure Kaynak Yöneticisi dağıtım modelini seçin. Ardından, **Kaynak aboneliği** ni ve Kaynak **kaynak grubunu**seçin.
10. Ayarları kaydetmek için **Tamam'ı** seçin.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Azure Sanal M'ler için çoğaltmayı etkinleştirin ve verileri kopyalamaya başlayın

Site Kurtarma, abonelik ve kaynak grubuyla ilişkili VM'lerin listesini alır.

1. Taşımak istediğiniz VM'yi seçin ve ardından **Tamam'ı**seçin.
2. **Ayarlar**için **Olağanüstü Durum kurtarma'yı**seçin.
3. Olağanüstü durum kurtarma > Hedef**bölgesini** **yapılandırmak**için, çoğaldığınız hedef bölgeyi seçin.
4. Varsayılan hedef kaynakları veya önceden oluşturduğunuz kaynakları kullanmayı seçin.
5. İşi başlatmak için **çoğaltmayı etkinleştir'i** seçin.

   ![Çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

 

## <a name="test-the-configuration"></a>Yapılandırmayı test edin


1. Kasaya git. **Yinelenen Ayarlar** > **öğelerinde,** hedef bölgeye taşımak istediğiniz sanal makineyi seçin. Ardından, **Test Failover'ı**seçin.
2. **Test Failover'da,** başarısız olmak için kullanılacak bir kurtarma noktası seçin:

   - **En son işlenen**: VM’nin yükünü, Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Veri işleme için zaman harcanması olmadığından, bu seçenek düşük kurtarma süresi hedefi (RTO) sağlar.
   - **En son uygulama tutarlılığı**: Tüm VM'lerde en son uygulama tutarlı kurtarma noktasına kadar başarısız olur. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktası seçin.

3. Yapılandırmayı sınamak için Azure VM'lerini taşımak istediğiniz hedef Azure sanal ağını seçin.

   > [!IMPORTANT]
   > Hedef bölgedeki üretim ağı için değil, test başarısız olması için ayrı bir Azure VM ağı kullanmanızı öneririz.

4. Hareketi test etmeye başlamak için **Tamam'ı**seçin. İlerlemeyi izlemek **için, Özelliklerini** açmak için VM'yi seçin. Veya kasadaki **Test Failover** işini seçin. Ardından, **Ayarlar** > **İşler** > **Sitesi Kurtarma işlerini**seçin.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Test etmek için oluşturduğunuz VM'yi silmek için, çoğaltılan öğeüzerinde **Temizleme testi başarısız olmasını** seçin. **Notlar,** kayıt ve test ile ilgili herhangi bir gözlem kaydedin.

## <a name="perform-the-move-and-confirm"></a>Taşımayı gerçekleştirin ve onaylayın

1. **Ayarlar** > **Çoğaltılan öğelerde**kasaya gidin, sanal makineyi seçin ve ardından **Failover'ı**seçin.
1. **Failover**için **En Son'u**seçin. 
2. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Kurtarma, başarısızolmayı tetiklemeden önce kaynak VM'yi kapatmaya çalışır. Ancak kapatma başarısız olsa bile başarısız olur. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
3. İş bittiğinde, VM'nin beklendiği gibi hedef Azure bölgesinde görünün.
4. **Çoğaltılan öğelerde**VM'ye sağ tıklayın ve **Commit'i**seçin. Bu hareketi bitirir. Taahhüt işi bitene kadar bekleyin.

## <a name="discard-the-resources-from-the-source-region"></a>Kaynakları kaynak bölgeden atma

- VM'ye gidin ve **Çoğaltmayı Devre Dışı**Bırak'ı seçin. Bu, VM için veri kopyalama işlemini durdurur.

  > [!IMPORTANT]
  > Taşımadan sonra Site Kurtarma çoğaltma için ücretlendirilmemek için bu adımı tamamlayın.

Kaynak kaynaklardan herhangi birini yeniden kullanmayı düşünmüyorsanız, aşağıdaki adımları izleyin:

1. [Kaynak VM'leri Hazırla'nın](#prepare-the-source-vms)4.
2. Kaynak bölgedeki ilgili depolama hesabını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure VM'leri farklı bir Azure bölgesine taşımayı öğrendiniz. Artık bu VM'ler için olağanüstü durum kurtarma yı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

