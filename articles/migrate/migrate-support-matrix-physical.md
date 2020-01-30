---
title: Azure geçişi ile fiziksel sunucu değerlendirmesi desteği
description: Azure geçişi ile fiziksel sunucu değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: 489f95bbbbeb261b56f1a3a86da44f5fcce0adf5
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76846578"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesi için destek matrisi 

Makineleri değerlendirmek ve Microsoft Azure buluta geçirmek için [Azure geçişi hizmetini](migrate-overview.md) kullanabilirsiniz. Bu makalede, şirket içi fiziksel sunucuları değerlendirmek ve geçirmek için destek ayarları ve sınırlamaları özetlenmektedir.


## <a name="overview"></a>Genel Bakış

Bu makaleyle Azure 'a geçiş için şirket içi makineleri değerlendirmek üzere Azure geçişi: Sunucu değerlendirmesi aracını bir Azure geçiş projesine ekleyin. [Azure geçişi](migrate-appliance.md)gereci dağıtırsınız. Gereç, şirket içi makineleri sürekli olarak bulur ve Azure 'a yapılandırma ve performans verilerini gönderir. Makine bulma işleminden sonra keşfedilen makineleri gruplar halinde toplar ve grup için bir değerlendirme çalıştırırsınız

## <a name="limitations"></a>Sınırlamalar

**Destek** | **Ayrıntılar**
--- | ---
**Değerlendirme limitleri**| Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 fiziksel sunucuyu bulur ve değerlendirin.
**Proje limitleri** | Bir Azure aboneliğinde birden çok proje oluşturabilirsiniz. Bir proje, VMware VM 'leri, Hyper-V VM 'Leri ve fiziksel sunucuları, değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure geçişi gereci en fazla 250 fiziksel sunucu bulabilir.
**Değerlendirme** | Tek bir gruba en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede en fazla 35.000 makine değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin](concepts-assessment-calculation.md) .




## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

| **Destek**                | **Ayrıntılar**               
| :-------------------       | :------------------- |
| **Fiziksel sunucu dağıtımı**       | Fiziksel sunucu tek başına olabilir veya bir kümede dağıtılabilir. |
| **İzinler**           | **Windows:** Bulmaya dahil etmek istediğiniz tüm Windows sunucularında yerel bir kullanıcı hesabı ayarlayın. Kullanıcı hesabının bu gruplara eklenmesi gerekir-uzak masaüstü kullanıcıları, performans Izleyicisi kullanıcıları ve performans günlüğü kullanıcıları. <br/> **Linux:** Bulunmasını istediğiniz Linux sunucularında bir kök hesabınız olması gerekir. |
| **İşletim sistemi** | Aşağıdakiler dışında tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri desteklenir:<br/> Windows Server 2003 <br/> SUSE Linux|


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure geçişi, bulma ve değerlendirme için [Azure geçişi](migrate-appliance.md) gereci kullanır. Fiziksel sunucular için gereç, bir VM 'de veya fiziksel bir makinede çalıştırılabilir. Bu ayarı, Azure portal indirtiğiniz bir PowerShell betiği kullanarak ayarlarsınız.

- Fiziksel sunucular için [gereç gereksinimleri](migrate-appliance.md#appliance---physical) hakkında bilgi edinin.
- Gerecin erişmesi gereken [URL 'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda, değerlendirme için bağlantı noktası gereksinimleri özetlenmektedir.

**cihaz** | **bağlantı**
--- | ---
**Elektrikli** | TCP bağlantı noktası 3389 üzerindeki gelen bağlantılar, gereci Uzak Masaüstü bağlantılarına izin vermek için.<br/> 44368 numaralı bağlantı noktası ile gereç yönetimi uygulamasına uzaktan erişim için gelen bağlantılar: ``` https://<appliance-ip-or-name>:44368 ```<br/> Azure geçişi 'ne bulma ve performans meta verileri göndermek için 443 (HTTPS), 5671 ve 5672 (AMQP) bağlantı noktalarında giden bağlantılar.
**Fiziksel sunucular** | **Windows:** WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerinde gelen bağlantılar, Windows sunucularından yapılandırma ve performans meta verilerini çekme. <br/> **Linux:**  22 (UDP) bağlantı noktasındaki, Linux sunucularından yapılandırma ve performans meta verileri çekmek için gelen bağlantılar. |

## <a name="agent-based-dependency-visualization"></a>Aracı tabanlı bağımlılık görselleştirmesi

[Bağımlılık görselleştirme](concepts-dependency-visualization.md) , değerlendirmek ve geçirmek istediğiniz makineler arasında bağımlılıkları görselleştirmenize yardımcı olur. Aracı tabanlı görselleştirme için, gereksinimler ve sınırlamalar aşağıdaki tabloda özetlenmiştir.


**Gereksinim** | **Ayrıntılar**
--- | ---
**Dağıtım** | Bağımlılık görselleştirmesini dağıtmadan önce, Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesine sahip olmanız gerekir. Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız.<br/><br/> Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.
**Hizmet Eşlemesi** | Aracı tabanlı bağımlılık görselleştirmesi [Azure izleyici günlüklerinde](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) [hizmet eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) çözümünü kullanır.<br/><br/> Dağıtımı yapmak için, yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure geçişi Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma alanının [hizmet eşlemesi desteklendiği](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites)bir bölgede olması gerekir.<br/><br/> Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez.
**Ücretleriyle** | Hizmet Eşlemesi çözümü, ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirmenizin bulunduğu günden).<br/><br/> 180 gün sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması standart Log Analytics ücretlendirmeye tabi olacaktır.<br/><br/> Azure geçişi projesini silerseniz, çalışma alanı onunla silinmez. Projeyi sildikten sonra Hizmet Eşlemesi ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir.
**Aracısını** | Aracı tabanlı bağımlılık görselleştirmesi, çözümlemek istediğiniz her makineye iki aracı yüklenmesini gerektirir.<br/><br/> - [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows)<br/><br/> - [bağımlılık Aracısı](https://docs.microsoft.com/azure/azure-monitor/platform/agents-overview#dependency-agent). 
**İnternet bağlantısı** | Makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidini yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Fiziksel sunucu değerlendirmesi Için hazırlanma](tutorial-prepare-physical.md).
