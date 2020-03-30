---
title: Azure Site Kurtarma'da başarısız lık ve geri ödeme hakkında
description: Azure Site Kurtarma'da başarısız ve başarısız hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: d9b54f3c452212e12419a5ffd67b116c8660308d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79281814"
---
# <a name="about-on-premises-disaster-recovery-failoverfailback"></a>Şirket içi olağanüstü durum kurtarma failover/failback hakkında

Bu makalede, şirket içi makinelerin [Azure Site Kurtarma](site-recovery-overview.md)ile Azure'a olağanüstü kurtarma sırasında başarısız ve başarısız bir şekilde geri dönüşe genel bir bakış sağlar.

## <a name="recovery-stages"></a>Kurtarma aşamaları

Site Kurtarma'da failover ve failback'in dört aşaması vardır:

- **Aşama 1: Şirket içinde başarısız olun**: Şirket içi makineler için Azure'a çoğaltma ayarladıktan sonra, şirket içi siteniz çöktükten sonra bu makineleri Azure'da başarısız elerleebilirsiniz. Başarısız olduktan sonra, Azure SANAL'ları çoğaltılan verilerden oluşturulur.
- **Aşama 2: Azure VM'lerini Yeniden Koruyun**: Azure'da Azure VM'lerini şirket içi siteye çoğaltmaya başlayacak şekilde yeniden korursunuz. Veri tutarlılığının sağlanmasına yardımcı olmak için şirket içi VM (varsa) yeniden koruma sırasında kapatılır.
- **Aşama 3: Azure'dan başarısız olun**: Şirket içi siteniz yeniden normal olarak çalışıyorsa, bu kez Azure VM'lerini şirket içi sitenize geri döndürecek başka bir başarısızlık çalıştırın. Başarısız olduğunuz özgün konuma veya başka bir konuma geri dönebilirsiniz.
- **4. Aşama: Şirket içi makineleri yeniden koruma**: Geri başarısız olduktan sonra, şirket içi makinelerin Azure'a çoğaltılmasını yeniden etkinleştirin.

## <a name="failover"></a>Yük devretme

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinizin bir parçası olarak bir başarısızlık gerçekleştirin.

- BCDR stratejinizin ilk adımı olarak, şirket içi makinelerinizi sürekli olarak Azure'da çoğaltırsınız. Kullanıcılar şirket içi kaynak makinelerde çalışan iş yüklerine ve uygulamalara erişir.
- Gereksinim ortaya çıkarsa, örneğin şirket içinde bir kesinti varsa, çoğaltma makinelerini Azure'da başarısız olursunuz. Azure SANAL'ları çoğaltılan veriler kullanılarak oluşturulur.
- İş sürekliliği için, kullanıcılar Azure VM'lerinde uygulamalara erişmeye devam edebilir.

Failover iki aşamalı bir etkinliktir:

- **Failover**: Seçili kurtarma noktasını kullanarak bir Azure VM oluşturan ve ortaya çıkaran başarısız lık.
- **Commit**: Azure'da VM'yi doğruladıktan sonra:
    - Daha sonra failover'ı seçili kurtarma noktasına bağlayabilir veya işleme için farklı bir nokta seçebilirsiniz.
    - Başarısız olduktan sonra, kurtarma noktası değiştirilemez.


## <a name="connect-to-azure-after-failover"></a>Başarısız olduktan sonra Azure'a bağlanma

RDP/SSH kullanılarak başarısız olduktan sonra oluşturulan Azure VM'lerine bağlanmak için bir dizi gereksinim vardır.

