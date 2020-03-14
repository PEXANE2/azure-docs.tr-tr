---
title: Azure Geçişi'nde bağımlılık görselleştirme
description: Azure geçişi 'nde sunucu değerlendirmesi hizmeti 'nde değerlendirme hesaplamalarına genel bakış sağlar
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: fd069ed98fa34fd6f281c98a061925f96c7bb2cd
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79269711"
---
# <a name="dependency-visualization"></a>Bağımlılık görselleştirme

Bu makalede Azure geçişi: Sunucu değerlendirmesi ' nde bağımlılık görselleştirmesi açıklanmaktadır.

## <a name="what-is-dependency-visualization"></a>Bağımlılık görselleştirmesi nedir?

Bağımlılık görselleştirme, değerlendirmek ve Azure 'a geçirmek istediğiniz şirket içi makineler arasındaki bağımlılıkları belirlemenize yardımcı olur. 

- Azure geçişi: Sunucu değerlendirmesi ' nde, makineleri bir gruba toplayın ve sonra grubu değerlendirin. Bağımlılık görselleştirmesi, makineleri değerlendirme için yüksek güvenilirlikle daha doğru şekilde gruplandırmanıza yardımcı olur.
- Bağımlılık görselleştirme, birlikte geçirilmesi gereken makineleri tanımlamanızı sağlar. Makinelerin kullanımda olup olmadığını veya geçişi yerine kullanımdan engellenmeyeceğini belirleyebilirsiniz.
- Bağımlılıkların görselleştirilmesi, hiçbir şeyin geri ayrılmaması ve geçiş sırasında beklenmedik kesintilerden kaçınılması sağlar.
- Görselleştirme özellikle, makinelerin Azure 'a geçirmek istediğiniz bir uygulama dağıtımının parçası olup olmadığından emin olmadığınız durumlarda faydalıdır.


> [!NOTE]
> Bağımlılık görselleştirmesi Azure Kamu 'da kullanılamaz.

## <a name="agent-basedagentless-visualization"></a>Aracı tabanlı/aracısız görselleştirme

Bağımlılık görselleştirmesini dağıtmaya yönelik iki seçenek vardır:

- **Aracı**tabanlı: aracı tabanlı bağımlılık görselleştirme, analiz etmek istediğiniz her şirket içi makineye aracıların yüklenmesini gerektirir.
- **Aracısız**: Bu seçenekle, çapraz denetlemek istediğiniz makinelere aracılar yüklemeniz gerekmez. Bu seçenek şu anda önizleme aşamasındadır ve yalnızca VMware VM 'Leri için kullanılabilir.


## <a name="agent-based-visualization"></a>Aracı tabanlı görselleştirme

