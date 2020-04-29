---
title: Azure geçişi 'nde fiziksel sunucu değerlendirmesi desteği
description: Azure geçişi sunucu değerlendirmesi ile fiziksel sunucu değerlendirmesi desteği hakkında bilgi edinin
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: ae76a6b570ec58e71a8a1728a2a601728030f58c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81538163"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesi için destek matrisi 

Bu makalede, Azure geçişi [: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak fiziksel sunucuları Azure 'a geçiş için değerlendirmenize ilişkin Önkoşullar ve destek gereksinimleri özetlenmektedir. Fiziksel sunucuları Azure 'a geçirmek istiyorsanız, [geçiş desteği matrisini](migrate-support-matrix-physical-migration.md)gözden geçirin.


Fiziksel sunucuları değerlendirmek için bir Azure geçişi projesi oluşturun ve sunucu değerlendirme aracını projeye ekleyin. Araç eklendikten sonra [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve makine meta verilerini ve performans verilerini Azure 'a gönderir. Bulma işlemi tamamlandıktan sonra, bulunan makineleri gruplar halinde toplar ve bir grup için değerlendirme çalıştırırsınız.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Bilgileri**
--- | ---
**Değerlendirme limitleri** | Tek bir [Azure geçişi projesinde](migrate-support-matrix.md#azure-migrate-projects)35.000 fiziksel sunucuyu bulabilir ve değerlendirebilirsiniz.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Fiziksel sunuculara ek olarak bir proje, VMware VM 'leri ve Hyper-V VM 'lerini, her biri için değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci en fazla 250 fiziksel sunucu bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede en fazla 35.000 makine değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .

## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

| **Destek**                | **Bilgileri**               
| :-------------------       | :------------------- |
| **Fiziksel sunucu dağıtımı**       | Fiziksel sunucu tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | **Windows:** Keşif yapmak istediğiniz tüm Windows sunucularında yerel veya etki alanı kullanıcı hesabına ihtiyacınız vardır. Kullanıcı hesabı şu gruplara eklenmelidir: uzak masaüstü kullanıcıları, performans Izleyicisi kullanıcıları ve performans günlüğü kullanıcıları. <br/><br/> **Linux:** Bulunmasını istediğiniz Linux sunucularında bir kök hesabınız olması gerekir. |
| **İşletim sistemi** | Windows Server 2003 ve SUSE Linux dışında, Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri.|


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Fiziksel sunucular için gereç, bir VM 'de veya fiziksel bir makinede çalıştırılabilir. Gereci, Azure portal indirtiğiniz bir PowerShell betiği kullanarak ayarlarsınız.

- Fiziksel sunucular için [gereç gereksinimleri](migrate-appliance.md#appliance---physical) hakkında bilgi edinin.
- Gereçlerin [ortak](migrate-appliance.md#public-cloud-urls) ve [kamu](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken URL 'ler hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**Cihaz** | **Bağlanma**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 ' deki gelen bağlantılar, Gereç ile Uzak Masaüstü bağlantılarına izin vermek için.<br/><br/> Bağlantı noktası 44368 ' de, URL 'YI kullanarak gereç yönetimi uygulamasına uzaktan erişmek için gelen bağlantılar:``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktalarında giden bağlantılar.
**Fiziksel sunucular** | **Windows:** WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar, Windows sunucularından yapılandırma ve performans meta verilerini çekme. <br/><br/> **Linux:**  22 (UDP) bağlantı noktasındaki gelen bağlantılar, Linux sunucularından yapılandırma ve performans meta verilerini çekme. |

## <a name="agent-based-dependency-analysis-requirements"></a>Aracı tabanlı bağımlılık Analizi gereksinimleri

[Bağımlılık Analizi](concepts-dependency-visualization.md) , değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracı tabanlı bağımlılık analizini ayarlamaya yönelik gereksinimleri özetler. Şu anda yalnızca aracı tabanlı bağımlılık Analizi fiziksel sunucular için desteklenir.

**Gereksinim** | **Bilgileri** 
--- | --- 
**Dağıtımdan önce** | Sunucu değerlendirme aracı projeye eklenerek bir Azure geçişi projesi olması gerekir.<br/><br/>  Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız<br/><br/> İlk kez bir proje oluşturmayı [öğrenin](create-manage-projects.md) .<br/> Mevcut bir projeye değerlendirme aracı eklemeyi [öğrenin](how-to-assess.md) .<br/> [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)veya fiziksel sunucu değerlendirmesi için Azure geçişi gerecini ayarlamayı öğrenin.
**Azure Devlet Kurumları** | Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.
**Log Analytics** | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../log-analytics/log-analytics-overview.md) [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) çözümünü kullanır.<br/><br/> Yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz. Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde bulunmalıdır. Diğer bölgelerdeki çalışma alanları bir projeyle ilişkilendirilemez.<br/><br/> Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede olması gerekir.<br/><br/> Log Analytics, Azure geçişi ile ilişkili çalışma alanı, geçiş projesi anahtarıyla ve proje adıyla etiketlenir.
**Gerekli aracılar** | Çözümlemek istediğiniz her makinede aşağıdaki aracıları yükleyebilirsiniz:<br/><br/> [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/> [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Şirket içi makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidi indirip yüklemeniz gerekir.<br/><br/> [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) yükleme ve [MMA](how-to-create-group-machine-dependencies.md#install-the-mma)hakkında daha fazla bilgi edinin.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure geçişi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma alanının [hizmet eşlemesi desteklendiği](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites)bir bölgede olması gerekir.<br/><br/> Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez.
**Maliyetler** | Hizmet Eşlemesi çözümü ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirdiğinizden itibaren)/<br/><br/> 180 günden sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması Log Analytics için [Standart ücretler](https://azure.microsoft.com/pricing/details/log-analytics/) doğurur.<br/><br/> Azure geçişi projesi silindiğinde, çalışma alanı onunla birlikte silinmez. Projeyi sildikten sonra Hizmet Eşlemesi kullanımı ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure genel kullanım (GA-28 Şubat 2018) geçirmeden önce oluşturduğunuz projeleriniz varsa, ek Hizmet Eşlemesi ücretleri tahakkuk etmeyebilirsiniz. Yalnızca 180 günden sonra ödemeyi sağlamak için, GA 'nin mevcut çalışma alanları Ücretlendirilebilir olmaya devam ettiğinden yeni bir proje oluşturmanızı öneririz.
**Yönetim** | Aracıları çalışma alanına kaydettiğinizde, Azure geçişi projesi tarafından sunulan KIMLIĞI ve anahtarı kullanırsınız.<br/><br/> Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz.<br/><br/> İlişkili Azure geçişi projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El Ile silin](../azure-monitor/platform/manage-access.md).<br/><br/> Azure geçişi projesini silmediğiniz takdirde Azure geçişi tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.
**İnternet bağlantısı** | Makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidini yüklemeniz gerekir.
**Azure Devlet Kurumları** | Aracı tabanlı bağımlılık Analizi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Fiziksel sunucu değerlendirmesi Için hazırlanma](tutorial-prepare-physical.md).