**Başarısızlık** | **Konum** | **Eylemler**
--- | --- | ---
**Azure VM (Windows)** | Başarısız olmadan önce şirket içi makinede | **Internet üzerinden erişim**: RDP'yi etkinleştirin. **Genel**kullanıma yönelik TCP ve UDP kurallarının eklenmiştir ve **Windows Güvenlik Duvarı** > **İzin Verilen Uygulamalardaki**tüm profiller için RDP'ye izin verilir.<br/><br/> **Siteden siteye VPN üzerinden erişim**: Makinede RDP'yi etkinleştirin. **Etki Alanı** ve Özel ağlar için RDP'ye Windows Güvenlik **Duvarı** -> İzin Verilen**uygulamalarda ve özelliklerde**izin verilip verilmediğini denetleyin.<br/><br/>  İşletim sistemi SAN ilkesinin **OnlineAll**olarak ayarlandıklarına emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Bir başarısızlığı tetiklediğinde VM'de bekleyen Windows güncelleştirmeleri olmadığından emin olun. Windows Update, başarısız olduğunuzda başlayabilir ve güncelleştirmeler bitene kadar VM'de oturum açamazsınız.
**Windows çalıştıran Azure VM** | Başarısız olduktan sonra Azure VM'de |  VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> VM üzerinde başarısız olan ağ güvenlik grubu kuralları (ve bağlı olduğu Azure alt ağı) RDP bağlantı noktasına gelen bağlantılara izin vermelidir.<br/><br/> VM'nin ekran görüntüsünü doğrulamak için **Önyükleme tanılamalarını** denetleyin. Bağlanamıyorsanız, VM'nin çalıştığını denetleyin ve [sorun giderme ipuçlarını](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)gözden geçirin.
**Linux çalıştıran Azure VM** | Başarısız olmadan önce şirket içi makinede | VM'deki Secure Shell hizmetinin sistem önyüklemede otomatik olarak başlatılıdığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran Azure VM** | Başarısız olduktan sonra Azure VM'de | VM üzerinden başarısız olan ağ güvenlik grubu kurallarının (ve bağlı olduğu Azure alt ağının) SSH bağlantı noktasına gelen bağlantılara izin vermeleri gerekir.<br/><br/> VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> VM'nin ekran görüntüsü için **Önyükleme tanılamalarını** kontrol edin.<br/><br/>

## <a name="types-of-failover"></a>Başarısızlık türleri

Site Kurtarma farklı failover seçenekleri sağlar.

**Başarısızlık** | **Şey** | **Kurtarma** | **Iş akışı**
--- | --- | --- | ---
**Test başarısız** | BcDR stratejinizi doğrulayan bir matkabı, veri kaybı veya kapalı kalma süresi olmadan çalıştırmak için kullanılır.| VM'nin Azure'da bir kopyasını oluşturur, bu da devam eden çoğaltma veya üretim ortamınız üzerinde hiçbir etkisi olmaz. | 1. Tek bir VM'de veya kurtarma planında birden çok VM'de bir test başarısızlığını çalıştırın.<br/><br/> 2. Test başarısız olması için kullanılacak bir kurtarma noktası seçin.<br/><br/> 3. Azure VM'nin başarısız olduktan sonra oluşturulduğunda bulunacağı bir Azure ağı seçin. Ağ yalnızca test başarısız olmak için kullanılır.<br/><br/> 4. Matkabın beklendiği gibi çalıştığını doğrulayın. Site Kurtarma, alıştırma sırasında Azure'da oluşturulan VM'leri otomatik olarak temizler.
**Planlanan failover-Hyper-V**  | Genellikle planlı kapalı kalma süresi için kullanılır.<br/><br/> Kaynak VM'ler kapatılır. En son veriler, başarısız işlemi başlatmadan önce eşitlenir. | Planlanan iş akışı için sıfır veri kaybı. | 1. Bir kesinti bakım penceresi planlayın ve kullanıcıları bilgilendirin.<br/><br/> 2. Kullanıcıya bakan uygulamaları çevrimdışına alın.<br/><br/> 3. En son kurtarma noktası ile planlanan bir başarısızlık başlatın. Makine kapatılmazsa veya hatalarla karşılaşılırsa, hata geçersiz olmaz.<br/><br/> 4. Başarısız olduktan sonra Azure VM çoğaltmasının Azure'da etkin olup olmadığını kontrol edin.<br/><br/> 5. Bitirmek için failover taahhüt. İşlem tüm kurtarma noktalarını siler.
**Failover-Hiper-V** | Genellikle planlanmamış bir kesinti varsa veya birincil site kullanılamıyorsa çalıştırın.<br/><br/> İsteğe bağlı olarak VM'yi kapatın ve başarısızı başlatmadan önce son değişiklikleri eşitle.  | Uygulamalar için en az veri kaybı. | 1. BCDR planınızı başlatın. <br/><br/> 2. Bir failover başlatın. Site Kurtarma'nın VM'yi kapatıp kapatmaması ve failover'ı tetiklemeden önce en son değişiklikleri eşitleyip çoğaltması gerekip gerekmediğini belirtin.<br/><br/> 3. Aşağıdaki tabloda özetlenen bir dizi kurtarma noktası seçeneğinde başarısız olabilirsiniz.<br/><br/> VM'yi kapatma seçeneğini etkinleştiremezseniz veya Site Kurtarma kapatamazsa, en son kurtarma noktası kullanılır.<br/>Makine kapatılamayabilir sayılsa bile arıza çalışır.<br/><br/> 4. Başarısız olduktan sonra, Azure VM çoğaltmasının Azure'da etkin olup olmadığını kontrol edeyim.<br/> Gerekirse, 24 saatlik bekletme penceresinden farklı bir kurtarma noktası seçebilirsiniz.<br/><br/> 5. Bitirmek için failover taahhüt. İşlem, kullanılabilir tüm kurtarma noktalarını siler.
**Failover-VMware** | Genellikle planlanmamış bir kesinti varsa veya birincil site kullanılamıyorsa çalıştırın.<br/><br/> İsteğe bağlı olarak, Site Kurtarma'nın VM'nin kapatılmasını tetiklemeye çalışması ve başarısızlığı başlatmadan önce son değişiklikleri eşitleyip çoğaltması gerektiğini belirtin.  | Uygulamalar için en az veri kaybı. | 1. BCDR planınızı başlatın. <br/><br/> 2. Site Kurtarma bir failover başlatın. Site Kurtarma'nın başarısız olmadan önce VM kapatmayı tetikleyip eşitlemeye çalışıp çalışmayacağını belirtin.<br/> Makineler kapatılamaysa bile arıza çalışır.<br/><br/> 3. Başarısız olduktan sonra Azure VM yinelemesinin Azure'da etkin olup olmadığını kontrol edin. <br/>Gerekirse, 72 saatlik bekletme penceresinden farklı bir kurtarma noktası seçebilirsiniz.<br/><br/> 5. Bitirmek için failover taahhüt. İşlem tüm kurtarma noktalarını siler.<br/> Windows VM'ler için Site Kurtarma, başarısız lık sırasında VMware araçlarını devre dışı kılabilir.