**Gereksinim** | **Ayrıntılar** | **Daha fazla bilgi**
--- | --- | ---
**Dağıtımdan önce** | Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesi olması gerekir.<br/><br/>  Şirket içi makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız. | İlk kez bir proje oluşturmayı [öğrenin](create-manage-projects.md) .<br/><br/> Mevcut bir projeye değerlendirme aracı eklemeyi [öğrenin](how-to-assess.md) .<br/><br/> [Hyper-V](how-to-set-up-appliance-hyper-v.md), [VMware](how-to-set-up-appliance-vmware.md)veya fiziksel sunucu değerlendirmesi için Azure geçişi gerecini ayarlamayı öğrenin.
**Gerekli aracılar** | Çözümlemek istediğiniz her makinede aşağıdaki aracıları yükleyebilirsiniz:<br/><br/> [Microsoft Monitoring Agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows).<br/><br/> [Bağımlılık Aracısı](../azure-monitor/platform/agents-overview.md#dependency-agent).<br/><br/> Şirket içi makineler Internet 'e bağlı değilse, bunlara Log Analytics ağ geçidi indirip yüklemeniz gerekir. | [Bağımlılık aracısını](how-to-create-group-machine-dependencies.md#install-the-dependency-agent) yükleme ve [MMA](how-to-create-group-machine-dependencies.md#install-the-mma)hakkında daha fazla bilgi edinin.
**Log Analytics** | Azure geçişi, bağımlılık görselleştirmesi için [Azure izleyici günlüklerinde](../log-analytics/log-analytics-overview.md) [hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) çözümünü kullanır.<br/><br/> Yeni veya mevcut bir Log Analytics çalışma alanını Azure geçişi projesiyle ilişkilendirirsiniz. Bir Azure geçişi projesi çalışma alanı eklendikten sonra değiştirilemez. <br/><br/> Çalışma alanı, Azure geçişi projesiyle aynı abonelikte olmalıdır.<br/><br/> Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde bulunmalıdır. Diğer bölgelerdeki çalışma alanları bir projeyle ilişkilendirilemez.<br/><br/> Çalışma alanının [hizmet eşlemesi desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede olması gerekir.<br/><br/> Log Analytics, Azure geçişi ile ilişkili çalışma alanı, geçiş projesi anahtarıyla ve proje adıyla etiketlenir.
**Malarını** | Hizmet Eşlemesi çözümü ilk 180 gün boyunca ücret almaz (Log Analytics çalışma alanını Azure geçişi projesi ile ilişkilendirdiğinizden itibaren)/<br/><br/> 180 gün sonra standart Log Analytics ücretleri uygulanır.<br/><br/> İlişkili Log Analytics çalışma alanında Hizmet Eşlemesi dışında herhangi bir çözümün kullanılması Log Analytics için [Standart ücretler](https://azure.microsoft.com/pricing/details/log-analytics/) doğurur.<br/><br/> Azure geçişi projesi silindiğinde, çalışma alanı onunla birlikte silinmez. Projeyi sildikten sonra Hizmet Eşlemesi kullanımı ücretsizdir ve her düğüm, Log Analytics çalışma alanının ücretli katmanına göre ücretlendirilir/<br/><br/>Azure genel kullanım (GA-28 Şubat 2018) geçirmeden önce oluşturduğunuz projeleriniz varsa, ek Hizmet Eşlemesi ücretleri tahakkuk etmeyebilirsiniz. Yalnızca 180 günden sonra ödemeyi sağlamak için, GA 'nin mevcut çalışma alanları Ücretlendirilebilir olmaya devam ettiğinden yeni bir proje oluşturmanızı öneririz.
**Yönetme** | Aracıları çalışma alanına kaydettiğinizde, Azure geçişi projesi tarafından sunulan KIMLIĞI ve anahtarı kullanırsınız.<br/><br/> Log Analytics çalışma alanını Azure geçişi dışında kullanabilirsiniz.<br/><br/> İlişkili Azure geçişi projesini silerseniz, çalışma alanı otomatik olarak silinmez. [El Ile silin](../azure-monitor/platform/manage-access.md).<br/><br/> Azure geçişi projesini silmediğiniz takdirde Azure geçişi tarafından oluşturulan çalışma alanını silmeyin. Bunu yaparsanız, bağımlılık görselleştirme işlevselliği beklendiği gibi çalışmaz.

## <a name="agentless-visualization"></a>Aracısız görselleştirme


**Gereksinim** | **Ayrıntılar** | **Daha fazla bilgi**
--- | --- | ---
**Dağıtımdan önce** | Azure geçişi: Sunucu değerlendirmesi aracı projeye eklenmiş bir Azure geçişi projesi olması gerekir.<br/><br/>  Şirket içi VMWare makinelerinizi bulmaya yönelik bir Azure geçiş gereci ayarladıktan sonra bağımlılık görselleştirmesini dağıtırsınız. | İlk kez bir proje oluşturmayı [öğrenin](create-manage-projects.md) .<br/><br/> Mevcut bir projeye değerlendirme aracı eklemeyi [öğrenin](how-to-assess.md) .<br/><br/> [VMware](how-to-set-up-appliance-vmware.md) VM 'lerinin değerlendirmesi Için Azure geçişi gerecini ayarlamayı öğrenin.
**Gerekli aracılar** | Çözümlemek istediğiniz makinelerde aracı gerekmez.
**Desteklenen işletim sistemleri** | Aracısız görselleştirme için desteklenen [işletim sistemlerini](migrate-support-matrix-vmware.md#agentless-dependency-visualization) gözden geçirin.
**VM’ler** | **VMware araçları**: çözümlemek istediğiniz VM 'Lerde VMware araçları yüklü ve çalışır olmalıdır.<br/><br/> **Hesap**: Azure geçişi gereci üzerinde, analiz Için VM 'lere erişmek üzere kullanılabilecek bir kullanıcı hesabı eklemeniz gerekir.<br/><br/> **Windows VM 'leri**: Kullanıcı hesabının makinede yerel veya etki alanı yöneticisi olması gerekir.<br/><br/> **Linux VM 'leri**: hesapta kök ayrıcalık gereklidir. Alternatif olarak, Kullanıcı hesabı/bin/netstat ve/bin/ls dosyalarında şu iki özelliği gerektirir: CAP_DAC_READ_SEARCH ve CAP_SYS_PTRACE. | Azure geçişi gereci [hakkında bilgi edinin](migrate-appliance.md) .
**VMware** | **vCenter**: gereç, salt okuma erişimi olan bir vCenter Server hesabına ve konuk Işlemleri > sanal makineler için etkinleştirilmiş ayrıcalıklara ihtiyaç duyuyor.<br/><br/> **ESXi Konakları**: çözümlemek Istediğiniz VM 'Leri çalıştıran ESXi konaklarında Azure geçişi gereci 443 numaralı TCP bağlantı noktasına bağlanabiliyor olmalıdır.
**Toplanan veriler** |  Aracısız bağımlılık analizi, etkin olduğu makinelerden gelen TCP bağlantısı verilerini yakalayarak işe yarar. Bağımlılık bulma etkinleştirildikten sonra, Gereç Konuk VM 'lerden 5 dakikada bir TCP bağlantı verilerini toplar. Bu veriler, vSphere API 'Leri kullanılarak vCenter Server aracılığıyla konuk VM 'lerden toplanır. Toplanan veriler, bağımlılık bilgilerini bırakmak ve 6 saatte bir Azure 'a gönderilmek üzere gereç üzerinde işlenir. Her makineden aşağıdaki veriler toplanır: <br/> -Etkin bağlantıları olan işlemlerin adları.<br/> -İşlemi etkin bağlantılarla çalıştıran uygulamaların adları.<br/> -Etkin bağlantılardaki hedef bağlantı noktası.


## <a name="next-steps"></a>Sonraki adımlar
- [Bağımlılık görselleştirmesini ayarlama](how-to-create-group-machine-dependencies.md)
- VMware VM 'Leri için [aracısız bağımlılık görselleştirmesini deneyin](how-to-create-group-machine-dependencies-agentless.md) .
- Bağımlılık görselleştirmesine ilişkin [genel soruları](common-questions-discovery-assessment.md#what-is-dependency-visualization) gözden geçirin.


