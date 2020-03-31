---
title: Azure Site Kurtarma ile Azure VM'lerini devlet ve genel bölgeler arasında taşıma
description: Azure VM'lerini Azure resmi ve genel bölgeler arasında taşımak için Azure Site Kurtarma'yı kullanın.
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: acaf16e7469b3ea4e5e391db91e37dc76be3b261
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78298539"
---
# <a name="move-azure-vms-between-azure-government-and-public-regions"></a>Azure VM’lerini Azure Kamu ve Genel bölgeleri arasında taşıma 

IaaS VM'lerinizi, mevcut VM'lerinizin kullanılabilirliğini artırmak, yönetilebilirliği artırmak veya [burada](azure-to-azure-move-overview.md)ayrıntılı olarak açıklandığı gibi yönetim nedenleriyle Azure Resmi ve Genel bölgeler arasında taşımak isteyebilirsiniz.

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) amacıyla şirket içi makinelerin ve Azure VM'lerinin olağanüstü durum kurtarmasını yönetmek ve düzenlemek için [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanmanın yanı sıra, Azure VM'lerini ikincil bir bölgeye taşımayı yönetmek için Site Kurtarma'yı da kullanabilirsiniz.       

Bu öğretici, Azure Site Kurtarma'yı kullanarak Azure Kamu ve Genel bölgeler arasında Azure VM'leri nasıl taşıyabileceğinizi gösterir. Aynı coğrafi küme içinde olmayan bölge çiftleri arasında VM taşımak için aynı genişletilebilir. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Önkoşulları doğrulama
> * Kaynak VM'leri hazırlama
> * Hedef bölgeyi hazırlama
> * Verileri hedef bölgeye kopyalama
> * Yapılandırmayı test edin
> * Hareketi gerçekleştirin
> * Kaynak bölgedeki kaynakları atın