## <a name="failover-processing"></a>Failover işleme

Bazı senaryolarda, failover tamamlanması yaklaşık 8 ila 10 dakika sürer ek işleme gerektirir. Daha uzun test başarısız kalma süreleri fark edebilirsiniz:

* 9,8'den eski bir Mobilite hizmet sürümü çalıştıran VMware VM'ler.
* Fiziksel sunucular.
* VMware Linux VM'ler.
* Hyper-V VM'ler fiziksel sunucu lar olarak korunur.
* DHCP hizmeti etkin olmayan VMware VM'ler.
* Aşağıdaki önyükleme sürücüleri olmayan VMware VM'ler: storvsc, vmbus, storflt, intelide, atapi.

## <a name="recovery-point-options"></a>Kurtarma noktası seçenekleri

Başarısız lık sırasında, bir dizi kurtarma noktası seçeneği seçebilirsiniz.

**Seçeneği** | **Şey**
--- | ---
**En son (en düşük RPO)** | Bu seçenek en düşük kurtarma noktası hedefi (RPO) sağlar. Önce site kurtarma hizmetine gönderilen tüm verileri işler, her VM için bir kurtarma noktası oluşturmak için, üzerinde başarısız önce. Bu kurtarma noktası, başarısız lık tetiklendiğinde Site Kurtarma'ya çoğaltılan tüm verileri sağlar.
**En son işlenmiş**  | Bu seçenek, Site Kurtarma tarafından işlenen en son kurtarma noktasına VM'ler üzerinde başarısız olur. Belirli bir VM için en son kurtarma noktasını görmek için VM ayarlarında **En Son Kurtarma Noktaları'na** bakın. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
**En son uygulama tutarlılığı** |  Bu seçenek, uygulama tutarlı kurtarma noktaları etkinse, Site Kurtarma tarafından işlenen en son uygulama tutarlı kurtarma noktası VM'ler üzerinde başarısız olur. VM ayarlarındaki en son kurtarma noktasını kontrol edin.
**En son çoklu VM işlenmiş** | Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM'li kurtarma planları için kullanılabilir. Ayarı etkin olan VM'ler en son ortak multi-VM tutarlı kurtarma noktasına geçemez. Plandaki diğer VM'ler en son işlenmiş kurtarma noktasına geçemez.
**En son çoklu VM uygulaması tutarlı** |  Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM'li kurtarma planları için kullanılabilir. Çoğaltma grubunun bir parçası olan VM'ler en son ortak çoklu VM uygulama tutarlı kurtarma noktasına geçemez. Diğer VM'ler en son uygulama tutarlı kurtarma noktasına geçemedi.
**Özel** | Belirli bir VM üzerinde zaman içinde belirli bir kurtarma noktasına başarısız olmak için bu seçeneği kullanın. Bu seçenek kurtarma planları için kullanılamaz.

