---
title: Azure Site Kurtarma ile Windows VM'lerinizi Azure Premium Depolama alanına taşıyın
description: Site Kurtarma'yı kullanarak mevcut sanal makinelerinizi Azure Premium Depolama'ya geçirin. Premium Depolama, Azure Sanal Makinelerde çalışan Yoğun I/İş yoğun iş yükleri için yüksek performanslı, düşük gecikmeli disk desteği sunar.
author: luywang
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 08/15/2017
ms.author: luywang
ms.subservice: disks
ms.openlocfilehash: 071596557de00c9dfb2afaa5751d9331c21ada99
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81866268"
---
# <a name="migrate-to-premium-storage-by-using-azure-site-recovery"></a>Azure Site Kurtarma'yı kullanarak Premium Depolama alanına geçiş

[Azure premium SSD'ler,](disks-types.md) Yoğun Yoğun Güstit ler çalıştıran sanal makineler (VM'ler) için yüksek performanslı, düşük gecikmeli disk desteği sağlar. Bu kılavuz, [Azure Site Kurtarma'yı](../../site-recovery/site-recovery-overview.md)kullanarak VM disklerinizi standart bir depolama hesabından premium depolama hesabına geçirmenize yardımcı olur.

Site Kurtarma, şirket içi fiziksel sunucuların ve VM'lerin buluta (Azure) veya ikincil bir veri merkezine çoğaltılmasını düzenleyerek iş sürekliliği ve olağanüstü durum kurtarma stratejinize katkıda bulunan bir Azure hizmetidir. Birincil konumunuzda kesintiler oluştuğunda, uygulamaları ve iş yüklerini kullanılabilir tutmak için ikincil konuma geçemezsiniz. Normal çalışma için geri döndüğünde birincil konumunuza geri başarısız olur. 

Site Kurtarma, üretim ortamlarını etkilemeden olağanüstü durum kurtarma matkaplarını desteklemek için test arızaları sağlar. Beklenmeyen felaketler için en az veri kaybı (çoğaltma sıklığına bağlı olarak) ile failovers çalıştırabilirsiniz. Premium Depolama'ya geçiş senaryosunda, hedef diskleri premium depolama hesabına geçirmek için [Site Kurtarma'daki başarısızlığı](../../site-recovery/site-recovery-failover.md) kullanabilirsiniz.

Bu seçenek minimum kapalı kalma süresi sağladığından, Site Kurtarma'yı kullanarak Premium Depolama alanına geçiş yapmanızı öneririz. Bu seçenek, diskleri kopyalamanın ve yeni VM'lerin oluşturulmasının el ile yürütülmesini de önler. Site Kurtarma, disklerinizi sistematik olarak kopyalar ve başarısız lık sırasında yeni VM'ler oluşturur. 

Site Kurtarma, en az veya hiç kapalı kalma süresi olmadan bir dizi başarısızlık türünü destekler. Kapalı kalma sürenizi planlamak ve veri kaybını tahmin etmek için [Site Kurtarma'daki başarısızlık türlerine](../../site-recovery/site-recovery-failover.md)bakın. Başarısız olduktan [sonra Azure VM'lerine bağlanmaya hazırlanıyorsanız,](../../site-recovery/vmware-walkthrough-overview.md)başarısız olduktan sonra RDP kullanarak Azure VM'ye bağlanabilmelisin.

![Olağanüstü durum kurtarma diyagramı][1]

## <a name="azure-site-recovery-components"></a>Azure Site Kurtarma bileşenleri

Bu Site Kurtarma bileşenleri bu geçiş senaryosu yla ilgilidir:

* **Yapılandırma sunucusu,** iletişimi koordine eden ve veri çoğaltma ve kurtarma işlemlerini yöneten bir Azure VM'sidir. Bu VM'de, yapılandırma sunucusunu ve işlem sunucusu adı verilen ek bir bileşeni çoğaltma ağ geçidi olarak yüklemek için tek bir kurulum dosyası çalıştırın. Yapılandırma [sunucusu ön koşulları](../../site-recovery/vmware-walkthrough-overview.md)hakkında bilgi edinin. Yapılandırma sunucusunu yalnızca bir kez ayarlarsınız ve aynı bölgeye yapılan tüm geçişler için kullanabilirsiniz.

