---
title: Azure Geçiş'te VMware değerlendirme desteği
description: Azure Geçir Sunucu Değerlendirmesi ile VMware VM değerlendirmesi desteği hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/23/2020
ms.openlocfilehash: 03d07adb6f19346901286bdae148f95e68290e4e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80336871"
---
# <a name="support-matrix-for-vmware-assessment"></a>VMware değerlendirmesi için destek matrisi 

Bu makalede, Azure'a geçişe hazırlık aşamasında VMware VM'leri değerlendirmek için ön koşullar ve destek gereksinimleri özetlenmiştir. VMware VM'leri Azure'a geçirmek istiyorsanız, [geçiş destek matrisini](migrate-support-matrix-vmware-migration.md)gözden geçirin.

[Azure Geçir:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracıyla fiziksel sunucuları değerlendirirsiniz. Bir Azure Geçir projesi oluşturun ve sonra aracı projeye eklersiniz. Araç eklendikten sonra Azure [Geçir cihazını](migrate-appliance.md)dağıtmış sınız. Cihaz sürekli olarak şirket içi makineleri keşfeder ve makine meta verilerini ve performans verilerini Azure'a gönderir. Makine keşfinden sonra, keşfedilen makineleri gruplar halinde toplar ve bir grup için bir değerlendirme çalıştırın.

## <a name="limitations"></a>Sınırlamalar

**Destek** | **Şey**
--- | ---
**Proje sınırları** | Azure aboneliğinde birden çok proje oluşturabilirsiniz.<br/><br/> Tek bir [projede](migrate-support-matrix.md#azure-migrate-projects)35.000 VMware VM'ye kadar keşfedebilir ve değerlendirebilirsiniz. Proje, her biri için değerlendirme sınırlarına kadar VMware VM'leri, fiziksel sunucuları ve Hyper-V V'leri içerebilir.
**Bulma** | Azure Geçir cihazı, vCenter Server'da 10.000 VMware VM'ye kadar keşfedebilir.
**Değerlendirme** | Tek bir grupta en fazla 35.000 makine ekleyebilirsiniz.<br/><br/> Tek bir değerlendirmede en fazla 35.000 VM'yi değerlendirebilirsiniz.

Değerlendirmeler hakkında [daha fazla bilgi edinin.](concepts-assessment-calculation.md)


## <a name="application-discovery"></a>Uygulama bulma

Azure Geçir: Sunucu Değerlendirmesi, makineleri keşfetmenin yanı sıra uygulamaları, rolleri ve makinelerde çalışan özellikleri de keşfedebilir. Uygulama envanterinizi keşfetmek, şirket içi iş yüklerinize göre uyarlanmış bir geçiş yolu belirlemenize ve planlamanıza olanak tanır. 

**Destek** | **Şey**
--- | ---
**Desteklenen makineler** | Uygulama bulma şu anda yalnızca şirket içi VMware VM'ler için desteklenir.
**Bulma** | Uygulama bulma aracısız. Makine konuk kimlik bilgilerini kullanır ve WMI ve SSH çağrılarını kullanarak makinelere uzaktan erişir.
**VM desteği** | Uygulama bulma tüm Windows ve Linux sürümleri için desteklenir.
**vCenter kimlik bilgileri** | Uygulama bulma nın yalnızca salt okunur erişime sahip bir vCenter Server hesabına ve Sanal Makineler > Konuk İşlemleri için etkinleştirilen ayrıcalıklara ihtiyacı vardır.
**VM kimlik bilgileri** | Uygulama bulma şu anda tüm Windows sunucuları için bir kimlik bilgisi ve tüm Linux sunucuları için bir kimlik bilgisi kullanımını destekler.<br/><br/> Windows VM'ler için bir konuk kullanıcı hesabı ve tüm Linux VM'leri için normal/normal bir kullanıcı hesabı (sudo erişimi olmayan) oluşturursunuz.
**VMware araçları** | VMware araçları, keşfetmek istediğiniz VM'ler üzerinde yüklü ve çalışıyor olmalıdır. <br/> VMware araçları sürümü 10.2.0'dan sonra olmalıdır.
**Powershell** | VM'lerde PowerShell sürüm 2.0 veya daha sonra yüklenmiş olmalıdır.
**Bağlantı noktası erişimi** | Keşfetmek istediğiniz VM'leri çalıştıran ESXi ana bilgisayarlarında Azure Geçiş cihazının TCP bağlantı noktası 443'e bağlanabilmesi gerekir.
**Limitler** | Uygulama bulma için, her Azure Geçiş cihazında en fazla 10000 VM keşfedebilirsiniz.



## <a name="vmware-requirements"></a>VMware gereksinimleri

**VMware** | **Şey**
--- | ---
**VMware Sanal Makineleri** | Değerlendirme tüm Windows ve Linux işletim sistemleri için desteklenir.
**vCenter Sunucu** | Bulmak ve değerlendirmek istediğiniz makineler vCenter Server sürüm 5.5, 6.0, 6.5 veya 6.7 tarafından yönetilmelidir.
**İzinler (değerlendirme)** | vCenter Server salt okunur hesabı.
**İzinler (uygulama bulma)** | sadece okunan erişime sahip vCenter Server hesabı ve Konuk İşlemleri > Sanal makineler için etkinleştirilen **ayrıcalıklar.**
**İzinler (bağımlılık görselleştirme)** | Salt okunur erişime sahip Center Server hesabı ve Sanal makineler için etkinleştirilen **ayrıcalıklar** > **Konuk İşlemleri.**


## <a name="azure-migrate-appliance-requirements"></a>Azure Geçişi aleti gereksinimleri

Azure Geçir, keşif ve değerlendirme için [Azure Geçir cihazını](migrate-appliance.md) kullanır. VMware için cihaz, vCenter Server'a aktarılan bir OVA şablonu kullanılarak dağıtılır. 

- VMware için [cihaz gereksinimleri](migrate-appliance.md#appliance---vmware) hakkında bilgi edinin.
- Cihazın erişmesi gereken [URL'ler](migrate-appliance.md#url-access) hakkında bilgi edinin.

## <a name="port-access"></a>Bağlantı noktası erişimi

**Cihaz** | **Bağlantı**
--- | ---
Cihaz | Cihaza uzak masaüstü bağlantılarına izin vermek için TCP bağlantı noktası 3389'daki gelen bağlantılar.<br/><br/> URL'yi kullanarak cihaz yönetimi uygulamasına uzaktan erişmek için 44368 bağlantı noktasındaki gelen bağlantılar:```https://<appliance-ip-or-name>:44368``` <br/><br/>Azure Geçiş'e bulma ve performans meta verileri göndermek için 443 (HTTPS) bağlantı noktasındaki giden bağlantılar.
vCenter server | Cihazın değerlendirmeler için yapılandırma ve performans meta verileri toplamasına olanak sağlamak için TCP bağlantı noktası 443'teki gelen bağlantılar. <br/><br/> Cihaz varsayılan olarak 443 portundaki vCenter'a bağlanır. vCenter sunucusu farklı bir bağlantı noktasında dinliyorsa, bulma ayarlarken bağlantı noktasını değiştirebilirsiniz.
ESXi ana bilgisayarları (uygulama bulma/aracısız bağımlılık analizi) | [Uygulama bulma](how-to-discover-applications.md) veya [aracısız bağımlılık çözümlemesi](concepts-dependency-visualization.md#agentless-analysis)yapmak istiyorsanız, cihaz, Uygulamaları keşfetmek, VM'lerde aracısız bağımlılık görselleştirmesi yapmak ve çalıştırmak için TCP bağlantı noktası 443'teki ESXi ana bilgisayarlarına bağlanır.

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
**VMware Araçları** | VMware Araçları (daha sonra 10.2) yüklü olmalı ve analiz etmek istediğiniz her VM üzerinde çalışıyor.
**vCenter Server kimlik bilgileri** | Bağımlılık görselleştirme, salt okunur erişime sahip bir vCenter Server hesabına ve Konuk İşlemleri > Sanal Makineler için etkinleştirilen ayrıcalıklara ihtiyaç duyar. 
**Powershell** | VM'lerde PowerShell sürüm 2.0 veya üzeri yüklü olmalıdır.
**Bağlantı noktası erişimi** | Analiz etmek istediğiniz VM'leri çalıştıran ESXi ana bilgisayarlarında Azure Geçiş cihazının TCP bağlantı noktası 443'e bağlanabilmesi gerekir.

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

- Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin.](best-practices-assessment.md)
- [VMware değerlendirmesine hazırlanın.](tutorial-prepare-vmware.md)
