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
ms.openlocfilehash: 4882206692c334d6ab6af28feb5d2cba5277eea1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78303945"
---
# <a name="move-azure-vms-to-another-region"></a>Azure VM’lerini başka bir bölgeye taşıma

Varolan Azure IaaS sanal makinelerinizi (VM' ler) bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, mevcut VM'lerinizin güvenilirliğini ve kullanılabilirliğini artırmak, yönetilebilirliği artırmak veya yönetim nedenleriyle hareket etmek istiyorsunuz. Daha fazla bilgi için [Azure VM'ye genel bakış](azure-to-azure-move-overview.md)bakın. 

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) için şirket içi makinelerin ve Azure VM'lerinin olağanüstü durum kurtarmasını yönetmek ve düzenlemek için [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanabilirsiniz. Azure VM'lerinin ikincil bir bölgeye taşınmasını yönetmek için Site Kurtarma'yı da kullanabilirsiniz.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> 
> * Taşıma için ön koşulları doğrulayın
> * Kaynak VM'leri ve hedef bölgeyi hazırlama
> * Verileri kopyalama ve çoğaltmayı etkinleştirme
> * Yapılandırmayı test edin ve taşımayı gerçekleştirin
> * Kaynak bölgedeki kaynakları silme
> 
> [!NOTE]
> Bu öğretici, Azure VM'leri olduğu gibi bir bölgeden diğerine nasıl taşıyabileceğinizi gösterir. Farklı bir bölgede sabitlenmiş VM'lere ayarlanmış bir kullanılabilirlik kümesinde VM'leri taşıyarak kullanılabilirliği artırmanız gerekiyorsa, [Azure VM'lerini Kullanılabilirlik Bölgeleri öğreticisine taşıyın'](move-azure-vms-avset-azone.md)a bakın.

## <a name="prerequisites"></a>Ön koşullar

- Azure VM'lerinin taşımak istediğiniz Azure bölgesinde olduğundan emin olun.
- Kaynak bölge seçiminizi [- hedef bölge birleşiminin desteklediğini](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)doğrulayın ve hedef bölge hakkında bilinçli bir karar verin.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini doğrulayın. Ücretsiz Azure hesabınızı oluşturduysanız, aboneliğinizin yöneticisisizsiniz. Abonelik yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Bir VM için çoğaltmayı etkinleştirmek ve azure site kurtarmayı kullanarak verileri kopyalamak için şunları yapmış olmalısınız:

    - Azure kaynaklarında sanal makine oluşturma izinleri. Sanal Makine Katılımcısı yerleşik rolü, aşağıdakileri içeren şu izinlere sahiptir:
    - Seçilen kaynak grubunda sanal makine oluşturma izni
    - Seçilen sanal ağda sanal makine oluşturma izni
    - Seçilen depolama hesabına yazma izni
    
    - Azure Site Kurtarma işlemlerini yönetme izinleri. Site Kurtarma Katılımcısı rolü, Kurtarma Hizmetleri kasasında Site Kurtarma işlemlerini yönetmek için gereken tüm izinlere sahiptir.

- Taşımak istediğiniz Azure'daki tüm kök sertifikaların azure'da olduğundan emin olun. En son kök sertifikalar VM'de yoksa, güvenlik kısıtlamaları veri kopyasını hedef bölgeye engeller.

- Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilmiş bir ortamda, kuruluşunuz için standart Windows Update ve sertifika güncelleştirme işlemlerini uygulayın.
    
- Linux VM'leri için, VM'deki en son güvenilir kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtımcınız tarafından sağlanan kılavuzu izleyin.
- Taşımak istediğiniz VM'ler için ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanmadığınızdan emin olun.

- Taşımaya çalıştığınız VM'nin internet erişimi yoksa veya giden erişimi denetlemek için bir güvenlik duvarı proxy'si [kullanıyorsa, gereksinimleri denetleyin.](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms)

- Kaynak ağ düzenini ve şu anda kullanmakta olduğunuz tüm kaynakları tanımlayın. Bu, yük dengeleyicileri, ağ güvenlik gruplarını (NSG'ler) ve genel IP'leri içerir, ancak bunlarla sınırlı değildir.

- Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmanıza izin verdiğini doğrulayın. Gerekli kotayı sağlamak için desteğe başvurun.

- Aboneliğinizin kaynak VM'lerinizle eşleşen boyutlardaki VM'leri desteklemek için yeterli kaynağa sahip olduğundan emin olun. Verileri hedefe kopyalamak için Site Kurtarma'yı kullanıyorsanız, Site Kurtarma hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

- Kaynak ağ düzeninde tanımlanan her bileşen için bir hedef kaynak oluşturduğunuzdan emin olun. Bu adım, VM'lerinizin kaynak bölgede bulunan hedef bölgedeki tüm işlevselliğe ve özelliklere sahip olduğundan emin olmak için önemlidir.

     > [!NOTE] 
     > Azure Site Kurtarma, kaynak VM için çoğaltmayı etkinleştirdiğinizde otomatik olarak sanal bir ağ keşfeder ve oluşturur. Ayrıca, etkinleştirme çoğaltma için bir ağı önceden oluşturabilir ve kullanıcı akışındaki VM'ye atayabilirsiniz. Daha sonra belirtildiği gibi, hedef bölgede diğer kaynakları el ile oluşturmanız gerekir.

    Kaynak VM yapılandırmasına dayalı olarak sizin için alakalı en yaygın kullanılan ağ kaynaklarını oluşturmak için aşağıdaki belgelere bakın:
    - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer)
    -  [Genel IP](../virtual-network/virtual-network-public-ip-address.md)
    - Diğer ağ bileşenleri için [ağ belgelerine](https://docs.microsoft.com/azure/?pivot=products&panel=network)bakın.



## <a name="prepare"></a>Hazırlama
Aşağıdaki adımlar, sanal makinenin çözüm olarak Azure Site Kurtarma'yı kullanarak taşınmaya nasıl hazırlanacağını gösterir. 

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Kaynak bölge dışında herhangi bir bölgede kasa oluşturun

1. [Azure portalı](https://portal.azure.com) > **Kurtarma Hizmetleri'nde**oturum açın.
1. Kaynak > **Yönetim Araçları** > **Yedekleme ve Site Kurtarma** **Oluştur'u**seçin.
1. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Birden fazla aboneliğiniz varsa uygun olanı seçin.
1. **ContosoRG**kaynak grubunu oluşturun.
1. Bir Azure bölgesi belirtin. Desteklenen bölgeleri denetlemek için Azure [Site Kurtarma fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/site-recovery/)coğrafi kullanılabilirlik bölümüne bakın.
1. **Kurtarma Hizmetleri kasalarında,** **Genel Bakış** > **ContosoVMVault** > **+Replicate'yi**seçin.
1. **Kaynak** bölümünde **Azure** seçeneğini belirleyin.
1. **Kaynak konumu**’nda, VM’lerinizin çalışmakta olduğu kaynak Azure bölgesini seçin.
1. Kaynak Yöneticisi dağıtım modelini seçin. Ardından **Kaynak aboneliği** ni ve Kaynak **kaynak grubunu**seçin.
1. Ayarları kaydetmek için **Tamam'ı** seçin.

### <a name="enable-replication-for-azure-vms-and-start-copying-the-data"></a>Azure Sanal M'ler için çoğaltmayı etkinleştirin ve verileri kopyalamaya başlayın

Site Kurtarma, abonelik ve kaynak grubuyla ilişkili VM'lerin listesini alır.

1. Bir sonraki adımda, taşımak istediğiniz VM'yi seçin ve **ardından Tamam'ı**seçin.
1. **Ayarlar'da** **Olağanüstü Durum kurtarma'yı**seçin.
1. Olağanüstü durum kurtarma > Hedef**bölgesini** **yapılandırın,** çoğaltacağınız hedef bölgeyi seçin.
1. Bu öğretici için diğer varsayılan ayarları kabul edin.
1. **Çoğaltmayı etkinleştir** seçeneğini belirleyin. Bu adım, VM için çoğaltmayı etkinleştirmek için bir iş başlatır.

    ![Çoğaltmayı etkinleştirme](media/tutorial-migrate-azure-to-azure/settings.png)

## <a name="move"></a>Taşı

Aşağıdaki adımlar, hedef bölgeye taşımanın nasıl gerçekleştirilgösterdiğini gösterir.

1. Kasaya git. Yinelenen**Ayarlar'da** **Settings** > VM'yi seçin ve ardından **Failover'ı**seçin.
2. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin.
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. İş bittikten sonra, VM'nin beklendiği gibi hedef Azure bölgesinde görünün.


## <a name="discard"></a>Vazgeç 

Taşınan VM'yi işaretlediyseniz ve başarısız lık noktasına kadar değiştirmeniz gerekiyorsa veya **çoğaltılan öğelerde**önceki bir noktaya geri dönmek istiyorsanız, VM > **Change kurtarma noktasını**sağ seçin. Bu adım, farklı bir kurtarma noktası ve bu bir başarısızlık belirtme seçeneği sağlar. 


## <a name="commit"></a>İşleme 

Taşınan VM'yi kontrol ettiğinizde ve **yinelenen öğelerde**değişikliği gerçekleştirmeye hazır olduktan sonra VM > **Commit'i**sağ seçin. Bu adım, hedef bölgeye taşıma işlemini tamamlar. Taahhüt işi bitene kadar bekleyin.

## <a name="clean-up"></a>Temizleme

Aşağıdaki adımlar, kaynak bölgenin yanı sıra taşıma için kullanılan ilgili kaynakların nasıl temizlenir konusunda size yol gösterecektir.

Taşıma için kullanılan tüm kaynaklar için:

- VM'ye git. **Çoğaltmayı Devre Dışı'** yı seçin. Bu adım, VM için veri kopyalama işlemini durdurur.

   > [!IMPORTANT]
   > Azure Site Kurtarma çoğaltma için ücretlendirilmekten kaçınmak için bu adımı gerçekleştirmek önemlidir.

Kaynak kaynaklardan herhangi birini yeniden kullanmayı planlıyorsanız, aşağıdaki ek adımları tamamlayın:

1. [Önkoşullarda](#prerequisites)tanımladığınız kaynak bölgedeki ilgili tüm ağ kaynaklarını silin.
1. Kaynak bölgedeki ilgili depolama hesabını silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure VM'yi farklı bir Azure bölgesine taşıdınız. Artık taşıdığınız VM için olağanüstü durum kurtarma yı yapılandırabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)