* **İşlem sunucusu** bir çoğaltma ağ geçidi dir: 

  1. Kaynak VM'lerden çoğaltma verileri alır.
  2. Önbelleğe alma, sıkıştırma ve şifreleme ile verileri optimize eder.
  3. Verileri bir depolama hesabına gönderir. 

  Ayrıca, mobilit hizmetinin kaynak VM'lere itme yüklemesini de işler ve kaynak VM'lerin otomatik olarak keşfini gerçekleştirir. Varsayılan işlem sunucusu yapılandırma sunucusuna yüklenir. Dağıtımınızı ölçeklendirmek için ek bağımsız işlem sunucuları dağıtabilirsiniz. İşlem [sunucusu dağıtımı](https://azure.microsoft.com/blog/best-practices-for-process-server-deployment-when-protecting-vmware-and-physical-workloads-with-azure-site-recovery/) ve ek [işlem sunucuları dağıtma](../../site-recovery/site-recovery-plan-capacity-vmware.md#deploy-additional-process-servers)için en iyi uygulamalar hakkında bilgi edinin. İşlem sunucusunu yalnızca bir kez ayarlarsınız ve aynı bölgeye yapılan tüm geçişler için kullanabilirsiniz.

* **Mobilite hizmeti,** çoğaltmak istediğiniz her standart VM'de dağıtılan bir bileşendir. Standart VM'deki verileri yakalar ve işlem sunucusuna iletir. [Çoğaltılan makine ön koşulları](../../site-recovery/vmware-walkthrough-overview.md)hakkında bilgi edinin.

Bu grafik, bu bileşenlerin nasıl etkileşimde olduğunu gösterir:

![Site Kurtarma bileşenlerinin etkileşimi][15]

> [!NOTE]
> Site Kurtarma Depolama Alanları disklerinin geçişini desteklemez.

Diğer senaryolar için ek bileşenler için [Senaryo mimarisine](../../site-recovery/vmware-walkthrough-overview.md)bakın.

## <a name="azure-essentials"></a>Azure temelleri

Bu geçiş senaryosu için Azure gereksinimleri şunlardır:

* Azure aboneliği.
* Çoğaltılan verileri depolamak için azure premium depolama hesabı.
* Başarısız olduklarında Sanal M'lerin bağlanacağı bir Azure sanal ağı. Azure sanal ağı, Site Kurtarma'nın çalıştığı ağla aynı bölgede olmalıdır.
* Çoğaltma günlüklerini depolamak için azure standart depolama hesabı. Bu, geçirilen VM diskleri için aynı depolama hesabı olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Önceki bölümde ilgili geçiş senaryosu bileşenlerini anlayın.
* [Site Kurtarma'da başarısız](../../site-recovery/site-recovery-failover.md)lık hakkında bilgi edinerek kesinti sürenizi planlayın.

## <a name="setup-and-migration-steps"></a>Kurulum ve geçiş adımları

Azure IaaS VM'lerini bölgeler arasında veya aynı bölge içinde geçirmek için Site Kurtarma'yı kullanabilirsiniz. Aşağıdaki yönergeler, [VMware VM'leri veya fiziksel sunucuları Azure'a çoğaltma](../../site-recovery/vmware-walkthrough-overview.md)makalesinden bu geçiş senaryosu için özel olarak uyarlanmıştır. Bu makaledeki talimatlara ek olarak ayrıntılı adımlar için lütfen linkleri takip edin.

### <a name="step-1-create-a-recovery-services-vault"></a>Adım 1: Kurtarma Hizmetleri kasası oluşturma

1. Azure [portalını](https://portal.azure.com)açın.
2. Kaynak > **Yönetimi** > **Yedeklemesi ve Site Kurtarma (OMS)** **oluştur'u**seçin. Alternatif olarak, **Gözat** > **Kurtarma Hizmetleri Vault** > **Ekle'yi**seçebilirsiniz.
   >[!NOTE]
   >Yedekleme ve Site Kurtarma eskiden [OMS paketinin](/azure/azure-monitor/terminology#april-2018---retirement-of-operations-management-suite-brand)bir parçasıydı.
1. VM'lerin çoğaltılacağı bir bölge belirtin. Aynı bölgede geçiş amacıyla, kaynak VM'lerinizin ve kaynak depolama hesaplarınızın bulunduğu bölgeyi seçin. 

### <a name="step-2-choose-your-protection-goals"></a>Adım 2: Koruma hedeflerinizi seçin 

1. Yapılandırma sunucusunu yüklemek istediğiniz VM'de [Azure portalını](https://portal.azure.com)açın.
2. Kurtarma **Hizmetleri vaults** > **Ayarlar** > **Site Kurtarma** > **Adım 1: Altyapı** > **Koruma hedefi**hazırlayın gidin.

   ![Koruma hedef bölmesine göz atma][2]

3. **Koruma hedefi**altında, ilk açılır listede **Azure'a'yı**seçin. İkinci açılır listede **sanallaştırılan değil / Diğer**'i seçin ve ardından **Tamam'ı**seçin.

   ![Doldurulmuş kutularla koruma hedef bölmesi][3]

### <a name="step-3-set-up-the-source-environment-configuration-server"></a>Adım 3: Kaynak ortamını ayarlama (configuration server)

1. **Azure Site Kurtarma Tümlatf Kurulum'u** ve kasa kayıt anahtarını**kaynak** > **Ekle** ekle **altyapısına** > giderek indirin. 
 
   Birleşik kurulumu çalıştırmak için kasa kayıt anahtarına ihtiyacınız olacaktır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

   ![Sunucu Ekle bölmesine göz atma][4]

2. Sunucu **Ekle** bölmesine bir yapılandırma sunucusu ekleyin.

   ![Configuration Server seçili sunucu bölmesi ekle][5]

3. Yapılandırma sunucusu olarak kullandığınız VM'de, yapılandırma sunucusunu ve işlem sunucusunu yüklemek için Unified Setup'ı çalıştırın. Yüklemeyi tamamlamak için ekran görüntüleri arasında [yürüyebilirsiniz.](../../site-recovery/vmware-walkthrough-overview.md) Bu geçiş senaryosu için belirtilen adımlar için aşağıdaki ekran görüntülerine başvurabilirsiniz.

   1. **Başlamadan Önce,** **yapılandırma sunucusu ve işlem sunucusu yükleyin**seçin.

      ![Sayfaya Başlamadan Önce][6]

   2. **Kayıt'ta**kasadan indirdiğiniz kayıt anahtarına göz atın ve seçin.

      ![Kayıt sayfası][7]

   3. **Ortam Ayrıntıları**’nda VMware sanal makinelerini çoğaltıp çoğaltmayacağınızı seçin. Bu geçiş senaryosu için **Hayır'ı**seçin.

      ![Çevre Ayrıntıları sayfası][8]

4. Yükleme tamamlandıktan sonra, **Microsoft Azure Site Kurtarma Yapılandırma Sunucusu** penceresinde aşağıdakileri yapın:
 
   1. Site Kurtarma'nın otomatik keşif için kullanabileceği hesabı oluşturmak için **Hesapları Yönet** sekmesini kullanın. (Fiziksel makineleri koruma yla ilgili senaryoda, hesabı ayarlamak önemli değildir, ancak aşağıdaki adımlardan birini etkinleştirmek için en az bir hesaba ihtiyacınız vardır. Bu durumda, hesap ve parolayı herhangi bir şekilde adlandırabilirsiniz.) 
   2. Vault **kimlik** bilgileri dosyasını yüklemek için Vault Registration sekmesini kullanın.

      ![Kasa Kayıt sekmesi][9]

### <a name="step-4-set-up-the-target-environment"></a>Adım 4: Hedef ortamı ayarlama

Altyapı > **Hedef** **In'i**seçin ve başarısız olduktan sonra VM'ler için kullanmak istediğiniz dağıtım modelini belirtin. Senaryonuza bağlı olarak **Klasik** veya **Kaynak Yöneticisi'ni**seçebilirsiniz.

![Hedef bölme][10]

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler. 

> [!NOTE]
> Çoğaltılan veriler için premium depolama hesabı kullanıyorsanız, çoğaltma günlüklerini depolamak için ek bir standart depolama hesabı ayarlamanız gerekir.

### <a name="step-5-set-up-replication-settings"></a>Adım 5: Çoğaltma ayarlarını ayarlama

Yapılandırma sunucunuzun oluşturduğunuz çoğaltma ilkesiyle başarıyla ilişkili olduğunu doğrulamak için [çoğaltma ayarlarını ayarla'yı](../../site-recovery/vmware-walkthrough-overview.md)izleyin.

### <a name="step-6-plan-capacity"></a>Adım 6: Kapasiteyi planlayın

1. Çoğaltma gereksinimlerinizi karşılamak için ağ bant genişliğini, depolamayı ve diğer gereksinimleri doğru bir şekilde tahmin etmek için [kapasite planlayıcısını](../../site-recovery/site-recovery-capacity-planner.md) kullanın. 
2. İşinizi bitirdiğinizde, **Evet'i seçin, kapasite** **planlamayı tamamladınız mı?**

   ![Kapasite planlamayı tamamladığınızı onaylamak için kutu][11]

### <a name="step-7-install-the-mobility-service-and-enable-replication"></a>Adım 7: Mobilite hizmetini yükleyin ve çoğaltmayı etkinleştirin

1. [Yüklemeyi](../../site-recovery/vmware-walkthrough-overview.md) kaynak VM'lerinize yüklemeyi veya taşınabilirlik hizmetini kaynak VM'lerinize [el ile yüklemeyi](../../site-recovery/site-recovery-vmware-to-azure-install-mob-svc.md) seçebilirsiniz. Yüklemeyi zorlama gereksinimini ve manuel yükleyicinin yolunu sağlanan bağlantıda bulabilirsiniz. El ile yükleme yapıyorsanız, yapılandırma sunucusunu bulmak için dahili bir IP adresi kullanmanız gerekebilir.

   ![Yapılandırma Sunucu Ayrıntıları sayfası][12]

   Başarısız vm iki geçici diskler olacaktır: birincil VM ve kurtarma bölgesinde VM sağlanması sırasında oluşturulan diğer. Çoğaltmadan önce geçici diski hariç tutmak için, çoğaltmayı etkinleştirmeden önce mobilite hizmetini yükleyin. Geçici diski nasıl dışlarız hakkında daha fazla bilgi edinmek için [bkz.](../../site-recovery/vmware-walkthrough-overview.md)

2. Aşağıda belirtilen şekilde çoğaltmayı etkinleştirin:
   1. **Uygulama** > **Kaynağını**Çoğaltma'yı seçin. Çoğaltmayı ilk kez etkinleştirdikten sonra, ek makineler için çoğaltmayı etkinleştirmek için kasada **+Çoğaltma'yı** seçin.
   2. Adım 1'de, **Kaynak'ı** işlem sunucunuz olarak ayarlayın.
   3. 2. adımda, başarısız olan dağıtım sonrası model, geçiş için bir premium depolama hesabı, günlükleri kaydetmek için standart bir depolama hesabı ve başarısız olacak sanal ağ belirtin.
   4. Adım 3'te, IP adresine göre korumalı VM'ler ekleyin. (Bunları bulmak için dahili bir IP adresine ihtiyacınız olabilir.)
   5. 4. adımda, işlem sunucusunda daha önce ayarladığınız hesapları seçerek özellikleri yapılandırın.
   6. Adım 5'te, daha önce oluşturduğunuz çoğaltma ilkesini seçin "Adım 5: Çoğaltma ayarlarını ayarlayın."
   7. **Tamam'ı**seçin.

   > [!NOTE]
   > Bir Azure VM'si devre dendiğinde ve yeniden başlatıldığında, aynı IP adresini alacağının garantisi yoktur. Yapılandırma sunucusunun/işlem sunucusunun veya korumalı Azure VM'lerinin IP adresi değişirse, bu senaryodaki çoğaltma düzgün çalışmayabilir.

   ![Kaynak seçili çoğaltma bölmesini etkinleştirme][13]

Azure Depolama ortamınızı tasarlarken, bir kullanılabilirlik kümesinde her VM için ayrı depolama hesapları kullanmanızı öneririz. [Her kullanılabilirlik kümesi için birden çok depolama hesabı kullanmak için](../linux/manage-availability.md)depolama katmanındaki en iyi uygulamayı izlemenizi öneririz. VM disklerinin birden çok depolama hesabına dağıtılması, depolama kullanılabilirliğini artırmaya yardımcı olur ve G/Ç'yi Azure depolama altyapısına dağıtır.

VM'leriniz bir kullanılabilirlik kümesindeyse, tüm VM'lerin disklerini tek bir depolama hesabına çoğaltmak yerine, birden çok vm'yi birden çok kez geçirmenizi öneririz. Bu şekilde, aynı kullanılabilirlik kümesindeki VM'ler tek bir depolama hesabını paylaşmaz. Her VM için teker teker bir hedef depolama hesabı ayarlamak için **Çoğaltmayı Etkinleştir** bölmesini kullanın.
 
İhtiyaçlarınıza göre bir post-failover dağıtım modeli seçebilirsiniz. Başarısız sonrası dağıtım modeliniz olarak Azure Kaynak Yöneticisi'ni seçerseniz, bir VM (Kaynak Yöneticisi) üzerinden VM (Kaynak Yöneticisi) üzerinden başarısız olabilir veya vm (klasik) üzerinden VM (Kaynak Yöneticisi) üzerinden başarısız olabilirsiniz.

### <a name="step-8-run-a-test-failover"></a>Adım 8: Bir test başarısızlığını çalıştırma

Çoğaltma işleminizin tamamlanıp tamamlanmadığını kontrol etmek için, Site Kurtarma örneğini seçin ve ardından**Yinelenen Öğeleri** **Ayarlar'ı** > seçin. Çoğaltma işleminizin durumunu ve yüzdesini görürsünüz. 

İlk çoğaltma tamamlandıktan sonra, çoğaltma stratejinizi doğrulamak için bir test başarısızlığını çalıştırın. Bir test başarısızlığı yla ilgili ayrıntılı adımlar [için](../../site-recovery/vmware-walkthrough-overview.md)bkz. 

> [!NOTE]
> Herhangi bir başarısızlığı çalıştırmadan önce, VM'lerinizin ve çoğaltma stratejinizin gereksinimleri karşıladığından emin olun. Bir test başarısızlığını çalıştırma hakkında daha fazla bilgi için, [Site Kurtarma'da Azure'a test başarısızlığını](../../site-recovery/site-recovery-test-failover-to-azure.md)görün.

**Ayarlar** > **İşleri** > *YOUR_FAILOVER_PLAN_NAME'da*test başarısızlığı durumunu görebilirsiniz. Bölmede, adımların ve başarı/başarısızlık sonuçlarının dökümünü görebilirsiniz. Test hatası herhangi bir adımda başarısız olursa, hata iletisini denetlemek için adımı seçin. 

### <a name="step-9-run-a-failover"></a>Adım 9: Bir başarısızlık çalıştırın

Test başarısızlığını tamamladıktan sonra, disklerinizi Premium Depolama'ya geçirmek ve VM örneklerini çoğaltmak için bir başarısızlık çalıştırın. [Bir failover çalıştır'daki](../../site-recovery/site-recovery-failover.md#run-a-failover)ayrıntılı adımları izleyin. 

**VM'leri kapatın'ı seçtiğinizden ve en son verileri eşitlediğinden**emin olun. Bu seçenek, Site Kurtarma'nın korunan VM'leri kapatmayı ve verilerin en son sürümünün üzerinde başarısız olması için verileri eşitlemeye çalışması gerektiğini belirtir. Bu seçeneği seçmezseniz veya girişim başarılı olmazsa, başarısızlık VM için kullanılabilir en son kurtarma noktasından olacaktır. 

Site Kurtarma, türü Premium Depolama özellikli bir VM ile aynı veya benzer bir VM örneği oluşturur. Windows Sanal Makineler Fiyatlandırma veya [Linux Sanal Makineler](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) [Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) giderek çeşitli VM örneklerinin performansını ve fiyatını kontrol edebilirsiniz.

## <a name="post-migration-steps"></a>Geçiş sonrası adımları

1. **Çoğaltılan VM'leri varsa kullanılabilirlik kümesine yapılandırın.** Site Kurtarma, kullanılabilirlik kümesiyle birlikte geçiş yapan VM'leri desteklemez. Çoğaltılan VM'nizin dağıtımına bağlı olarak aşağıdakilerden birini yapın:
   * Klasik dağıtım modeli yle oluşturulan bir VM için: VM'yi Azure portalında ayarlanan kullanılabilirlik durumuna ekleyin. Ayrıntılı adımlar için, [varolan sanal makineyi kullanılabilirlik kümesine ekle'ye](../linux/classic/configure-availability-classic.md)gidin.
   * Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan bir VM için: VM yapılandırmanızı kaydedin ve ardından kullanılabilirlik kümesindeki VM'leri silip yeniden oluşturun. Bunu yapmak için, Komut dosyasını [Azure Kaynak Yöneticisi VM Kullanılabilirlik Kümesi'nde](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4)kullanın. Bu komut dosyasını çalıştırmadan önce, sınırlamalarını denetleyin ve kapalı kalma sürenizi planlayın.

2. **Eski VM'leri ve diskleri silin.** Premium disklerin kaynak disklerle tutarlı olduğundan ve yeni VM'lerin kaynak VM'lerle aynı işlevi gösterdiğinden emin olun. Azure portalındaki kaynak depolama hesaplarınızdaki VM'yi silin ve diskleri silin. VM'yi silseniz bile diskin silinmemesi ile ilgili bir sorun varsa, [Sorun Giderme depolama kaynağı silme hatalarına](storage-resource-deletion-errors.md)bakın.

3. **Azure Site Kurtarma altyapısını temizleyin.** Site Kurtarma'ya artık gerek yoksa, altyapısını temizleyebilirsiniz. Çoğaltılan öğeleri, yapılandırma sunucusunu ve kurtarma ilkesini silin ve ardından Azure Site Kurtarma kasasını silin.

## <a name="troubleshooting"></a>Sorun giderme

* [Sanal makineler ve fiziksel sunucular için izleme ve sorun giderme koruması](../../site-recovery/site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft Azure Site Kurtarma forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleri geçirmek için belirli senaryolar için aşağıdaki kaynaklara bakın:

* [Azure Sanal Makinelerini Depolama Hesapları arasında geçirme](https://azure.microsoft.com/blog/2014/10/22/migrate-azure-virtual-machines-between-storage-accounts/)
* [Windows Server VHD oluşturma ve Azure’a yükleme](upload-generalized-managed.md)
* [Sanal Makineleri Amazon AWS'den Microsoft Azure'a geçirme](https://channel9.msdn.com/Series/Migrating-Virtual-Machines-from-Amazon-AWS-to-Microsoft-Azure)

Ayrıca, Azure Depolama ve Azure Sanal Makineleri hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)
* [Azure Sanal Makineler](https://azure.microsoft.com/documentation/services/virtual-machines/)

[1]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-1.png
[2]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-2.png
[3]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-3.png
[4]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-4.png
[5]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-5.png
[6]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-6.PNG
[7]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-7.PNG
[8]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-8.PNG
[9]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-9.PNG
[10]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-10.png
[11]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-11.PNG
[12]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-12.PNG
[13]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-13.png
[14]:../site-recovery/media/site-recovery-vmware-to-azure/v2a-architecture-henry.png
[15]:./media/migrate-to-premium-storage-using-azure-site-recovery/migrate-to-premium-storage-using-azure-site-recovery-14.png
