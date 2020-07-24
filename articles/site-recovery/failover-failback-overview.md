---
title: Azure Site Recovery 'de yük devretme ve yeniden çalışma hakkında
description: Azure Site Recovery yük devretme ve failable hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 12/24/2019
ms.openlocfilehash: d9b54f3c452212e12419a5ffd67b116c8660308d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089541"
---
# <a name="about-on-premises-disaster-recovery-failoverfailback"></a>Şirket içi olağanüstü durum kurtarma yük devretme/yeniden çalışma hakkında

Bu makalede, [Azure Site Recovery](site-recovery-overview.md)ile şirket Içi makinelerin Azure 'a olağanüstü durum kurtarması sırasında yük devretme ve yeniden çalışma için bir genel bakış sunulmaktadır.

## <a name="recovery-stages"></a>Kurtarma aşamaları

Site Recovery yük devretme ve yeniden çalışma dört aşamaya sahiptir:

- **1. Aşama:** şirket içi makineler için Azure 'a çoğaltma ayarladıktan sonra şirket içi siteniz oturum açtıktan sonra bu makinelerinizden Azure 'a yük devredebilmeniz gerekir. Yük devretmeden sonra Azure VM 'Leri çoğaltılan verilerden oluşturulur.
- **2. Aşama: Azure VM 'Lerini yeniden koruma**: Azure 'da, Azure VM 'leri, şirket içi siteye tekrar çoğaltmaya başlayacak şekilde yeniden korumalısınız. Veri tutarlılığı sağlamaya yardımcı olmak için, yeniden koruma sırasında şirket içi VM (varsa) kapalıdır.
- **3. Aşama: Azure 'Da yük devretme**: şirket içi siteniz tekrar normal şekilde çalıştığında, Azure VM 'lerinizi şirket içi sitenize geri yüklemek için başka bir yük devretme işlemi çalıştırırsınız. Yük devretmesini veya alternatif bir konuma geri dönebilirsiniz.
- **4. Aşama: şirket içi makineleri yeniden koruma**: geri alındıktan sonra, şirket Içi makinelerin Azure 'a çoğaltılmasını yeniden etkinleştirin.

## <a name="failover"></a>Yük devretme

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinizin bir parçası olarak bir yük devretme gerçekleştirirsiniz.

- BCDR stratejinizde ilk adım olarak, şirket içi makinelerinizi sürekli olarak Azure 'a çoğaltmanız gerekir. Kullanıcılar, şirket içi kaynak makinelerde çalışan iş yüklerine ve uygulamalarına erişir.
- Gerekirse, örneğin şirket içi bir kesinti varsa, makinelerin Azure 'a çoğaltılmasını sağlayabilirsiniz. Azure VM 'Ler çoğaltılan veriler kullanılarak oluşturulur.
- İş sürekliliği için, kullanıcılar Azure VM 'lerinde uygulamalara erişmeye devam edebilir.

Yük devretme iki aşamalı bir etkinliktir:

- **Yük devretme**: Seçilen kurtarma noktasını kullanarak bir Azure VM 'yi oluşturan ve getiren yük devretme.
- **Yürüt**: yük devretmenin ardından Azure 'da VM 'yi doğrularsınız:
    - Daha sonra yük devretmeyi seçili kurtarma noktasına kaydedebilir veya kaydetme için farklı bir nokta seçebilirsiniz.
    - Yük devretme işlemi tamamlandıktan sonra, kurtarma noktası değiştirilemez.


## <a name="connect-to-azure-after-failover"></a>Yük devretmeden sonra Azure 'a Bağlan

RDP/SSH kullanılarak yük devretmeden sonra oluşturulan Azure VM 'lerine bağlanmak için bazı gereksinimler vardır.

