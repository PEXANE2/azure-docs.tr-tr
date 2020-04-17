---
title: Azure Geçiş'te fiziksel sunucu değerlendirmesi desteği
description: Azure Geçir Sunucu Değerlendirmesi ile fiziksel sunucu değerlendirmesi desteği hakkında bilgi edinin
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: ae76a6b570ec58e71a8a1728a2a601728030f58c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81538163"
---
# <a name="support-matrix-for-physical-server-assessment"></a>Fiziksel sunucu değerlendirmesi için destek matrisi 

Bu makalede, [Azure Geçiş:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracını kullanarak azure'a geçiş için fiziksel sunucuları değerlendirirken ön koşullar ve destek gereksinimleri özetlenmiştir. Fiziksel sunucuları Azure'a geçirmek istiyorsanız, [geçiş destek matrisini](migrate-support-matrix-physical-migration.md)gözden geçirin.


Fiziksel sunucuları değerlendirmek için bir Azure Geçir projesi oluşturur sunuz ve projeye Sunucu Değerlendirmesi aracını ekleyin. Araç eklendikten sonra Azure [Geçir cihazını](migrate-appliance.md)dağıtmış sınız. Cihaz sürekli olarak şirket içi makineleri keşfeder ve makine meta verilerini ve performans verilerini Azure'a gönderir. Keşif tamamlandıktan sonra, keşfedilen makineleri gruplar halinde toplar ve bir grup için bir değerlendirme çalıştırın.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Şey**
--- | ---
**Değerlendirme limitleri** | Tek bir [Azure Geçiş projesinde](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 fiziksel sunucu keşfedebilir ve değerlendirebilirsiniz.
**Proje sınırları** | Azure aboneliğinde birden çok proje oluşturabilirsiniz. Fiziksel sunuculara ek olarak, bir proje vmware VM'ler ve Hyper-V VM'ler, her biri için değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure Geçir cihazı en fazla 250 fiziksel sunucu keşfedebilir.
**Değerlendirme** | Tek bir grupta en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede en fazla 35.000 makineyi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin.](concepts-assessment-calculation.md)

## <a name="physical-server-requirements"></a>Fiziksel sunucu gereksinimleri

| **Destek**                | **Şey**               
| :-------------------       | :------------------- |
| **Fiziksel sunucu dağıtımı**       | Fiziksel sunucu tek başına veya bir kümede dağıtılabilir. |
| **İzinler**           | **Windows:** Keşfetmek istediğiniz tüm Windows sunucularında yerel veya etki alanı kullanıcı hesabına ihtiyacınız vardır. Kullanıcı hesabı bu gruplara eklenmelidir: Uzak Masaüstü Kullanıcıları, Performans Monitörü Kullanıcıları ve Performans Günlüğü kullanıcıları. <br/><br/> **Linux:** Keşfetmek istediğiniz Linux sunucularında bir kök hesaba ihtiyacınız vardır. |
| **İşletim sistemi** | Windows Server 2003 ve SUSE Linux hariç Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri.|


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure Geçir, keşif ve değerlendirme için [Azure Geçir cihazını](migrate-appliance.md) kullanır. Fiziksel sunucular için cihaz vm veya fiziksel bir makine üzerinde çalıştırabilirsiniz. Azure portalından indirdiğiniz powershell komut dosyasını kullanarak cihazı ayarlarsınız.

- Fiziksel [sunucular](migrate-appliance.md#appliance---physical) için cihaz gereksinimleri hakkında bilgi edinin.
- Cihazın [genel](migrate-appliance.md#public-cloud-urls) ve [devlet](migrate-appliance.md#government-cloud-urls) bulutlarında erişmesi gereken URL'ler hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda değerlendirme için bağlantı noktası gereksinimleri özetlenmiştir.

**Cihaz** | **Bağlantı**
--- | ---
**Cihaz** | Cihaza uzak masaüstü bağlantılarına izin vermek için TCP bağlantı noktası 3389'daki gelen bağlantılar.<br/><br/> URL'yi kullanarak cihaz yönetimi uygulamasına uzaktan erişmek için 44368 no'daki bağlantı noktasıüzerindeki gelen bağlantılar:``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Azure Geçiş'e bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktalarındagiden bağlantılar.
**Fiziksel sunucular** | **Windows:** Windows sunucularından yapılandırma ve performans meta verilerini çekmek için WinRM bağlantı noktaları5985 (HTTP) ve 5986 (HTTPS) üzerindeki gelen bağlantılar. <br/><br/> **Linux:**  Linux sunucularından yapılandırma ve performans meta verilerini çekmek için bağlantı 22 (UDP) bağlantı larından gelen bağlantılar. |

## <a name="agent-based-dependency-analysis-requirements"></a>Aracı tabanlı bağımlılık analizi gereksinimleri

[Bağımlılık çözümlemesi,](concepts-dependency-visualization.md) değerlendirmek ve Azure'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracı tabanlı bağımlılık çözümlemesi için gereksinimleri özetler. Şu anda yalnızca aracı tabanlı bağımlılık çözümlemesi fiziksel sunucular için desteklenir.

**Gereksinim** | **Şey** 
--- | --- 
**Dağıtımdan önce** | Sunucu Değerlendirme aracı projeye eklenen bir Azure Geçiş projeniz olmalıdır.<br/><br/>  Şirket içi makinelerinizi keşfetmek için bir Azure Geçir cihazı kurduktan sonra bağımlılık görselleştirmesini dağıtAbilirsiniz<br/><br/> İlk kez bir [projeoluşturmayı öğrenin.](create-manage-projects.md)<br/> Varolan bir projeye nasıl bir değerlendirme aracı ekleyeceğinizi [öğrenin.](how-to-assess.md)<br/> [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)veya fiziksel sunucuların değerlendirilmesi için Azure Geçir cihazını nasıl ayarlayacağınızı öğrenin.
**Azure Devlet Kurumları** | Bağımlılık görselleştirmesi Azure Kamu'da kullanılamaz.
**Log Analytics** | Azure Geçir, bağımlılık görselleştirme [süslerinde](../log-analytics/log-analytics-overview.md) [Hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md) çözümünü kullanır.<br/><br/> Yeni veya varolan bir Log Analytics çalışma alanını bir Azure Geçiş projesiyle ilişkilendirin. Azure Geçir projesinin çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure Geçiş projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde ikamet etmelidir. Diğer bölgelerdeki çalışma alanları projeyle ilişkilendirilemez.<br/><br/> Çalışma [alanı, Hizmet Haritası'nın desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede olmalıdır.<br/><br/> Günlük Analizi'nde, Azure Geçişi ile ilişkili çalışma alanı Geçiş Projesi anahtarı ve proje adı ile etiketlenir.
**Gerekli aracılar** | Analiz etmek istediğiniz her makineye aşağıdaki aracıları yükleyin:<br/><br/> [Microsoft İzleme aracısı (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/> [Bağımlılık aracısı.](../azure-monitor/platform/agents-overview.md#dependency-agent)<br/><br/> Şirket içi makineler internete bağlı değilse, Log Analytics ağ geçidini indirmeniz ve yüklemeniz gerekir.<br/><br/> [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) ve [MMA'yı](how-to-create-group-machine-dependencies.md#install-the-mma)yükleme hakkında daha fazla bilgi edinin.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure Geçiş projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure Geçir, Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma [alanı, Hizmet Haritası'nın desteklendiği](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites)bir bölgede olmalıdır.<br/><br/> Azure Geçir projesinin çalışma alanı eklendikten sonra değiştirilemez.
**Maliyetler** | Hizmet Haritası çözümü, ilk 180 gün boyunca (Log Analytics çalışma alanını Azure Geçiş projesiyle ilişkilendirdiğiniz günden itibaren) herhangi bir ücrete tabi değildir)/<br/><br/> 180 günden sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Haritası dışında herhangi bir çözümün kullanılması, Log Analytics için [standart ücrete](https://azure.microsoft.com/pricing/details/log-analytics/) tabi olacaktır.<br/><br/> Azure Geçir projesi silindiğinde, çalışma alanı da silinmez. Proje silen projeden sonra, Hizmet Haritası kullanımı ücretsiz değildir ve her düğüm Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure Geçiş genel kullanılabilirliğinden önce oluşturduğunuz projeleriniz varsa (GA- 28 Şubat 2018), ek Hizmet Haritası ücretleri ne tabi olabilir. Yalnızca 180 gün sonra ödeme sağlamak için, GA'dan önceki mevcut çalışma alanları hala ücretli olduğundan yeni bir proje oluşturmanızı öneririz.
**Yönetim** | Aracıları çalışma alanına kaydettiğinizde, Azure Geçiş projesi tarafından sağlanan kimlik ve anahtarı kullanırsınız.<br/><br/> Azure Geçiş dışında Günlük Analizi çalışma alanını kullanabilirsiniz.<br/><br/> İlişkili Azure Geçiş projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El ile silin.](../azure-monitor/platform/manage-access.md)<br/><br/> Azure Geçiş projesini silerseniz Azure Geçir tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.
**İnternet bağlantısı** | Makineler Internet'e bağlı değilse, log Analytics ağ geçidini yüklemeniz gerekir.
**Azure Devlet Kurumları** | Aracı tabanlı bağımlılık çözümlemesi desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

[Fiziksel sunucu değerlendirmesi için hazırlanın.](tutorial-prepare-physical.md)
