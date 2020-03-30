---
title: Azure Geçiş'te Hyper-V değerlendirmesi desteği
description: Azure Geçir Sunucu Değerlendirmesi ile Hyper-V değerlendirmesi desteği hakkında bilgi edinin
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 61d8e635a32024ba5afabb34fefa5bf169e2911f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336903"
---
# <a name="support-matrix-for-hyper-v-assessment"></a>Hyper-V değerlendirmesi için destek matrisi

Bu makalede, Azure'a geçişe hazırlık aşamasında Hyper-V VM'lerin değerlendirilmesi için ön koşullar ve destek gereksinimleri özetlenmiştir. Hyper-V VM'leri Azure'a geçirmek istiyorsanız, [geçiş destek matrisini](migrate-support-matrix-hyper-v-migration.md)gözden geçirin.

Hyper-V VM'leri [Azure Geçiş:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla değerlendirirsiniz. Bir Azure Geçir projesi oluşturun ve sonra aracı projeye eklersiniz. Araç eklendikten sonra Azure [Geçir cihazını](migrate-appliance.md)dağıtmış sınız. Cihaz sürekli olarak şirket içi makineleri keşfeder ve makine meta verilerini ve performans verilerini Azure'a gönderir. Makine keşfinden sonra, keşfedilen makineleri gruplar halinde toplar ve bir grup için bir değerlendirme çalıştırın.


## <a name="limitations"></a>Sınırlamalar