**Yükünü** | **Konum** | **Eylemler**
--- | --- | ---
**Azure VM (Windows (** | Yük devretmeden önce şirket içi makinede | **İnternet üzerinden erişim**: RDP 'yi etkinleştirin. TCP ve UDP kurallarının **genel**için eklendiğinden ve **Windows Güvenlik Duvarı**  >  **izin verilen uygulamalar**'daki tüm profiller için RDP 'ye izin verildiğinden emin olun.<br/><br/> **Siteden sıteye VPN üzerinden erişim**: makinede RDP 'yi etkinleştirin. **Windows Firewall**  ->  **Etki alanı ve özel** ağlar için Windows Güvenlik Duvarı**izin verilen uygulamalar ve Özellikler**'de RDP 'ye izin verildiğinden emin olun.<br/><br/>  İşletim sistemi SAN ilkesinin **OnlineAll**olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).<br/><br/> Yük devretme tetiklemeniz sırasında VM 'de bekleyen bir Windows güncelleştirmesi olmadığından emin olun. Yük devretmek Windows Update başlayabilir ve güncelleştirmeler tamamlanana kadar VM 'de oturum açamazsınız.
**Windows çalıştıran Azure VM** | Yük devretmeden sonra Azure VM 'de |  VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> Yük devredilen VM 'deki (ve bağlı olduğu Azure alt ağı) ağ güvenlik grubu kuralları, RDP bağlantı noktasına gelen bağlantılara izin vermelidir.<br/><br/> VM 'nin ekran görüntüsünü doğrulamak için **önyükleme tanılamalarını** denetleyin. Bağlanamıyorsanız, sanal makinenin çalışıp çalışmadığını denetleyin ve [sorun giderme ipuçları](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx)' nı gözden geçirin.
**Linux çalıştıran Azure VM** | Yük devretmeden önce şirket içi makinede | VM 'deki Secure Shell hizmetinin sistem önyüklemesi üzerinde otomatik olarak başlayacak şekilde ayarlandığından emin olun.<br/><br/> Güvenlik duvarı kurallarının gerçekleştirilecek SSH bağlantısına izin verdiğinden emin olun.
**Linux çalıştıran Azure VM** | Yük devretmeden sonra Azure VM 'de | Yük devredilen VM 'deki (ve bağlı olduğu Azure alt ağı) ağ güvenlik grubu kurallarının SSH bağlantı noktasına gelen bağlantılara izin vermeniz gerekir.<br/><br/> VM için bir [ortak IP adresi ekleyin](https://aka.ms/addpublicip).<br/><br/> VM 'nin ekran görüntüsü için **önyükleme tanılamayı** denetleyin.<br/><br/>

## <a name="types-of-failover"></a>Yük devretme türleri

Site Recovery, farklı yük devretme seçenekleri sağlar.

**Yükünü** | **Ayrıntılar** | **Kurtarma** | **Workflow**
--- | --- | --- | ---
**Yük devretme testi** | Veri kaybı veya kapalı kalma süresi olmadan BCDR stratejinizi doğrulayan bir detaya gitme çalıştırmak için kullanılır.| Azure 'da, devam eden çoğaltma üzerinde veya üretim ortamınızda bir etkisi olmadan VM 'nin bir kopyasını oluşturur. | 1. bir kurtarma planında tek bir VM 'de veya birden çok VM 'de yük devretme testi çalıştırın.<br/><br/> 2. test yük devretmesi için kullanılacak bir kurtarma noktası seçin.<br/><br/> 3. Azure VM 'nin yük devretme sonrasında oluşturulduğu sırada bulunduğu bir Azure ağı seçin. Ağ yalnızca yük devretme testi için kullanılır.<br/><br/> 4. detayın beklendiği gibi çalıştığını doğrulayın. Site Recovery, detaya gitme sırasında Azure 'da oluşturulan VM 'Leri otomatik olarak temizler.
**Planlı Yük devretme-Hyper-V**  | Genellikle planlanan kapalı kalma süresi için kullanılır.<br/><br/> Kaynak VM 'Ler kapatılıyor. En son veriler, yük devretme başlatılmadan önce eşitlenir. | Planlı iş akışı için sıfır veri kaybı. | 1. kesinti süresi bakım penceresi planlayın ve kullanıcılara bildirin.<br/><br/> 2. kullanıcıya yönelik uygulamaları çevrimdışına alın.<br/><br/> 3. en son kurtarma noktasıyla planlanmış bir yük devretme başlatın. Makine kapanmazsa veya hatalarla karşılaşılırsa yük devretme çalışmaz.<br/><br/> 4. yük devretmeden sonra Azure 'da çoğaltma Azure VM 'sinin etkin olduğunu denetleyin.<br/><br/> 5. yük devretmeyi tamamlamak için yürütün. Kaydetme eylemi tüm kurtarma noktalarını siler.
**Yük devretme-Hyper-V** | Genellikle planlanmamış bir kesinti varsa veya birincil site kullanılamıyorsa çalışır.<br/><br/> İsteğe bağlı olarak VM 'yi kapatın ve son değişiklikleri, yük devretmeyi başlatmadan önce eşitler.  | Uygulamalar için en az veri kaybı. | 1. BCDR planınızı başlatın. <br/><br/> 2. yük devretme başlatın. Site Recovery VM 'nin kapatılıp kapatılmayacağını ve yük devretmeyi tetiklemeden önce en son değişiklikleri eşitlemesini/çoğaltmasını belirtin.<br/><br/> 3. aşağıdaki tabloda özetlenen çeşitli kurtarma noktası seçeneklerine yük devretmek için yük devretme yapabilirsiniz.<br/><br/> VM 'yi kapatma seçeneğini etkinleştirmezseniz veya Site Recovery kapatamaz, en son kurtarma noktası kullanılır.<br/>Makine kapatılmasa bile yük devretme çalışır.<br/><br/> 4. yük devretmeden sonra Azure 'da çoğaltma Azure VM 'sinin etkin olduğunu kontrol edersiniz.<br/> Gerekirse, bekletme penceresinde 24 saat olan farklı bir kurtarma noktası seçebilirsiniz.<br/><br/> 5. yük devretmeyi tamamlamak için yürütün. Kaydetme eylemi, kullanılabilir tüm kurtarma noktalarını siler.
**Yük devretme-VMware** | Genellikle planlanmamış bir kesinti varsa veya birincil site kullanılamıyorsa çalışır.<br/><br/> İsteğe bağlı olarak, Site Recovery VM 'nin kapatılmasını tetiklemeyi ve yük devretmeyi başlatmadan önce son değişiklikleri eşitleyeceğini ve çoğaltmayı denemelidir.  | Uygulamalar için en az veri kaybı. | 1. BCDR planınızı başlatın. <br/><br/> 2. Site Recovery bir yük devretme başlatın. Yük devretmeyi çalıştırmadan önce Site Recovery VM kapatılmasını ve eşitlemesini tetiklemeyi deneyip denemeyeceğini belirtin.<br/> Makineler kapatılmasa bile yük devretme çalışır.<br/><br/> 3. yük devretmeden sonra Azure 'da çoğaltma Azure VM 'sinin etkin olduğunu kontrol edin. <br/>Gerekirse, 72 saatlik bekletme penceresinde farklı bir kurtarma noktası seçebilirsiniz.<br/><br/> 5. yük devretmeyi tamamlamak için yürütün. Kaydetme eylemi tüm kurtarma noktalarını siler.<br/> Windows VM 'Leri için, Site Recovery yük devretme sırasında VMware araçlarını devre dışı bırakır.

## <a name="failover-processing"></a>Yük devretme işleme

Bazı senaryolarda, yük devretme işleminin tamamlanabilmesi için 8 ila 10 dakika geçen ek işleme gerekir. İçin yük devretme sürelerinin daha uzun süre test edebilirsiniz:

* 9,8 'den eski bir Mobility hizmeti sürümü çalıştıran VMware VM 'Leri.
* Fiziksel sunucular.
* VMware Linux VM 'Leri.
* Fiziksel sunucu olarak korunan Hyper-V VM 'Leri.
* DHCP hizmeti etkinleştirilmemiş VMware VM 'Leri.
* Şu önyükleme sürücülerine sahip olmayan VMware VM 'Leri: storvsc, VMBus, storflt, intelide, Atapi.

## <a name="recovery-point-options"></a>Kurtarma noktası seçenekleri

Yük devretme sırasında, bir dizi kurtarma noktası seçeneği belirleyebilirsiniz.

**Seçenek** | **Ayrıntılar**
--- | ---
**En son (en düşük RPO)** | Bu seçenek en düşük kurtarma noktası hedefini (RPO) sağlar. İlk olarak, Site Recovery hizmetine gönderilen tüm verileri, yük devretmadan önce her VM için bir kurtarma noktası oluşturmak üzere işler. Bu kurtarma noktası, yük devretme tetiklendiğinde Site Recovery tüm verilerin çoğaltılmasını sağlar.
**En son işlenen**  | Bu seçenek, Site Recovery tarafından işlenen en son kurtarma noktasına VM 'Leri devreder. Belirli bir sanal makine için en son kurtarma noktasını görmek üzere VM ayarlarındaki **en son kurtarma noktalarını** kontrol edin. İşlenmemiş verileri işlemek için zaman harcanmadığından bu seçenekte düşük bir RTO (Kurtarma Süresi Hedefi) sağlanır.
**En son uygulamayla tutarlı** |  Bu seçenek, uygulama ile tutarlı kurtarma noktaları etkinse, Site Recovery tarafından işlenen en son uygulamayla tutarlı kurtarma noktasına VM 'Leri devreder. VM ayarlarındaki en son kurtarma noktasını denetleyin.
**İşlenen en son çoklu VM** | Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM 'ye sahip kurtarma planlarında kullanılabilir. Ayarı etkin olan VM 'Ler, en son ortak çoklu VM tutarlı kurtarma noktasına yük devreder. Plandaki diğer VM 'Ler, işlenen en son kurtarma noktasına yük devreder.
**En son çoklu VM uygulaması-tutarlı** |  Bu seçenek, çoklu VM tutarlılığı etkinleştirilmiş bir veya daha fazla VM 'ye sahip kurtarma planlarında kullanılabilir. Bir çoğaltma grubunun parçası olan VM 'Ler, en son ortak çoklu VM uygulamayla tutarlı kurtarma noktasına yük devreder. Diğer VM 'Ler, uygulamayla tutarlı en son kurtarma noktasına yük devreder.
**Özel** | Belirli bir VM 'nin yükünü zaman içinde belirli bir kurtarma noktasına devretmek için bu seçeneği kullanın. Bu seçenek, kurtarma planları için kullanılamaz.

> [!NOTE]
> Kurtarma noktaları başka bir kurtarma hizmetleri kasasına geçirilemez.

## <a name="reprotectionfailback"></a>Yeniden koruma/yeniden çalışma

Azure 'a yük devretmenin ardından, çoğaltılan Azure VM 'Leri korumasız bir durumdaydı.

- Şirket içi sitenize geri dönmek için ilk bir adım olarak, şirket içi siteye çoğaltılan Azure VM 'lerini başlatmanız gerekir. Yeniden koruma işlemi, yük devredilen makinelerin türüne bağlıdır.
- Makineler Azure 'dan şirket içine çoğaltıldıktan sonra, Azure 'dan şirket içi sitenize bir yük devretme işlemi çalıştırabilirsiniz.
- Makineler şirket içinde yeniden çalıştıktan sonra, olağanüstü durum kurtarma için çoğaltmayı Azure 'a çoğaltacak şekilde etkinleştirebilirsiniz.

Yeniden çalışma aşağıdaki gibi çalışır:

- Yeniden yük devretmek için, bir VM 'nin yeniden başlatılması için en az bir kurtarma noktası olması gerekir. Bir kurtarma planında, plandaki tüm VM 'Lerin en az bir kurtarma noktası olması gerekir.
- Geri dönmesi için **en son** kurtarma noktasını kullanmanızı öneririz (Bu kilitlenme ile tutarlı bir noktasıdır).
    - Uygulamayla tutarlı bir kurtarma noktası seçeneği vardır. Bu durumda, tek bir VM, uygulamayla tutarlı en son kurtarma noktasını kurtarır. Çoğaltma grubu olan bir kurtarma planı için, her bir çoğaltma grubu ortak kullanılabilir kurtarma noktasına kurtarır.
    - Uygulamayla tutarlı kurtarma noktaları zaman içinde kalabilir ve verilerde kayıp olabilir.
- Azure 'dan şirket içi siteye yük devretme sırasında Site Recovery Azure VM 'lerini kapatır. Yük devretmeyi kaydettiğinizde, Site Recovery Azure 'daki başarısız geri Azure VM 'lerini kaldırır.


## <a name="vmwarephysical-reprotectionfailback"></a>VMware/fiziksel yeniden koruma/yeniden çalışma

VMware makinelerini ve fiziksel sunucuları Azure 'dan şirket içine yeniden korumak ve geri yüklemek için bir yeniden çalışma altyapısına ihtiyacınız vardır ve bazı gereksinimler vardır.

- **Azure 'Da geçici işlem sunucusu**: Azure 'dan yeniden yük devretmek Için BIR Azure VM 'yi, Azure 'dan çoğaltmayı işleyecek bir işlem sunucusu olarak davranacak şekilde ayarlarsınız. Yeniden çalışma sona erdikten sonra bu VM'yi silebilirsiniz.
- **VPN bağlantısı**: geri dönmek Için, Azure ağından şirket içi sıteye bir VPN bağlantısına (veya ExpressRoute) sahip olmanız gerekir.
- **Ayrı ana hedef sunucusu**: varsayılan olarak, şirket ıçı VMware VM 'de yapılandırma sunucusu ile yüklenen ana hedef sunucu yeniden çalışmayı işler. Büyük hacimlerde trafik geri yüklemeniz gerekiyorsa, bu amaçla ayrı bir şirket içi ana hedef sunucusu ayarlayın.
- **Yeniden çalışma ilkesi**: Şirket içi sitenize geri çoğaltmak için bir yeniden çalışma ilkeniz olmalıdır. Bu ilke, Şirket içinden Azure 'a bir çoğaltma ilkesi oluşturduğunuzda otomatik olarak oluşturulur.
    - Bu ilke yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.
    - Bu ilkeyi düzenleyemezsiniz.
    - İlke değerleri: RPO eşiği-15 dakika; Kurtarma noktası bekletme-24 saat; Uygulamayla tutarlı anlık görüntü sıklığı-60 dakika.

VMware/fiziksel koruma ve yeniden çalışma hakkında daha fazla bilgi edinin:
- Yeniden koruma ve yeniden çalışma için ek gereksinimleri [gözden geçirin](vmware-azure-reprotect.md#before-you-begin) .
- Azure 'da bir işlem sunucusu [dağıtın](vmware-azure-prepare-failback.md#deploy-a-process-server-in-azure) .
- Ayrı bir ana hedef sunucu [dağıtın](vmware-azure-prepare-failback.md#deploy-a-separate-master-target-server) .

Azure VM 'lerini şirket içi olarak yeniden koruduğunuzda, özgün konuma veya alternatif bir konuma yeniden yük devretmek istediğinizi belirtebilirsiniz.

- **Özgün konum kurtarma**: varsa, Azure 'dan aynı kaynak şirket içi makineye geri dönme işlemi yapılır. Bu senaryoda, yalnızca değişiklikler şirket içi olarak geri çoğaltılır.
- **Alternatif konum kurtarma**: şirket içi makine yoksa, Azure 'dan alternatif bir konuma yeniden yük devreedebilirsiniz. Azure VM 'yi şirket içi olarak yeniden koruduğunuzda şirket içi makine oluşturulur. Azure 'dan şirket içine tam veri çoğaltma işlemi gerçekleşir. --Konum yeniden çalışma için gereksinimleri ve sınırlamaları [gözden geçirin](concepts-types-of-failback.md) .



## <a name="hyper-v-reprotectionfailback"></a>Hyper-V yeniden koruma/yeniden çalışma

Hyper-V VM 'lerini Azure 'dan şirket içine yeniden korumak ve geri yüklemek için:

- Yalnızca bir depolama hesabı kullanarak çoğaltılan Hyper-V VM 'lerinin geri dönmesini sağlayabilirsiniz. Yönetilen diskler kullanılarak çoğaltılan Hyper-V VM 'lerinin yeniden çalışması desteklenmez.
- Şirket içi Hyper-V Konakları (veya kullanılıyorsa System Center VMM) Azure 'a bağlanmalıdır.
- Azure 'dan şirket içine planlı bir yeniden çalışma çalıştırırsınız.
- Hyper-V VM yeniden çalışma için ayarlanması gereken belirli bir bileşen yok.
- Planlı Yük devretme sırasında, yeniden çalışma öncesinde verileri eşitlemeye yönelik seçenekleri belirleyebilirsiniz:
    - **Yük devretmeden önce verileri eşitleme**: Bu seçenek, makineleri kapatmadan makineyi eşitlerken sanal makineler için kapalı kalma süresini en aza indirir.
        - 1. Aşama: Azure VM 'nin bir anlık görüntüsünü alır ve şirket içi Hyper-V konağına kopyalar. Makine Azure 'da çalışmaya devam ediyor.
        - 2. Aşama: hiçbir yeni değişiklik gerçekleşmemesi için Azure VM 'yi kapatır. Son Delta değişikliği kümesi şirket içi sunucuya aktarılır ve şirket içi VM başlatılır.
    - **Yalnızca yük devretme sırasında verileri eşitler**: Bu seçenek daha hızlıdır çünkü diskin çoğunu değiştirmiş ve bu nedenle sağlama toplamı hesaplamaları gerçekleştirmemelidir. Diskin indirilmesini gerçekleştirir. VM, Azure 'da bir süredir (bir ay veya daha fazla) çalışıyorsa veya şirket içi VM silinmişse bu seçeneği kullanmanızı öneririz.

Hyper-V yeniden koruma ve yeniden çalışma hakkında [daha fazla bilgi edinin](hyper-v-azure-failback.md) .

Azure VM 'lerini şirket içi olarak yeniden koruduğunuzda, özgün konuma veya alternatif bir konuma yeniden yük devretmek istediğinizi belirtebilirsiniz.

- **Özgün konum kurtarma**: varsa, Azure 'dan aynı kaynak şirket içi makineye geri dönme işlemi yapılır. Bu senaryoda, önceki yordamda açıklanan eşitleme seçeneklerinden birini seçersiniz.
- **Alternatif konum kurtarma**: şirket içi makine yoksa, Azure 'dan alternatif bir konuma yeniden yük devreedebilirsiniz. Azure VM 'yi şirket içi olarak yeniden koruduğunuzda şirket içi makine oluşturulur. Bu seçenekle, yük devretmeden önce verileri eşitlemeye yönelik seçeneği seçmenizi öneririz
- Konum yeniden çalışma için gereksinimleri ve sınırlamaları [gözden geçirin](hyper-v-azure-failback.md) .


Şirket içi siteye geri yüklendikten sonra, çoğaltmayı tamamlayarak çoğaltmayı tamamlayarak VM 'yi Azure 'a Çoğaltmaya başlamak için **ters çoğaltma** 'yı etkinleştirin.




## <a name="next-steps"></a>Sonraki adımlar
- [Belirli VMware VM](vmware-azure-tutorial-failover-failback.md) 'lerinin yükünü devret
- [Belirli Hyper-V VM](hyper-v-azure-failover-failback-tutorial.md)'lerinin yükünü devreder.
- Kurtarma planı [oluşturun](site-recovery-create-recovery-plans.md) .
- [Kurtarma planındaki VM 'lerin](site-recovery-failover.md)yükünü devreder.
- [Hazırlanma](vmware-azure-failback.md) VMware yeniden koruma ve yeniden çalışma.
- [Hyper-V VM](hyper-v-azure-failback.md)'lerini yeniden devretmek.