> [!NOTE]
> Kurtarma noktaları başka bir Kurtarma Hizmetleri kasasına geçirilemiyor.

## <a name="reprotectionfailback"></a>Yeniden koruma/geri dönüş

Azure'a geçilen başarısız olduktan sonra, çoğaltılan Azure VM'leri korumasız durumdadır.

- Şirket içi sitenize geri dönmenin ilk adımı olarak, Azure VM'lerini şirket içinde çoğaltmaya başlamanız gerekir. Yeniden koruma işlemi, başarısız olduğunuz makinelerin türüne bağlıdır.
- Makineler Azure'dan şirket içi çoğaltma dan sonra, Azure'dan şirket içi sitenize bir arıza çalıştırabilirsiniz.
- Makineler şirket içinde yeniden çalışmaya başladıktan sonra, olağanüstü durum kurtarma için Azure'da çoğaltmak için çoğaltmayı etkinleştirebilirsiniz.

Failback aşağıdaki gibi çalışır:

- Geri başarısız olmak için, bir VM geri başarısız olmak için en az bir kurtarma noktası gerekir. Kurtarma planında, plandaki tüm VM'lerin en az bir kurtarma noktası gerekir.
- Geri başarısız olmak için **En Son** kurtarma noktasını kullanmanızı öneririz (bu kilitlenme tutarlı bir noktadır).
    - Uygulama tutarlı bir kurtarma noktası seçeneği vardır. Bu durumda, tek bir VM en son kullanılabilir uygulama tutarlı kurtarma noktasına kurtarır. Çoğaltma grubu olan bir kurtarma planı için, her çoğaltma grubu kullanılabilir ortak kurtarma noktasına kurtarır.
    - Uygulama tutarlı kurtarma noktaları zaman içinde geride olabilir ve veri kaybı olabilir.
- Azure'dan şirket içi siteye geçememesi sırasında Site Kurtarma, Azure VM'lerini kapatır. Başarısız olduğunuzda, Site Kurtarma Azure'da başarısız olan Azure VM'lerini kaldırır.


## <a name="vmwarephysical-reprotectionfailback"></a>VMware/fiziksel yeniden koruma/geri dönüş

VMware makinelerini ve fiziksel sunucularını Azure'dan şirket başına yeniden korumak ve başarısız olmak için bir geri ödeme altyapısına ve bir dizi gereksinime ihtiyacınız vardır.

- **Azure'daki geçici işlem sunucusu**: Azure'dan geri dönmek için, Azure'dan çoğaltma işlemini işlemek için bir işlem sunucusu olarak hareket edecek bir Azure VM'si ayarlarsınız. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: Geri başarısız olmak için Azure ağından şirket içi siteye VPN bağlantısına (veya ExpressRoute)'ye ihtiyacınız vardır.
- **Ayrı ana hedef sunucu**: Varsayılan olarak, şirket içi VMware VM'de yapılandırma sunucusuyla yüklenen ana hedef sunucu failback'i işler. Büyük hacimlerde trafikte başarısız olmanız gerekiyorsa, bu amaç için ayrı bir şirket içi ana hedef sunucusu ayarlayın.
- **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke, şirket içinden Azure'a bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturulur.
    - Bu ilke yapılandırma sunucusuyla otomatik olarak ilişkilidir.
    - Bu ilkeyi kaldıramazsınız.
    - İlke değerleri: RPO eşiği - 15 dakika; Kurtarma noktası tutma - 24 Saat; Uygulama tutarlı anlık frekans - 60 dakika.

