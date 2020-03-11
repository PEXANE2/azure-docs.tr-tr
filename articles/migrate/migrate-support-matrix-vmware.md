---
title: Azure geçişi 'nde VMware değerlendirmesi desteği
description: Azure geçişi 'nde VMware değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: b887508fb8e422bd83aa9d13e42085d7a6bd2283
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79080402"
---
# <a name="support-matrix-for-vmware-assessment"></a>VMware değerlendirmesi için destek matrisi 

Bu makalede, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-migration-tool)Ile VMware VM 'lerini değerlendirmek için destek ayarları ve sınırlamaları özetlenmektedir. VMware VM 'lerini Azure 'a geçirme hakkında bilgi arıyorsanız, [geçiş desteği matrisini](migrate-support-matrix-vmware-migration.md)gözden geçirin.

## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 VMware VM bulun ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Keşfini** | Azure geçişi gereci vCenter Server en fazla 10.000 VMware VM 'yi bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .


## <a name="application-discovery"></a>Uygulama bulma

Azure geçişi: Sunucu değerlendirmesi, makineleri keşfetmenin yanı sıra makinelerde çalışan uygulamalar, rol ve özellikleri bulabilir. Uygulama envanterinizi keşfetmek, şirket içi iş yükleriniz için uyarlanmış bir geçiş yolu tanımlamanızı ve planlamanızı sağlar. 

**Destek** | **Ayrıntılar**
--- | ---
**Keşfini** | Bulgu aracısız, makine konuk kimlik bilgileri kullanılarak ve WMI ve SSH çağrıları kullanılarak makinelere uzaktan erişim.
**Desteklenen makineler** | Şirket içi VMware VM 'Leri.
**Makine işletim sistemi** | Tüm Windows ve Linux sürümleri.
**vCenter kimlik bilgileri** | Salt okuma erişimi olan ve konuk Işlemleri > sanal makineler için etkinleştirilen ayrıcalıkların vCenter Server bir hesabı.
**VM kimlik bilgileri** | Şu anda tüm Windows sunucuları için bir kimlik bilgisi ve tüm Linux sunucuları için bir kimlik bilgisi kullanımını desteklemektedir.<br/><br/> Windows VM 'ler için bir Konuk Kullanıcı hesabı ve tüm Linux sanal makineleri için normal/normal kullanıcı hesabı (sudo erişimi olmayan) oluşturursunuz.
**VMware araçları** | Keşif yapmak istediğiniz VM 'lerde VMware araçları yüklü ve çalışır olmalıdır. <br/> VMware araçları sürümünüz 9,10-10.2.0 arasındaysa, lütfen 10.2.0 'in ötesine yükseltdiğinizden emin olun.
**PowerShell** | VM 'Ler sürüm 2,0 veya üzeri PowerShell 'i içermelidir
**Bağlantı noktası erişimi** | Bulmayı istediğiniz VM 'Leri çalıştıran ESXi konağında, Azure geçiş gereci 443 numaralı TCP bağlantı noktasına bağlanabilmelidir.
**Limitler** | Uygulama bulma için, her gereç için 10000 'e kadar keşfedebilirsiniz. 

## <a name="vmware-requirements"></a>VMware gereksinimleri

**VMware** | **Ayrıntılar**
--- | ---
**vCenter Server** | Bulmak ve değerlendirmek istediğiniz makineler vCenter Server sürüm 5,5, 6,0, 6,5 veya 6,7 tarafından yönetilmelidir.
**İzinler (değerlendirme)** | salt vCenter Server hesabı.
**İzinler (App-Discovery)** | salt okuma erişimi olan ve **Konuk işlemleri > sanal makineler**için etkinleştirilen ayrıcalıkların vCenter Server hesabı.
**İzinler (bağımlılık görselleştirme)** | Salt okuma erişimine sahip merkezi sunucu hesabı ve **sanal makineler** için etkinleştirilen ayrıcalıklar > **Konuk işlemleri**.


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. VMware için gereç, vCenter Server içeri aktarılan bir OVA şablonu kullanılarak dağıtılır. 

