---
title: Azure geçiş 'de Hyper-V değerlendirmesi desteği
description: Azure geçişi ile Hyper-V değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 9c1228992d71e56b9118e88967478e619c14959a
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79245817"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Hyper-V değerlendirmesi için destek matrisi

Bu makalede, Hyper-V VM 'lerini [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) ile değerlendirmek için destek ayarları ve sınırlamaları özetlenmektedir. Hyper-V VM 'lerini Azure 'a geçirme hakkında bilgi arıyorsanız, [geçiş desteği matrisini](migrate-support-matrix-hyper-v-migration.md)gözden geçirin.

## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projedeki](migrate-support-matrix.md#azure-migrate-projects)35.000 adede kadar Hyper-V VM 'yi bulur ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Keşfini** | Azure geçişi gereci en fazla 5000 Hyper-V VM 'yi bulabilir.<br/><br/> Gereç, 300 adede kadar Hyper-V konaklarına bağlanabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede 35.000 adede kadar VM 'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .



## <a name="hyper-v-host-requirements"></a>Hyper-V konağı gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Konak dağıtımı**       | Hyper-V konağı tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | Hyper-V konağında yönetici izinlerine sahip olmanız gerekir. <br/> Alternatif olarak, yönetici izinleri atamak istemiyorsanız, bir yerel veya etki alanı kullanıcı hesabı oluşturun ve kullanıcıyı bu gruplara (uzak yönetim kullanıcıları, Hyper-V yöneticileri ve performans Izleyicisi kullanıcıları) ekleyin. |
| **Konak işletim sistemi** | Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2.<br/> Windows Server 2012 çalıştıran Hyper-V konaklarında yer alan VM'leri değerlendiremezsiniz. |
| **PowerShell uzaktan Iletişim**   | Her konakta etkin olmalıdır. |
| **Hyper-V çoğaltma**       | Hyper-V çoğaltması kullanıyorsanız (veya aynı VM tanımlayıcılarına sahip birden fazla sanal makine varsa) Azure geçişi 'ni kullanarak hem özgün hem de çoğaltılan VM 'Leri keşfetiyorsanız, Azure geçişi tarafından oluşturulan değerlendirme doğru olmayabilir. |


## <a name="hyper-v-vm-requirements"></a>Hyper-V VM gereksinimleri

| **Destek**                  | **Ayrıntılar**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **Tümleştirme Hizmetleri**       | [Hyper-V tümleştirme hizmetlerinin](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) , işletim sistemi bilgilerini yakalamak için değerlendirmekte olduğunuz VM 'lerde çalışıyor olması gerekir. |


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Hyper-V için gereç bir Hyper-V VM üzerinde çalışır ve Azure portal indirdiğinizde, sıkıştırılmış bir Hyper-V VHD kullanılarak dağıtılır. 

- Hyper-V için [gereç gereksinimleri](migrate-appliance.md#appliance---hyper-v) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**Aygıtların** | **Bağlanma**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ``` https://<appliance-ip-or-name>:44368 ```<br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS), 5671 ve 5672 (AMQP) bağlantı noktalarında giden bağlantılar.
**Hyper-V konağı/kümesi** | Genel Bilgi Modeli (CıM) oturumu kullanarak Hyper-V VM 'lerinin yapılandırma ve performans meta verilerini çekmek için WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar.

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


## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V VM değerlendirmesi için hazırlanma](tutorial-prepare-hyper-v.md)
