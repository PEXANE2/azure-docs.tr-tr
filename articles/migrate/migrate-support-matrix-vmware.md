---
title: Azure geçişi 'nde VMware değerlendirmesi desteği
description: Azure geçişi sunucu değerlendirmesi ile VMware VM değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 11/10/2020
ms.openlocfilehash: 6e033bdf0f1492d6cbb4c41192cca8206816917d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94444954"
---
# <a name="support-matrix-for-vmware-assessment"></a>VMware değerlendirmesi için destek matrisi 

Bu makalede, Azure geçişi [: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak Azure 'a geçiş Için VMware VM 'lerini bulup değerlendirdikten sonra Önkoşullar ve destek gereksinimleri özetlenmektedir. VMware VM 'lerini değerlendirmek için bir Azure geçişi projesi oluşturun ve sunucu değerlendirme aracını projeye ekleyin. Araç eklendikten sonra Azure geçişi gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve makine meta verilerini ve performans verilerini Azure 'a gönderir. Bulma işlemi tamamlandıktan sonra, bulunan makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız. 

VMware VM 'lerini Azure 'a geçirmek istiyorsanız, [geçiş desteği matrisini](migrate-support-matrix-vmware-migration.md)gözden geçirin.



## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz.<br/><br/> Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 VMware VM 'yi bulabilir ve değerlendirebilirsiniz. Bir proje, fiziksel sunucular ve Hyper-V VM 'lerini her biri için değerlendirme sınırlarına kadar de içerebilir.
**Bulma** | Azure geçişi gereci vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .


## <a name="vmware-requirements"></a>VMware gereksinimleri

**VMware** | **Ayrıntılar**
--- | ---
**vCenter Server** | Keşfetmek ve değerlendirmek istediğiniz makineler vCenter Server sürüm 5,5, 6,0, 6,5, 6,7 veya 7,0 tarafından yönetilmelidir.<br/><br/> Gereç içinde ESXi ana bilgisayar ayrıntıları sağlayarak VMware VM 'lerinin bulunması Şu anda desteklenmiyor.
**İzinler** | Sunucu değerlendirmesi bulma ve değerlendirme için vCenter Server salt okuma hesabına ihtiyaç duyuyor.<br/><br/> Uygulama bulma veya bağımlılık görselleştirmesi yapmak istiyorsanız, **sanal makinelerin**  >  **Konuk işlemlerinde** ayrıcalıkların etkinleştirilmesi gerekir.

## <a name="vm-requirements"></a>VM gereksinimleri
**VMware** | **Ayrıntılar**
--- | ---
**VMware Sanal Makineleri** | Geçiş için tüm işletim sistemleri değerlendirilenebilir. 
**Depolama** | SCSI, IDE ve SATA tabanlı denetleyicilere bağlı diskler desteklenir.


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Gereci bir OVA şablonu kullanarak bir VMware VM 'si olarak dağıtabilir, vCenter Server içeri aktarılabilir veya bir [PowerShell betiği](deploy-appliance-script.md)kullanabilirsiniz.

- VMware için [gereç gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Azure Kamu 'da, [betiği kullanarak](deploy-appliance-script-government.md)gereci dağıtmanız gerekir.
- Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken URL 'leri gözden geçirin.


## <a name="port-access-requirements"></a>Bağlantı noktası erişim gereksinimleri

**Cihaz** | **Bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/><br/> 44368 numaralı bağlantı noktası üzerinden gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktası üzerinden giden bağlantılar.
**vCenter server** | TCP bağlantı noktası 443 ' deki gelen bağlantılar, gerecin değerlendirmeler için yapılandırma ve performans meta verilerini toplamasına izin verir. <br/><br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bulmayı ayarlarken bağlantı noktasını değiştirebilirsiniz.
**ESXi Konakları** | [Uygulama bulmayı](how-to-discover-applications.md)veya [aracısız bağımlılık analizini](concepts-dependency-visualization.md#agentless-analysis)yapmak istiyorsanız, Gereç, uygulamalar bulmak ve VM 'lerde aracısız bağımlılık GÖRSELLEŞTIRMESINI çalıştırmak için TCP bağlantı noktası 443 üzerindeki ESXi konaklarına bağlanır.

## <a name="application-discovery-requirements"></a>Uygulama bulma gereksinimleri

Sunucu değerlendirmesi, makineleri keşfetmenin yanı sıra makinelerde çalışan uygulamalar, roller ve Özellikler bulabilir. Uygulama envanterinizi keşfetmek, şirket içi iş yükleriniz için uyarlanmış bir geçiş yolu tanımlamanızı ve planlamanızı sağlar. 

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen makineler** | Şu anda yalnızca VMware VM 'Leri için destekleniyor. Her bir Azure geçişi gerecinden en fazla 10000 VMware VM 'ye yüklenmiş uygulamaları bulabilirsiniz.
**İşletim sistemleri** | Tüm Windows ve Linux sürümlerini çalıştıran VM 'Ler için destek.
**VM gereksinimleri** | VMware araçlarının, uygulamaları keşfetmesini istediğiniz VM 'lerde yüklü ve çalışıyor olması gerekir. <br/><br/> VMware araçları sürümü 10.2.0 'dan daha sonra olmalıdır.<br/><br/> VM 'Lerin PowerShell sürüm 2,0 veya sonraki bir sürümü yüklü olmalıdır.
**Bulma** | Bir VM 'de yüklü uygulamalar hakkında bilgiler, VM 'de yüklü olan VMware araçları kullanılarak vCenter Server toplayın. Gereç, vSphere API 'Lerini kullanarak vCenter Server uygulama bilgilerini toplar. Uygulama bulma aracısız. Sanal makinelere hiçbir şey yüklenmez ve gereç doğrudan VM 'lere bağlanamaz. WMI/SSH, sanal makinelerde etkinleştirilmelidir ve kullanılabilir olmalıdır.
**vCenter** | Değerlendirme için kullanılan vCenter Server salt oku hesabı, **Virtual Machines**  >  uygulama bulma için VM ile etkileşime geçmek üzere sanal makineler **Konuk işlemleri** için etkinleştirilmiş ayrıcalıklara ihtiyaç duyuyor.
**VM erişimi** | Uygulama bulma 'nın uygulama bulma için VM 'de yerel bir kullanıcı hesabı olması gerekir.<br/><br/> Azure geçişi Şu anda tüm Windows sunucuları için bir kimlik bilgisi ve tüm Linux sunucuları için bir kimlik bilgisi kullanımını desteklemektedir.<br/><br/> Windows VM 'ler için bir Konuk Kullanıcı hesabı ve tüm Linux sanal makineleri için normal/normal kullanıcı hesabı (sudo erişimi olmayan) oluşturursunuz.
**Bağlantı noktası erişimi** | Azure geçişi gereci, uygulamaları bulmayı istediğiniz VM 'Leri çalıştıran ESXi konaklarında TCP bağlantı noktası 443 ' e bağlanabilmelidir. VCenter Server, uygulama bilgilerini içeren dosyayı indirmek için bir ESXI ana bilgisayar bağlantısı döndürür.



## <a name="dependency-analysis-requirements-agentless"></a>Bağımlılık Analizi gereksinimleri (aracısız)

[Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo aracısız bağımlılık analizini ayarlamaya yönelik gereksinimleri özetler.

**Gereksinim** | **Ayrıntılar**
--- | --- 
**Desteklenen makineler** | Şu anda yalnızca VMware VM 'Leri için destekleniyor.
**Windows VM'leri** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64 bit).<br/>Microsoft Windows Server 2008 (32-bit). 
**Linux VM'leri** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14,04, 16,04<br/> Desek6, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.<br/> SUSE Linux Enterprise Server 11 ve üzeri.
**VM gereksinimleri** | VMware araçları (10.2.0 ' den sonraki sürümler), çözümlemek istediğiniz VM 'lerde yüklü ve çalışır olmalıdır.<br/><br/> VM 'Lerin PowerShell sürüm 2,0 veya sonraki bir sürümü yüklü olmalıdır.
**Bulma yöntemi** |  VM 'Ler arasında bağımlılık bilgileri, sanal makinede yüklü olan VMware araçları kullanılarak vCenter Server toplanır. Gereç, vSphere API 'Lerini kullanarak vCenter Server bilgileri toplar. Bulma aracısız. VM 'ye hiçbir şey yüklenmez ve gereç doğrudan VM 'lere bağlanamaz. WMI/SSH, sanal makinelerde etkinleştirilmelidir ve kullanılabilir olmalıdır.
**vCenter hesabı** | Değerlendirme için Azure geçişi tarafından kullanılan salt tanımlı hesabın, **sanal makineler > Konuk işlemleri** için etkinleştirilmiş ayrıcalıkları vardır.
**Windows VM izinleri** |  VM 'lerde yerel yönetici izinlerine sahip bir hesap (yerel yönetici veya etki alanı).
**Linux hesabı** | Kök kullanıcı hesabı veya/bin/netstat ve/bin/ls dosyalarında şu izinlere sahip bir hesap: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.<br/><br/> Aşağıdaki komutları kullanarak bu özellikleri ayarlayın: <br/><br/> sudo setcap CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/bin/ls<br/><br/> sudo setcap CAP_DAC_READ_SEARCH, CAP_SYS_PTRACE = EP/bin/netstat
**Bağlantı noktası erişimi** | Azure geçişi gereci, bağımlılıklarını öğrenmek istediğiniz VM 'Leri çalıştıran ESXI konaklarında TCP bağlantı noktası 443 ' e bağlanabilmelidir. VCenter Server, bağımlılık bilgilerini içeren dosyayı indirmek için bir ESXI ana bilgisayar bağlantısı döndürür.


## <a name="dependency-analysis-requirements-agent-based"></a>Bağımlılık Analizi gereksinimleri (aracı tabanlı)

[Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracı tabanlı bağımlılık analizini ayarlamaya yönelik gereksinimleri özetler. 

**Gereksinim** | **Ayrıntılar** 
--- | --- 
**Dağıtımdan önce** | Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesi olması gerekir.<br/><br/>  Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız<br/><br/> İlk kez bir proje oluşturmayı [öğrenin](create-manage-projects.md) .<br/> Mevcut bir projeye değerlendirme aracı eklemeyi [öğrenin](how-to-assess.md) .<br/> [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)veya fiziksel sunucu değerlendirmesi için Azure geçişi gerecini ayarlamayı öğrenin.
**Desteklenen makineler** | Tüm makineler için desteklenir.
**Azure Devlet Kurumları** | Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.
**Log Analytics** | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../azure-monitor/log-query/log-query-overview.md) [hizmet eşlemesi](../azure-monitor/insights/service-map.md) çözümünü kullanır.<br/><br/> Yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz. Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde bulunmalıdır. Diğer bölgelerdeki çalışma alanları bir projeyle ilişkilendirilemez.<br/><br/> Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions)bir bölgede olması gerekir.<br/><br/> Log Analytics, Azure geçişi ile ilişkili çalışma alanı, geçiş projesi anahtarıyla ve proje adıyla etiketlenir.
**Gerekli aracılar** | Çözümlemek istediğiniz her makinede aşağıdaki aracıları yükleyebilirsiniz:<br/><br/> [Microsoft Monitoring Agent (MMA)](../azure-monitor/platform/agent-windows.md).<br/> [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Şirket içi makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidi indirip yüklemeniz gerekir.<br/><br/> [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) yükleme ve [MMA](how-to-create-group-machine-dependencies.md#install-the-mma)hakkında daha fazla bilgi edinin.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure geçişi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-configure-workspace.md#supported-regions)bir bölgede olması gerekir.<br/><br/> Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez.
**Maliyetler** | Hizmet Eşlemesi çözümü ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirdiğinizden itibaren)/<br/><br/> 180 günden sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması Log Analytics için [Standart ücretler](https://azure.microsoft.com/pricing/details/log-analytics/) doğurur.<br/><br/> Azure geçişi projesi silindiğinde, çalışma alanı onunla birlikte silinmez. Projeyi sildikten sonra Hizmet Eşlemesi kullanımı ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure genel kullanım (GA-28 Şubat 2018) geçirmeden önce oluşturduğunuz projeleriniz varsa, ek Hizmet Eşlemesi ücretleri tahakkuk etmeyebilirsiniz. Yalnızca 180 günden sonra ödemeyi sağlamak için, GA 'nin mevcut çalışma alanları Ücretlendirilebilir olmaya devam ettiğinden yeni bir proje oluşturmanızı öneririz.
**Yönetim** | Aracıları çalışma alanına kaydettiğinizde, Azure geçişi projesi tarafından sunulan KIMLIĞI ve anahtarı kullanırsınız.<br/><br/> Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz.<br/><br/> İlişkili Azure geçişi projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El Ile silin](../azure-monitor/platform/manage-access.md).<br/><br/> Azure geçişi projesini silmediğiniz takdirde Azure geçişi tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.
**İnternet bağlantısı** | Makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidini yüklemeniz gerekir.
**Azure Devlet Kurumları** | Aracı tabanlı bağımlılık Analizi desteklenmez.


## <a name="next-steps"></a>Sonraki adımlar

- Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin](best-practices-assessment.md) .
- [VMware değerlendirmesi Için hazırlanma](./tutorial-discover-vmware.md) .