- VMware için [gereç gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

**Aygıtların** | **Bağlanma**
--- | ---
Elektrikli | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/><br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ```https://<appliance-ip-or-name>:44368``` <br/><br/>Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS), 5671 ve 5672 (AMQP) numaralı bağlantı noktası üzerinden giden bağlantılar.
vCenter server | TCP bağlantı noktası 443 ' deki gelen bağlantılar, gerecin değerlendirmeler için yapılandırma ve performans meta verilerini toplamasına izin verir. <br/><br/> Gereç, bağlantı noktası 443 ' de varsayılan olarak vCenter 'a bağlanır. VCenter sunucusu farklı bir bağlantı noktasını dinliyorsa, bulmayı ayarlarken bağlantı noktasını değiştirebilirsiniz.
ESXi Konakları | **Yalnızca [uygulama bulma](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#application-discovery) ve [aracısız bağımlılık görselleştirmesi](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-dependency-visualization) için gereklidir** <br/><br/> Gereç, uygulamalar bulmaya ve konaklarda çalışan VM 'lerde aracısız bağımlılık görselleştirmesini çalıştırmaya yönelik TCP bağlantı noktası 443 üzerindeki ESXi konaklarına bağlanır.

## <a name="agent-based-dependency-visualization"></a>Aracı tabanlı bağımlılık görselleştirmesi

[Bağımlılık görselleştirme](concepts-dependency-visualization.md) , değerlendirmek ve geçirmek istediğiniz makineler arasında bağımlılıkları görselleştirmenize yardımcı olur. Aracı tabanlı görselleştirme için, gereksinimler ve sınırlamalar aşağıdaki tabloda özetlenmiştir


**Gereksinim** | **Ayrıntılar**
--- | ---
**Dağıtım** | Bağımlılık görselleştirmesini dağıtmadan önce, Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesine sahip olmanız gerekir. Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız.<br/><br/> Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.
**Hizmet Eşlemesi** | Aracı tabanlı bağımlılık görselleştirmesi [Azure izleyici günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [hizmet eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır.<br/><br/> Dağıtımı yapmak için, yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure geçişi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma alanının [hizmet eşlemesi desteklendiği](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites)bir bölgede olması gerekir.<br/><br/> Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez.
**Ücretleriyle** | Hizmet Eşlemesi çözümü, ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirmenizin bulunduğu günden).<br/><br/> 180 gün sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması standart Log Analytics ücretlendirmeye tabi olacaktır.<br/><br/> Azure geçişi projesini silerseniz, çalışma alanı onunla silinmez. Projeyi sildikten sonra Hizmet Eşlemesi ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir.
**Aracısını** | Aracı tabanlı bağımlılık görselleştirmesi, çözümlemek istediğiniz her makineye iki aracı yüklenmesini gerektirir.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [bağımlılık Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**İnternet bağlantısı** | Makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidini yüklemeniz gerekir.


## <a name="agentless-dependency-visualization"></a>Aracısız bağımlılık görselleştirme

Bu seçenek şu anda önizleme aşamasındadır. [Daha fazla bilgi edinin](how-to-create-group-machine-dependencies-agentless.md). Gereksinimler aşağıdaki tabloda özetlenmiştir.

**Gereksinim** | **Ayrıntılar**
--- | ---
**Dağıtım** | Bağımlılık görselleştirmesini dağıtmadan önce, Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesine sahip olmanız gerekir. Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız.
**VM desteği** | Şu anda yalnızca VMware VM 'Leri için destekleniyor.
**Windows VM’leri** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64-bit)
**Linux VM'leri** | Red Hat Enterprise Linux 7, 6, 5<br/> Ubuntu Linux 14,04, 16,04<br/> Desek6, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.
**Windows hesabı** |  Görselleştirmenin yerel veya etki alanı yöneticisi hesabı olması gerekir.
**Linux hesabı** | Görselleştirmenin kök ayrıcalığına sahip bir kullanıcı hesabı olması gerekir.<br/><br/> Alternatif olarak, Kullanıcı hesabının/bin/netstat ve/bin/ls dosyalarında bu izinlere ihtiyacı vardır: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.
**VM aracıları** | VM 'lerde gerekli bir aracı yok.
**VMware araçları** | VMware araçlarının, çözümlemek istediğiniz VM 'lerde yüklü ve çalışıyor olması gerekir. <br/> VMware araçları sürümünüz 9,10-10.2.0 arasındaysa, lütfen 10.2.0 'in ötesine yükseltdiğinizden emin olun.
**PowerShell** | VM 'Ler sürüm 2,0 veya üzeri PowerShell 'i içermelidir
**vCenter kimlik bilgileri** | Salt okuma erişimi olan ve konuk Işlemleri > sanal makineler için etkinleştirilen ayrıcalıkların vCenter Server bir hesabı.
**Bağlantı noktası erişimi** | Çözümlemek istediğiniz VM 'Leri çalıştıran ESXi konağında, Azure geçiş gereci 443 numaralı TCP bağlantı noktasına bağlanabilmelidir.



## <a name="next-steps"></a>Sonraki adımlar

- Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin](best-practices-assessment.md) .
- [VMware değerlendirmesi Için hazırlanma](tutorial-prepare-vmware.md) .
