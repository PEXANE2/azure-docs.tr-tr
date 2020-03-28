---
title: Azure Site Kurtarma'yı kullanarak kullanılabilirlik bölgeleri olan bir Azure bölgesine taşıma
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: 5728ce7125695b191de4f91d5bd9003384f428a7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78298317"
---
# <a name="move-azure-vms-into-availability-zones"></a>Azure VM'lerini Kullanılabilirlik Alanlarına taşıma
Azure'daki Kullanılabilirlik Bölgeleri, uygulamalarınızın ve verilerinizin veri merkezi hatalarından korunmasına yardımcı olur. Her Kullanılabilirlik Alanı bağımsız enerji, soğutma ve ağ kaynaklarıyla donatılmış bir veya daha fazla veri merkezinden oluşur. Esnekliği sağlamak için, tüm etkin bölgelerde en az üç ayrı bölge vardır. Kullanılabilirlik Bölgelerinin bir bölge içindeki fiziksel olarak ayrılması, uygulamaların ve verilerin veri merkezi hatalarından korunmasına yardımcı olur. Kullanılabilirlik Bölgeleri ile Azure, sanal makinelerin (VM) çalışma süresi için %99,99'luk hizmet düzeyi sözleşmesi (SLA) sunar. Kullanılabilirlik Bölgeleri, [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?](https://docs.microsoft.com/azure/availability-zones/az-overview#services-support-by-region)

VM'lerinizin belirli bir bölgeye *tek örnek* olarak dağıtıldığı ve bu VM'leri kullanılabilirlik bölgesine taşıyarak kullanılabilirliğinizi artırmak istediğiniz bir senaryoda, bunu Azure Site Kurtarma'yı kullanarak yapabilirsiniz. Bu eylem daha da kategorize edilebilir:

- Tek örnekli VM'leri hedef bölgedeki Kullanılabilirlik Bölgelerine taşıma
- Kullanılabilirlik kümesindeki VM'leri hedef bölgedeki Kullanılabilirlik Bölgelerine taşıma

> [!IMPORTANT]
> Şu anda Azure Site Kurtarma, VM'lerin bir bölgeden diğerine taşınmasını destekler, ancak bir bölge içinde taşınmayı desteklemez.

## <a name="check-prerequisites"></a>Önkoşulları denetleme

- Hedef bölgenin Kullanılabilirlik Bölgeleri için desteği olup olmadığını kontrol [edin.](https://docs.microsoft.com/azure/availability-zones/az-overview#services-support-by-region) [Kaynak bölge/hedef bölge birleşimi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)seçiminizi destekleninden kontrol edin. Hedef bölge hakkında bilinçli bir karar ver.
- [Senaryo mimarisini ve bileşenlerini ](azure-to-azure-architecture.md) anladığınızdan emin olun.
- [Destek sınırlamaları ve gereksinimleri](azure-to-azure-support-matrix.md) konusunu inceleyin.
- Hesap izinlerini kontrol edin. Ücretsiz Azure hesabınızı yeni oluşturduysanız, aboneliğinizin yöneticisisiniz. Abonelik yöneticisi değilseniz, ihtiyacınız olan izinleri atamak için yöneticiyle birlikte çalışın. Bir VM için çoğaltmayı etkinleştirmek ve sonunda Azure Site Kurtarma'yı kullanarak verileri hedefe kopyalamak için şunları yapmış olmalısınız:

    1. Azure kaynaklarında VM oluşturma izni. *Sanal Makine Katılımcısı* yerleşik rolü, aşağıdakileri içeren şu izinlere sahiptir:
        - Seçilen kaynak grubunda sanal makine oluşturma izni
        - Seçilen sanal ağda sanal makine oluşturma izni
        - Seçilen depolama hesabına yazma izni

    2. Azure Site Kurtarma görevlerini yönetme izni. *Site Kurtarma Katılımcısı* rolü, Kurtarma Hizmetleri kasasındaki Site Kurtarma eylemlerini yönetmek için gereken tüm izinlere sahiptir.

## <a name="prepare-the-source-vms"></a>Kaynak VM'leri hazırlama

1. VM'leriniz, Site Kurtarma'yı kullanarak kullanılabilirlik bölgesine taşımak istiyorsanız yönetilen diskleri kullanmalıdır. Yönetilen diskleri kullanmak için yönetilmeyen diskleri kullanan varolan Windows VM'lerini dönüştürebilirsiniz. Windows sanal [makinesini yönetilmeyen disklerden yönetilen disklere dönüştür'teki](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)adımları izleyin. Kullanılabilirlik kümesinin *yönetilen*olarak yapılandırıldığından emin olun.
2. Taşımak istediğiniz Azure VM'lerinde en son kök sertifikaların tümünden olup olmadığını kontrol edin. En son kök sertifikalar yoksa, güvenlik kısıtlamaları nedeniyle hedef bölgeye veri kopyası etkinleştirilemez.

3. Windows VM’ler için, güvenilir kök sertifikaların tamamı makinede mevcut olacak şekilde sanal makineye en son Windows güncelleştirmelerinin tümünü yükleyin. Bağlantısı kesilen bir ortamda, kuruluşunuz için standart Windows güncelleştirme ve sertifika güncelleştirme işlemlerini izleyin.

4. Linux VM'leri için, VM'deki en son güvenilir kök sertifikalarını ve sertifika iptal listesini almak için Linux dağıtımcınız tarafından sağlanan kılavuzu izleyin.
5. Taşımak istediğiniz VM'ler için ağ bağlantısını denetlemek için kimlik doğrulama proxy'si kullanmadığınızdan emin olun.

6. Taşımaya çalıştığınız VM'nin Internet erişimi yoksa ve giden erişimi denetlemek için bir güvenlik duvarı proxy'si kullanıyorsa, [giden ağ bağlantısını Yapılandır'da](azure-to-azure-tutorial-enable-replication.md#set-up-outbound-network-connectivity-for-vms)gereksinimleri denetleyin.

7. Yük dengeleyicileri, NSG'ler ve genel IP'ler de dahil olmak üzere kaynak ağ düzenini ve şu anda doğrulama için kullandığınız kaynakları tanımlayın.

## <a name="prepare-the-target-region"></a>Hedef bölgeyi hazırlama

1. Azure aboneliğinizin olağanüstü durum kurtarma için kullanılan hedef bölgede VM'ler oluşturmanıza izin verdiğini denetleyin. Gerekirse, gerekli kotayı etkinleştirmek için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Verileri hedefe kopyalamak için Site Kurtarma'yı kullanırsanız, hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımlanan her bileşen için bir hedef kaynak oluşturun. Bu eylem, hedef bölgeye geçtikten sonra, VM'lerinizin kaynaktaki tüm işlevselliğe ve özelliklere sahip olmasını sağlar.

    > [!NOTE]
    > Azure Site Kurtarma, kaynak VM için çoğaltmayı etkinleştirdiğinizde otomatik olarak sanal bir ağ ve depolama hesabı keşfeder ve oluşturur. Ayrıca, bu kaynakları önceden oluşturabilir ve etkinleştirme çoğaltma adımının bir parçası olarak VM'ye atayabilirsiniz. Ancak, daha sonra belirtildiği gibi, diğer kaynaklar için, bunları hedef bölgede el ile oluşturmanız gerekir.

     Aşağıdaki belgeler, kaynak VM yapılandırmasına bağlı olarak, sizinle alakalı en sık kullanılan ağ kaynaklarının nasıl oluşturulacak olduğunu söyler.

    - [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer)
    - [Genel IP](../virtual-network/virtual-network-public-ip-address.md)
    
   Diğer ağ bileşenleri için ağ [belgelerine](https://docs.microsoft.com/azure/?pivot=products&panel=network)bakın.

    > [!IMPORTANT]
    > Hedefte bölge yedekli yük dengeleyicisi kullandığınızdan emin olun. [Standart Yük Dengeleyici ve Kullanılabilirlik Bölgeleri'nde](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-availability-zones)daha fazla bilgi edinebilirsiniz.

4. Hedef bölgeye kesmeden önce yapılandırmayı sınamak istiyorsanız, hedef bölgede üretim dışı bir ağ el ile [oluşturun.](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) Üretim ortamına en az müdahaleye neden olduğu için bu yaklaşımı öneririz.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme
Aşağıdaki adımlar, verileri hedef bölgeye çoğaltmayı etkinleştirmek için Azure Site Kurtarma'yı kullanırken, bunları kullanılabilirlik bölgelerine taşımadan önce size yol gösterecektir.

> [!NOTE]
> Bu adımlar tek bir VM içindir. Aynı şeyi birden çok VM'ye uzatabilirsiniz. Kurtarma Hizmetleri kasasına gidin, **+ Çoğaltma'yı**seçin ve ilgili VM'leri birlikte seçin.

1. Azure portalında **Sanal makineleri**seçin ve Kullanılabilirlik Bölgeleri'ne taşımak istediğiniz VM'yi seçin.
2. **İşlemler** menüsünden **Olağanüstü durum kurtarma** seçeneğini belirleyin.
3. Olağanüstü durum kurtarma > Hedef**bölgesini** **yapılandırın,** çoğaltacağınız hedef bölgeyi seçin. Bu bölgenin Kullanılabilirlik Bölgelerini [desteklediğinden](https://docs.microsoft.com/azure/availability-zones/az-overview#services-support-by-region) emin olun.

    ![Hedef bölgenin seçimi](media/azure-vms-to-zones/enable-rep-1.PNG)

4. **Sonraki'ni seçin: Gelişmiş ayarlar.**
5. Hedef abonelik, hedef VM kaynak grubu ve sanal ağ için uygun değerleri seçin.
6. **Kullanılabilirlik** bölümünde, VM'yi taşımak istediğiniz Kullanılabilirlik Bölgesini seçin. 
   > [!NOTE]
   > Kullanılabilirlik kümesi veya Availabilty Zone seçeneğini görmüyorsanız, [ön koşulların](#prepare-the-source-vms) karşılandığından ve kaynak VM'lerin [hazırlanmasının](#prepare-the-source-vms) tamamlandığından emin olun.
  
    ![Kullanılabilirlik Bölgesi seçmek için seçimler](media/azure-vms-to-zones/enable-rep-2.PNG)

7. **Çoğaltmayı Etkinleştir'i**seçin. Bu eylem VM için çoğaltma yı etkinleştirmek için bir iş başlatır.

## <a name="check-settings"></a>Ayarları kontrol edin

Çoğaltma işlemi bittikten sonra, çoğaltma durumunu denetleyebilir, çoğaltma ayarlarını değiştirebilir ve dağıtımı test edebilirsiniz.

1. VM menüsünden **Olağanüstü durum kurtarma** seçeneğini belirleyin.
2. Çoğaltma durumunu, oluşturulan kurtarma noktalarını ve kaynak ve hedef bölgeleri haritaüzerinde denetleyebilirsiniz.

   ![Çoğaltma durumu](media/azure-to-azure-quickstart/replication-status.png)

## <a name="test-the-configuration"></a>Yapılandırmayı test edin

1. Sanal makine menüsünde **Olağanüstü Durum kurtarma'yı**seçin.
2. Test **Failover** simgesini seçin.
3. **Test Failover'da,** başarısız olmak için kullanılacak bir kurtarma noktası seçin:

   - **En son işlenen**: VM’nin yükünü, Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük kurtarma süresi hedefi (RTO) elde edilir.
   - **Uygulamayla tutarlı olan son**: Bu seçenek, tüm VM’lerin yükünü uygulamayla tutarlı olan en son kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktası seçin.

3. Yapılandırmayı sınamak için Azure VM'lerini taşımak istediğiniz test hedefi Azure sanal ağını seçin. 

    > [!IMPORTANT]
    > Test hatası için ayrı bir Azure VM ağı kullanmanızı öneririz, VM'lerinizi taşımak istediğiniz hedef bölgedeki üretim ağını kullanmamanızı öneririz.

4. Hareketi test etmeye başlamak için **Tamam'ı**seçin. İlerlemeyi izlemek için özelliklerini açmak için VM'yi seçin. Veya, **Ayarlar** > **İşler** > **Sitesi Kurtarma işleri**> kasa adında Test **Failover** iş seçebilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Hareketi test etme nin bir parçası olarak oluşturulan VM'yi silmek istiyorsanız, çoğaltılan öğeüzerinde **Temizleme testi başarısızlığı'nı** seçin. **Notlar,testle**ilişkili gözlemleri kaydedin ve kaydedin.

## <a name="move-to-the-target-region-and-confirm"></a>Hedef bölgeye taşıyın ve

1.  Sanal makine menüsünde **Olağanüstü Durum kurtarma'yı**seçin.
2. **Failover** simgesini seçin.
3. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. 
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. 
5. İş bittikten sonra, VM'nin beklendiği gibi hedef Azure bölgesinde görünün.
6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, hedef bölgeye taşıma işlemini tamamlar. Taahhüt işi bitene kadar bekleyin.

## <a name="discard-the-resource-in-the-source-region"></a>Kaynağı kaynak bölgeye atın

VM'ye git. **Çoğaltmayı Devre Dışı'** yı seçin. Bu eylem, VM için veri kopyalama işlemini durdurur.  

> [!IMPORTANT]
> Taşımadan sonra Site Kurtarma çoğaltma için ücret almamak için önceki adımı yapın. Kaynak çoğaltma ayarları otomatik olarak temizlenir. Çoğaltmanın bir parçası olarak yüklenen Site Kurtarma uzantısıkaldırılmadığını ve el ile kaldırılması gerektiğini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir kullanılabilirlik kümesine veya Kullanılabilirlik Bölgesine taşınarak bir Azure VM kullanılabilirliğini artırdınız. Artık taşınan VM için olağanüstü durum kurtarma ayarlayabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)