**Destek** | **Şey**
--- | ---
**Değerlendirme limitleri** | Tek bir [Azure Geçiş projesinde](migrate-support-matrix.md#azure-migrate-projects)en fazla 35.000 Hyper-V V VM keşfedebilir ve değerlendirebilirsiniz.
**Proje sınırları** | Azure aboneliğinde birden çok proje oluşturabilirsiniz. Hyper-V VM'lere ek olarak, bir proje VMware VM'leri ve fiziksel sunucuları, her biri için değerlendirme sınırlarına kadar içerebilir.
**Bulma** | Azure Geçir cihazı 5000'e kadar Hyper-V VM keşfedebilir.<br/><br/> Cihaz en fazla 300 Hyper-V ana bilgisayara bağlanabilir.
**Değerlendirme** | Tek bir grupta en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Bir grup için tek bir değerlendirmede en fazla 35.000 VM'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin.](concepts-assessment-calculation.md)



## <a name="hyper-v-host-requirements"></a>Hyper-V ana bilgisayar gereksinimleri

| **Destek**                | **Şey**               
| :-------------------       | :------------------- |
| **Hyper-V konağı**       | Hyper-V ana bilgisayar tek başına veya bir kümede dağıtılabilir.<br/><br/> Hyper-V ana bilgisayar Windows Server 2019, Windows Server 2016 veya Windows Server 2012 R2 çalıştırabilir.<br/> Windows Server 2012 çalıştıran Hyper-V konaklarında yer alan VM'leri değerlendiremezsiniz.
| **Izin**           | Hyper-V ana bilgisayarda Yönetici izinlerine ihtiyacınız vardır. <br/> Yönetici izinleri atamak istemiyorsanız, yerel veya etki alanı kullanıcı hesabı oluşturun ve kullanıcı hesabını bu gruplara ekleyin: Uzak Yönetim Kullanıcıları, Hyper-V Yöneticileri ve Performans Monitörü Kullanıcıları. |
| **PowerShell uzaktan iletişim**   | [PowerShell remoting](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-7) her Hyper-V ana bilgisayarda etkinleştirilmelidir. |
| **Hyper-V Çoğaltma**       | Hyper-V Çoğaltma kullanıyorsanız (veya aynı VM tanımlayıcıları olan birden fazla VM'inüz varsa) ve Azure Geçiş'i kullanarak hem orijinal hem de çoğaltılan VM'leri keşfederseniz, Azure Geçiş tarafından oluşturulan değerlendirme doğru olmayabilir. |


## <a name="hyper-v-vm-requirements"></a>Hyper-V VM gereksinimleri

| **Destek**                  | **Şey**               
| :----------------------------- | :------------------- |
| **İşletim sistemi** | Azure tarafından desteklenen tüm [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) ve [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) işletim sistemleri. |
| **Tümleştirme Hizmetleri**       | [Hyper-V Entegrasyon Hizmetleri,](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) işletim sistemi bilgilerini yakalamak için değerlendirdiğiniz VM'lerde çalışıyor olmalıdır. |


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure Geçir, keşif ve değerlendirme için [Azure Geçir cihazını](migrate-appliance.md) kullanır. Cihazı portaldan indirdiğiniz sıkıştırılmış Hyper-V VHd kullanarak veya PowerShell komut dosyası kullanarak dağıtabilirsiniz.

- Hyper-V için [cihaz gereksinimleri](migrate-appliance.md#appliance---hyper-v) hakkında bilgi edinin.
- Cihazın erişmesi gereken [URL'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

Aşağıdaki tabloda değerlendirme için bağlantı noktası gereksinimleri özetlenmiştir.

**Cihaz** | **Bağlantı**
--- | ---
**Cihaz** | Cihaza uzak masaüstü bağlantılarına izin vermek için TCP bağlantı noktası 3389'daki gelen bağlantılar.<br/><br/> URL'yi kullanarak cihaz yönetimi uygulamasına uzaktan erişmek için 44368 bağlantı noktasındaki gelen bağlantılar:``` https://<appliance-ip-or-name>:44368 ```<br/><br/> Azure Geçiş'e bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktalarındagiden bağlantılar.
**Hiper-V ana bilgisayar/küme** | Ortak Bilgi Modeli (CIM) oturumu kullanarak Hyper-V V VM'ler için meta verileri ve performans verilerini çekmek için WinRM bağlantı noktaları 5985 (HTTP) ve 5986 (HTTPS) üzerindeki gelen bağlantılar.

## <a name="agentless-dependency-analysis-requirements"></a>Aracısız bağımlılık analizi gereksinimleri

[Bağımlılık çözümlemesi,](concepts-dependency-visualization.md) değerlendirmek ve Azure'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracısız bağımlılık çözümlemesi için gereksinimleri özetler. 

**Gereksinim** | **Şey** 
--- | --- 
**Dağıtımdan önce** | Projeye Azure Geçiş: Sunucu Değerlendirme aracı eklenmiştir.<br/><br/>  Şirket içi VMWare makinelerinizi keşfetmek için bir Azure Geçir cihazı kurduktan sonra bağımlılık görselleştirmesini dağıtAbilirsiniz.<br/><br/> İlk kez bir [projeoluşturmayı öğrenin.](create-manage-projects.md)<br/> Varolan bir projeye nasıl bir değerlendirme aracı ekleyeceğinizi [öğrenin.](how-to-assess.md)<br/> VMware VM'lerin değerlendirilmesi için Azure Geçir cihazını nasıl ayarlayatısınız [öğrenin.](how-to-set-up-appliance-vmware.md)
**VM desteği** | Şu anda yalnızca VMware VM'ler için desteklenir.
**Windows VM’leri** | Windows Server 2016<br/> Windows Server 2012 R2<br/> Windows Server 2012<br/> Windows Server 2008 R2 (64 bit).
**Windows hesabı** |  Bağımlılık çözümlemesi için Azure Geçiş cihazının Windows VM'lerine erişmek için yerel veya etki alanı yöneticisi hesabına ihtiyacı vardır.
**Linux VM'leri** | Kırmızı Şapka Kurumsal Linux 7, 6, 5<br/> Ubuntu Linux 14.04, 16.04<br/> Debian 7, 8<br/> Oracle Linux 6, 7<br/> CentOS 5, 6, 7.
**Linux hesabı** | Bağımlılık analizi için, Linux makinelerinde Azure Geçir cihazının Root ayrıcalığına sahip bir kullanıcı hesabına ihtiyacı vardır.<br/><br/> Alternatif olarak, kullanıcı hesabının /bin/netstat ve /bin/ls dosyalarında bu izinlere ihtiyacı vardır: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE.
**Gerekli aracılar** | Analiz etmek istediğiniz makinelerde ajan gerekmez.
**VMware araçları** | VMware Araçları (daha sonra 10.2) yüklü olmalı ve analiz etmek istediğiniz her VM üzerinde çalışıyor.
**vCenter Sunucu** | Bağımlılık görselleştirme, salt okunur erişime sahip bir vCenter Server hesabına ve Konuk İşlemleri > Sanal Makineler için etkinleştirilen ayrıcalıklara ihtiyaç duyar. **ESXi ana bilgisayarları**: Analiz etmek istediğiniz VM'leri çalıştıran ESXi ana bilgisayarlarında Azure Geçiş cihazı TCP bağlantı noktası 443'e bağlanabilmeli.

## <a name="agent-based-dependency-analysis-requirements"></a>Aracı tabanlı bağımlılık analizi gereksinimleri

[Bağımlılık çözümlemesi,](concepts-dependency-visualization.md) değerlendirmek ve Azure'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. Tablo, aracı tabanlı bağımlılık çözümlemesi için gereksinimleri özetler. 

**Gereksinim** | **Şey** 
--- | --- 
**Dağıtımdan önce** | Projeye Azure Geçiş: Sunucu Değerlendirme aracı eklenmiştir.<br/><br/>  Şirket içi makinelerinizi keşfetmek için bir Azure Geçir cihazı kurduktan sonra bağımlılık görselleştirmesini dağıtAbilirsiniz<br/><br/> İlk kez bir [projeoluşturmayı öğrenin.](create-manage-projects.md)<br/> Varolan bir projeye nasıl bir değerlendirme aracı ekleyeceğinizi [öğrenin.](how-to-assess.md)<br/> [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)veya fiziksel sunucuların değerlendirilmesi için Azure Geçir cihazını nasıl ayarlayacağınızı öğrenin.
**Azure Kamu** | Bağımlılık görselleştirmesi Azure Kamu'da kullanılamaz.
**Log Analytics** | Azure Geçir, bağımlılık görselleştirme [süslerinde](../log-analytics/log-analytics-overview.md) [Hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md) çözümünü kullanır.<br/><br/> Yeni veya varolan bir Log Analytics çalışma alanını bir Azure Geçiş projesiyle ilişkilendirin. Azure Geçir projesinin çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure Geçiş projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde ikamet etmelidir. Diğer bölgelerdeki çalışma alanları projeyle ilişkilendirilemez.<br/><br/> Çalışma [alanı, Hizmet Haritası'nın desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede olmalıdır.<br/><br/> Günlük Analizi'nde, Azure Geçişi ile ilişkili çalışma alanı Geçiş Projesi anahtarı ve proje adı ile etiketlenir.
**Gerekli aracılar** | Analiz etmek istediğiniz her makineye aşağıdaki aracıları yükleyin:<br/><br/> [Microsoft İzleme aracısı (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/> [Bağımlılık aracısı.](../azure-monitor/platform/agents-overview.md#dependency-agent)<br/><br/> Şirket içi makineler internete bağlı değilse, Log Analytics ağ geçidini indirmeniz ve yüklemeniz gerekir.<br/><br/> [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) ve [MMA'yı](how-to-create-group-machine-dependencies.md#install-the-mma)yükleme hakkında daha fazla bilgi edinin.
**Log Analytics çalışma alanı** | Çalışma alanı, Azure Geçiş projesiyle aynı abonelikte olmalıdır.<br/><br/> Azure Geçir, Doğu ABD, Güneydoğu Asya ve Batı Avrupa bölgelerinde bulunan çalışma alanlarını destekler.<br/><br/>  Çalışma [alanı, Hizmet Haritası'nın desteklendiği](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#prerequisites)bir bölgede olmalıdır.<br/><br/> Azure Geçir projesinin çalışma alanı eklendikten sonra değiştirilemez.
**Maliyetler** | Hizmet Haritası çözümü, ilk 180 gün boyunca (Log Analytics çalışma alanını Azure Geçiş projesiyle ilişkilendirdiğiniz günden itibaren) herhangi bir ücrete tabi değildir)/<br/><br/> 180 günden sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Haritası dışında herhangi bir çözümün kullanılması, Log Analytics için [standart ücrete](https://azure.microsoft.com/pricing/details/log-analytics/) tabi olacaktır.<br/><br/> Azure Geçir projesi silindiğinde, çalışma alanı da silinmez. Proje silen projeden sonra, Hizmet Haritası kullanımı ücretsiz değildir ve her düğüm Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure Geçiş genel kullanılabilirliğinden önce oluşturduğunuz projeleriniz varsa (GA- 28 Şubat 2018), ek Hizmet Haritası ücretleri ne tabi olabilir. Yalnızca 180 gün sonra ödeme sağlamak için, GA'dan önceki mevcut çalışma alanları hala ücretli olduğundan yeni bir proje oluşturmanızı öneririz.
**Yönetim** | Aracıları çalışma alanına kaydettiğinizde, Azure Geçiş projesi tarafından sağlanan kimlik ve anahtarı kullanırsınız.<br/><br/> Azure Geçiş dışında Günlük Analizi çalışma alanını kullanabilirsiniz.<br/><br/> İlişkili Azure Geçiş projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El ile silin.](../azure-monitor/platform/manage-access.md)<br/><br/> Azure Geçiş projesini silerseniz Azure Geçir tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.
**İnternet bağlantısı** | Makineler Internet'e bağlı değilse, log Analytics ağ geçidini yüklemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Hyper-V VM değerlendirmesine hazırlanın](tutorial-prepare-hyper-v.md)