VMware/fiziksel yeniden koruma ve geri dönüş hakkında daha fazla bilgi edinin:
- Yeniden koruma ve geri dönüş için ek gereksinimleri [gözden geçirin.](vmware-azure-reprotect.md#before-you-begin)
- Azure'da bir işlem sunucusu [dağıtın.](vmware-azure-prepare-failback.md#deploy-a-process-server-in-azure)
- Ayrı bir ana hedef sunucusu [dağıtın.](vmware-azure-prepare-failback.md#deploy-a-separate-master-target-server)

Azure VM'leri şirket içinde yeniden koruduğınızda, özgün konuma veya alternatif bir konuma geri dönmek istediğinizi belirtebilirsiniz.

- **Özgün konum kurtarma**: Bu, varsa Azure'dan aynı kaynak için şirket içi makineye geri başarısız olur. Bu senaryoda, yalnızca değişiklikler şirket içinde çoğaltılır.
- **Alternatif konum kurtarma**: Şirket içi makine yoksa, Azure'dan alternatif bir konuma geri dönebilirsiniz. Azure VM'yi şirket içinde yeniden koruduğunuzda, şirket içi makine oluşturulur. Tam veri çoğaltma, Azure'dan şirket içi'ne kadar gerçekleşir. - - Konum geri tepmesi için gereksinimleri ve sınırlamaları [gözden geçirin.](concepts-types-of-failback.md)



## <a name="hyper-v-reprotectionfailback"></a>Hyper-V yeniden koruma/geri dönüş

Hyper-V VM'leri Azure'dan şirket başına yeniden korumak ve geri almak için:

- Yalnızca bir depolama hesabı kullanarak çoğaltma Hyper-V VM'leri geri başarısız olabilirsiniz. Yönetilen diskler kullanılarak çoğaltılan Hyper-V VM'lerin geri tepmesi desteklenmez.
- Şirket içi Hyper-V ana bilgisayarları (veya kullanılırsa Sistem Merkezi VMM) Azure'a bağlanmalıdır.
- Azure'dan şirket içi planlı bir geri dönüş çalıştırın.
- Hyper-V VM geri tepmesi için belirli bileşenlerin ayarlanılması gerekmez.
- Planlı hata sırasında, failback'den önce verileri eşitlemek için seçenekler seçebilirsiniz:
    - Başarısız olmadan **önce verileri senkronize etme**: Bu seçenek, sanal makinelerin kapatmadan makineleri senkronize ederken kapalı kalma süresini en aza indirir.
        - Aşama 1: Azure VM'nin anlık görüntüsünü alır ve şirket içi Hyper-V ana bilgisayara kopyalar. Makine Azure'da çalışmaya devam ediyor.
        - Aşama 2: Azure VM'yi kapatır, böylece orada yeni değişiklikler olmaz. Delta değişikliklerinin son kümesi şirket içi sunucuya aktarılır ve şirket içi VM başlatılır.
    - **Yalnızca başarısız olurken verileri eşitleyin**: Bu seçenek, diskin çoğunun değiştiğini beklediğimiz ve bu nedenle checksum hesaplamaları gerçekleştirmediğimiz için daha hızlıdır. Diskin karşıdan yükleniyor. VM bir süredir (bir ay veya daha uzun bir süredir Azure'da çalışıyorsa) veya şirket içi VM silinmişse bu seçeneği kullanmanızı öneririz.

Hyper-V yeniden koruma ve geri dönüş hakkında [daha fazla bilgi edinin.](hyper-v-azure-failback.md)

Azure VM'leri şirket içinde yeniden koruduğınızda, özgün konuma veya alternatif bir konuma geri dönmek istediğinizi belirtebilirsiniz.

- **Özgün konum kurtarma**: Bu, varsa Azure'dan aynı kaynak için şirket içi makineye geri başarısız olur. Bu senaryoda, önceki yordamda açıklanan eşitleme seçeneklerinden birini seçersiniz.
- **Alternatif konum kurtarma**: Şirket içi makine yoksa, Azure'dan alternatif bir konuma geri dönebilirsiniz. Azure VM'yi şirket içinde yeniden koruduğunuzda, şirket içi makine oluşturulur. Bu seçenekle, veri eşitleme seçeneğini seçmenizi öneririz
- Konum geri başarısızlığına yönelik gereksinimleri ve sınırlamaları [gözden geçirin.](hyper-v-azure-failback.md)


Şirket içi siteye geri dönmeyip, VM'yi Azure'a çoğaltmaya ve döngüyü tamamlamaya başlamak için **Ters Çoğaltma'yı** etkinleştirirsiniz.




## <a name="next-steps"></a>Sonraki adımlar
- Belirli [VMware VM'ler](vmware-azure-tutorial-failover-failback.md) üzerinde başarısız
- Belirli [Hyper-V VM'ler](hyper-v-azure-failover-failback-tutorial.md)üzerinde başarısız olun.
- Bir kurtarma planı [oluşturun.](site-recovery-create-recovery-plans.md)
- Kurtarma [planında VM'ler](site-recovery-failover.md)üzerinde başarısız.
- [Hazırlanın](vmware-azure-failback.md) VMware yeniden koruma ve geri başarısız.
- [Hyper-V VM'leri](hyper-v-azure-failback.md)geri başarısız edin.