> [!IMPORTANT]
> Bu öğretici, Azure VM'lerini Azure Genel ve Genel Bölgeler arasında veya Azure VM'leri için normal olağanüstü durum kurtarma çözümü tarafından desteklenmeyen bölge çiftleri arasında nasıl taşıyabileceğinizi gösterir. Kaynak ve hedef bölge çiftlerinizin [desteklenmesi](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix#region-support)durumunda, taşıma için lütfen bu [belgeye](azure-to-azure-tutorial-migrate.md) bakın. Gereksiniminiz, farklı bir bölgede sabitlenmiş VM'lere ayarlanmış bir kullanılabilirlik kümesinde VM'leri taşıyarak kullanılabilirliği artırmaksa, [buradaki](move-azure-VMs-AVset-Azone.md)öğreticiye bakın.

> [!IMPORTANT]
> Çiftler, dr senaryosu için kritik önemtaşıyan veri gecikmesi akılda tutarak tanımlandıklarından, DR'yi desteklenmeyen bölge çiftleri arasında yapılandırmak için bu yöntemi kullanmak tavsiye edilmez.

## <a name="verify-prerequisites"></a>Önkoşulları doğrulama

> [!NOTE]
> Bu senaryonun [mimarisini ve bileşenlerini](physical-azure-architecture.md) anladığınızdan emin olun. Bu mimari, **VM'leri fiziksel sunucular olarak değerlendirerek**Azure VM'lerini taşımak için kullanılacaktır.

- Tüm bileşenler için [destek gereksinimlerini](vmware-physical-secondary-support-matrix.md) gözden geçirin.
- Çoğaltmak istediğiniz sunucuların [Azure VM gereksinimlerine](vmware-physical-secondary-support-matrix.md#replicated-vm-support)uygun olduğundan emin olun.
- Çoğaltmak istediğiniz her sunucuda Mobilite hizmetinin otomatik olarak yüklenmesi için bir hesap hazırlayın.

- Azure'da hedef bölgeye başarısız olduktan sonra kaynak bölgeye doğrudan bir başarısız lık gerçekleştiremeyeceğinizdir. Yeniden hedefe çoğaltma kurmak gerekir.

### <a name="verify-azure-account-permissions"></a>Azure hesap izinlerini doğrulama

Azure hesabınızın VM'lerin Azure'a çoğaltılması için izinleri olduğundan emin olun.

- Makineleri Azure'da çoğaltmak için gereken [izinleri](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) gözden geçirin.
- [Rol tabanlı erişim](../role-based-access-control/role-assignments-portal.md) izinlerini doğrulayın ve değiştirin. 

### <a name="set-up-an-azure-network"></a>Azure ağı ayarlama

Hedef [Azure ağı](../virtual-network/quick-create-portal.md)ayarlayın.

- Azure VM'ler, başarısız olduktan sonra oluşturulduklarında bu ağa yerleştirilir.
- Ağ, Kurtarma Hizmetleri kasası ile aynı bölgede olmalıdır


### <a name="set-up-an-azure-storage-account"></a>Azure depolama hesabı ayarlama

Bir [Azure depolama hesabı](../storage/common/storage-account-create.md)ayarlayın.

- Site Kurtarma, şirket içi makineleri Azure depolamasına kopyalar. Azure VM'leri, başarısız olduktan sonra depolama dan oluşturulur.
- Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir.


## <a name="prepare-the-source-vms"></a>Kaynak VM'leri hazırlama

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Mobilite hizmeti çoğaltmak istediğiniz her sunucuya yüklenmiş olmalıdır. Site Kurtarma, sunucu için çoğaltmayı etkinleştirdiğinizde bu hizmeti otomatik olarak yükler. Otomatik olarak yüklemek için, Site Kurtarma'nın sunucuya erişmek için kullanacağı bir hesap hazırlamanız gerekir.

- Bir etki alanı veya yerel hesap kullanabilirsiniz
- Windows VM'leri için, bir etki alanı hesabı kullanmıyorsanız, yerel makinede Uzaktan Kullanıcı Erişimi denetimini devre dışı bırakın. Bunu yapmak için, **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**altında kayıt, 1 değeri ile DWORD girişi **LocalAccountTokenFilterPolicy**ekleyin.
- CLI'den ayarı devre dışı atmak için kayıt defteri girişini eklemek için şunları yazın:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Linux için, hesap kaynak Linux sunucusunda kök olmalıdır.


## <a name="prepare-the-target-region"></a>Hedef bölgeyi hazırlama

1. Azure aboneliğinizin, olağanüstü durum kurtarma için kullanılan hedef bölgede VM’ler oluşturmanıza izin verdiğinden emin olun. Gerekli kotayı sağlamak için desteğe başvurun.

2. Aboneliğinizin, kaynak VM’lerinize uygun boyutlardaki VM’leri desteklemek için yeterli kaynakları içerdiğinden emin olun. Verileri hedefe kopyalamak için Site Kurtarma'yı kullanıyorsanız, hedef VM için aynı boyutu veya mümkün olan en yakın boyutu seçer.

3. Kaynak ağ düzeninde tanımlanan her bileşen için bir hedef kaynak oluşturduğunuzdan emin olun. Bu, hedef bölgeye kesme sonrası, VM'lerinizin kaynaktaki tüm işlevselliğe ve özelliklere sahip olduğundan emin olmak için önemlidir.

    > [!NOTE]
    > Azure Site Kurtarma, kaynak VM için çoğaltmayı etkinleştirdiğinizde otomatik olarak sanal bir ağ keşfeder ve oluşturur veya çoğaltmayı etkinleştirmek için kullanıcı akışında bir ağ önceden oluşturabilir ve VM'ye atayabilirsiniz. Ancak, aşağıda belirtildiği gibi, diğer kaynaklar için, bunları hedef bölgede el ile oluşturmanız gerekir.

     Kaynak VM yapılandırmasına bağlı olarak, sizin için en sık kullanılan ağ kaynaklarını oluşturmak için lütfen aşağıdaki belgelere bakın.

    - [Ağ Güvenlik Grupları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group)
    - [Yük dengeleyiciler](https://docs.microsoft.com/azure/load-balancer)
    - [Genel IP](../virtual-network/virtual-network-public-ip-address.md)
    
    Diğer ağ bileşenleri için ağ [belgelerine](https://docs.microsoft.com/azure/?pivot=products&panel=network)bakın.

4. Son kesmeyi hedef bölgeye gerçekleştirmeden önce yapılandırmayı test etmek istiyorsanız, hedef bölgede üretim dışı bir ağ el ile [oluşturun.](https://docs.microsoft.com/azure/virtual-network/quick-create-portal) Bu üretim ile en az girişim yaratacak ve tavsiye edilir.

## <a name="copy-data-to-the-target-region"></a>Verileri hedef bölgeye kopyalama
Aşağıdaki adımlar, verileri hedef bölgeye kopyalamak için Azure Site Kurtarma'yı nasıl kullanacağınız konusunda size yol gösterecektir.

### <a name="create-the-vault-in-any-region-except-the-source-region"></a>Kaynak bölgesi dışında herhangi bir bölgede kasayı oluşturun.

1. [Azure portalı](https://portal.azure.com) > **Kurtarma Hizmetleri'nde**oturum açın.
2. Kaynak > **Yönetim Araçları** > **Yedekleme ve Site Kurtarma** **Oluştur'u**tıklatın.
3. **Ad** bölümünde **ContosoVMVault** kolay adını belirtin. Eğer birden fazla a varsa. abonelik, uygun bir seçin.
4. Bir **ContosoRG** kaynak grubu oluşturun.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Kurtarma Hizmetleri kasalarında, **Genel Bakış** > **ConsotoVMVault** > **+Çoğaltma**
7. **Azure** > **Sanallaştırılan Değil/Diğer'i**seçin.

### <a name="set-up-the-configuration-server-to-discover-vms"></a>VM'leri keşfetmek için yapılandırma sunucusunu ayarlayın.


Yapılandırma sunucusunu ayarlayın, kasaya kaydedin ve VM'leri keşfedin.

1. **Site Kurtarma** > **Hazırlık Altyapı** > **Kaynağı'nı**tıklatın.
2. Yapılandırma sunucunuz yoksa **+Configuration server'ı**tıklatın.
3. **Add Server'da**Configuration **Server'ın** **Sunucu türünde**görünip görünmediğini denetleyin.
4. Site Kurtarma Birleşik Kurulum yükleme dosyasını indirin.
5. Kasa kayıt anahtarını indirin. Birleşik Kurulum'u çalıştırdığınızda buna ihtiyacınız vardır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Kaynağı ayarlama](./media/physical-azure-disaster-recovery/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Yapılandırma sunucusunu kasaya kaydedin

Başlamadan önce aşağıdakileri yapın: 

#### <a name="verify-time-accuracy"></a>Zaman doğruluğunu doğrula
Yapılandırma sunucusu makinesinde, sistem saatinin Bir Time [Server](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service)ile senkronize olduğundan emin olun. Eşleşmeli. Önde veya arkada 15 dakika varsa, kurulum başarısız olabilir.

#### <a name="verify-connectivity"></a>Bağlantıyı doğrulama
Makinenin ortamınıza bağlı olarak bu URL'lere erişebileceğinden emin olun: 

[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]  

IP adresi tabanlı güvenlik duvarı kuralları, https (443) bağlantı noktası üzerinde yukarıda listelenen tüm Azure URL'leri ile iletişime izin vermelidir. IP Aralıklarını basitleştirmek ve sınırlamak için URL filtrelemenin yapılması önerilir.

- **Ticari IP'ler** - [Azure Veri Merkezi IP Aralıklarına](https://www.microsoft.com/download/confirmation.aspx?id=41653)ve HTTPS (443) bağlantı noktasına izin verin. AAD, Yedekleme, Çoğaltma ve Depolama URL'lerini desteklemek için aboneliğinizin Azure bölgesi için IP adres aralıklarına izin verin.  
- **Devlet IP'leri** - AAD, Yedekleme, Çoğaltma ve Depolama URL'lerini desteklemek için Tüm USGov Bölgeleri (Virginia, Teksas, Arizona ve Iowa) için [Azure Devlet Veri Merkezi IP Aralıkları](https://www.microsoft.com/en-us/download/details.aspx?id=57063)ve HTTPS (443) bağlantı noktasına izin verin.  

#### <a name="run-setup"></a>Kurulumu çalıştırma
Yapılandırma sunucusunu yüklemek için Birleşik Kurulumu Yerel Yönetici olarak çalıştırın. İşlem sunucusu ve ana hedef sunucu da varsayılan olarak yapılandırma sunucusuna yüklenir.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Kayıt bittikten sonra yapılandırma sunucusu kasadaki **Ayarlar** > **Sunucuları** sayfasında görüntülenir.

### <a name="configure-target-settings-for-replication"></a>Çoğaltma için hedef ayarlarını yapılandırma

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapı Hedef'i Hazırla'yı** > **Target**tıklatın ve kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

   ![Hedef](./media/physical-azure-disaster-recovery/network-storage.png)


### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. Yeni bir çoğaltma ilkesi oluşturmak için **Site Kurtarma altyapısı** > **Çoğaltma İlkeleri** > **+Çoğaltma İlkesi'ni**tıklatın.
2. **Çoğaltma ilkesi oluştur** bölümünde bir ilke adı belirtin.
3. **RPO eşiği** bölümünde kurtarma noktası hedefi (RPO) sınırını belirtin. Bu değer, veri kurtarma noktalarının ne sıklıkta oluşturulduğunu belirtir. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
4. **Kurtarma noktası bekletme** bölümünde, her kurtarma noktası için bekletme süresinin ne kadar olacağını (saat) belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. Premium depolama alanına çoğaltılan makineler için 24 saate, standart depolama için de 72 saate kadar bekletme desteklenir.
5. **Uygulama tutarlı anlık görüntü sıklığında,** uygulama tutarlı anlık görüntüler içeren kurtarma noktalarının ne sıklıkta (dakika dakika içinde) oluşturulacağını belirtin. İlkeyi oluşturmak için **Tamam**’a tıklayın.

    ![Çoğaltma ilkesi](./media/physical-azure-disaster-recovery/replication-policy.png)


İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. Eşleşen bir ilke, varsayılan olarak yeniden çalışma için otomatik oluşturulur. Örneğin, çoğaltma **ilkesi rep-ilke** ise, başarısız ilke **temsilcisi-failback** oluşturulur. Bu ilke Azure’dan bir yeniden çalışma başlatılana kadar kullanılmaz.

### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

- Site Kurtarma çoğaltma etkinleştirildiğinde Mobilite hizmetini yükler.
- Bir sunucu için çoğaltmayı etkinleştirdiğinizde, değişikliklerin etkili olması 15 dakika veya daha uzun sürebilir ve portalda görünebilir.

1. Uygulama**Kaynağını** **Çoğaltma'yı** > tıklatın.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türünde,** **Fiziksel makineleri**seçin.
4. İşlem sunucusunu (yapılandırma sunucusu) seçin. Ardından **Tamam**'a tıklayın.
5. **Hedef'te,** başarısız olduktan sonra Azure VM'lerini oluşturmak istediğiniz abonelik ve kaynak grubunu seçin. Azure'da kullanmak istediğiniz dağıtım modelini (klasik veya kaynak yönetimi) seçin.
6. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin. 
7. Yük devretme sonrasında oluşturulan Azure VM'lerinin bağlanacağı Azure ağını ve alt ağını seçin.
8. Ağ ayarını koruma için seçtiğiniz tüm makinelere uygulamak **için seçili makineler için şimdi Yapılandır'ı**seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin. 
9. **Fiziksel Makineler'de**ve **+Fiziksel makine'ye**tıklayın. Adı ve IP adresini belirtin. Çoğaltmak istediğiniz makinenin işletim sistemini seçin. Sunucuların bulunması ve listelenilmesi birkaç dakika sürer. 

   > [!WARNING]
   > Taşımayı istediğiniz Azure VM'nin IP adresini girmeniz gerekir

10. **Özellikleri** > **Yapılandırma özelliklerinde,** mobilite hizmetini makineye otomatik olarak yüklemek için işlem sunucusu tarafından kullanılacak hesabı seçin.
11. **Çoğaltma ayarlarında** > **çoğaltma ayarlarını yapılandırın,** doğru çoğaltma ilkesinin seçildiğini doğrulayın. 
12. **Çoğaltmayı Etkinleştir**’e tıklayın. **Ayarlar** > **İşler** > **Site Recovery İşleri** bölümünden **Korumayı Etkinleştir** işinin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi çalıştırıldıktan sonra makine yük devretme için hazırdır.


Eklediğiniz sunucuları izlemek için, **yapılandırma sunucularında** > bunlar için son keşfedilen zamanı son**kişi adresinden**kontrol edebilirsiniz. Zamanlanan bir bulma süresini beklemeden makine eklemek için yapılandırma sunucusunu vurgulayın (tıklatmayın) ve **Yenile'yi**tıklatın.

## <a name="test-the-configuration"></a>Yapılandırmayı test edin


1. Tonoz gidin, **Ayarlar** > **Çoğaltılan öğeleri**, hedef bölgeye taşımak niyetinde Sanal makine tıklayın, **+Test Failover** simgesine tıklayın.
2. **Yük Devretme Testi** bölümünde, yük devretmede kullanılması için bir kurtarma noktası seçin:

   - **En son işlenen**: VM’nin yükünü, Site Recovery hizmeti tarafından işlenen en son kurtarma noktasına devreder. Zaman damgası gösterilir. Bu seçenekle veri işlemeye zaman harcanmadığından düşük RTO sağlanılır (Kurtarma Süresi Hedefi)
   - **Uygulamayla tutarlı olan son**: Bu seçenek, tüm VM’lerin yükünü uygulamayla tutarlı olan en son kurtarma noktasına devreder. Zaman damgası gösterilir.
   - **Özel**: Herhangi bir kurtarma noktası seçin.

3. Yapılandırmayı sınamak için Azure VM'lerini taşımak istediğiniz hedef Azure sanal ağını seçin. 

   > [!IMPORTANT]
   > Çoğaltmayı etkinleştirdiğinizde ayarlanmış olan VM'lerinizi taşımak istediğiniz üretim ağını değil, test başarısızlığı için ayrı bir Azure VM ağı kullanmanızı öneririz.

4. Hareketi test etmeye başlamak için **Tamam'ı**tıklatın. İlerleme durumunu izlemek için, VM’ye tıklayarak özelliklerini açın. Ya da kasa adı > **Ayarlar** > **İşler** > **Site Recovery işleri** bölümünde **Yük Devretme Testi** işine tıklayabilirsiniz.
5. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. VM’nin çalıştığından, uygun şekilde boyutlandırıldığından ve uygun ağa bağlı olduğundan emin olun.
6. Hareketi test etme nin bir parçası olarak oluşturulan VM'yi silmek istiyorsanız, çoğaltılan öğeüzerinde **Temizleme testi başarısız lığı'nı** tıklatın. **Notlar,testle**ilişkili gözlemleri kaydedin ve kaydedin.

## <a name="perform-the-move-to-the-target-region-and-confirm"></a>Hedef bölgeye taşımayı gerçekleştirin ve onaylayın.

1. **Tonoz** > gidin, Ayarlar**Çoğaltılan öğeleri,** sanal makineye tıklayın ve sonra **Failover**tıklatın.
2. **Yük devretme** bölümünde **En geç** seçeneğini belirleyin. 
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce kaynak sanal makineyi kapatmaya çalışır. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz. 
4. İş tamamlandıktan sonra, VM'nin beklendiği gibi hedef Azure bölgesinde görünün.
5. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Yürüt**’e tıklayın. Bu, hedef bölgeye taşıma işlemini tamamlar. Taahhüt işi tamamlanana kadar bekleyin.

## <a name="discard-the-resource-in-the-source-region"></a>Kaynağı kaynak bölgeye atın 

- VM'ye gidin.  **Çoğaltmayı Devre Dışı**Devre Tonu'na tıklayın.  Bu, VM için veri kopyalama işlemini durdurur.  

   > [!IMPORTANT]
   > ASR çoğaltma için ücret almak önlemek için bu adımı gerçekleştirmek önemlidir.

Kaynak kaynaklardan herhangi birini yeniden kullanmayı planlamanız yoksa lütfen bir sonraki adım kümesine devam edin.

1. [Kaynak VM'leri Hazırlama'da](#prepare-the-source-vms) Adım 4'ün bir parçası olarak listelediğiniz kaynak bölgedeki tüm ilgili ağ kaynaklarını silmeye devam edin 
2. Kaynak bölgedeki ilgili depolama hesabını silin.



## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde bir Azure VM'yi farklı bir Azure bölgesine taşıdınız. Artık taşınan VM için olağanüstü durum kurtarma yapılandırmayapabilirsiniz.

> [!div class="nextstepaction"]
> [Geçişten sonra olağanüstü durum kurtarmayı ayarlama](azure-to-azure-quickstart.md)
