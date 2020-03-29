---
title: Azure Site Kurtarma ile Azure Trafik Yöneticisi | Microsoft Dokümanlar
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Kurtarma ile Azure Trafik Yöneticisi'nin nasıl kullanılacağını açıklar
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 6c77cd43231d4596535c11564313a0fe90633cdb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60947819"
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Site Recovery ile Azure Traffic Manager

Azure Trafik Yöneticisi, uygulama bitiş noktalarınız arasında trafik dağılımını denetlemenize olanak tanır. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Trafik Yöneticisi, trafik yönlendirme yöntemini ve uç noktaların durumunu temel alan, istemci isteklerini en uygun bitiş noktasına yönlendirmek için Alan Adı Sistemi'ni (DNS) kullanır. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](../traffic-manager/traffic-manager-monitoring.md) sunar. İstemciler doğrudan seçilen uç noktaya bağlanır. Trafik Yöneticisi bir proxy veya ağ geçidi değildir ve istemci ile hizmet arasında geçen trafiği görmez.

Bu makalede, Azure Trafik Monitörü'ne ait akıllı yönlendirmeile Azure Site Kurtarma'nın güçlü olağanüstü durum kurtarma ve geçiş özellikleriyle nasıl birleştirebileceğiniz açıklanmaktadır.

## <a name="on-premises-to-azure-failover"></a>Azure'a şirket içi başarısız

İlk senaryo için, tüm uygulama altyapısı şirket içi ortamında çalışan **A Şirketini** düşünün. İş sürekliliği ve uyumluluk **nedenleriyle, A Şirketi** uygulamalarını korumak için Azure Site Kurtarma'yı kullanmaya karar verir.

**A şirketi,** genel uç noktaları olan uygulamaları çalıştırıyor ve bir felaket durumunda trafiği sorunsuz bir şekilde Azure'a yönlendirme olanağı istiyor. Azure Trafik Yöneticisi'ndeki [Öncelikli](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafik yönlendirme yöntemi, A Şirketinin bu hata değiştirme desenini kolayca uygulamasına olanak tanır.

Kurulum aşağıdaki gibidir:
- **Şirket A,** [Trafik Yöneticisi profili](../traffic-manager/traffic-manager-create-profile.md)oluşturur.
- **Öncelikli** yönlendirme yöntemini kullanan **Şirket A,** şirket için **birincil** ve Azure için **Failover** olmak üzere iki uç nokta oluşturur. **Birincil** Öncelik 1 atanır ve **Failover** Öncelik 2 atanır.
- **Birincil** bitiş noktası Azure dışında barındırıldığı için, bitiş noktası [Dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç nokta olarak oluşturulur.
- Azure Site Kurtarma ile Azure sitesinde başarısız olmadan önce çalışan sanal makine veya uygulama yoktur. Bu nedenle, **Failover** bitiş noktası da **harici** uç noktası olarak oluşturulur.
- Varsayılan olarak, bu uç nokta onunla ilişkili en yüksek önceliğe sahip olduğundan, kullanıcı trafiği şirket içi uygulamaya yönlendirilir. **Birincil** bitiş noktası sağlıklıysa, hiçbir trafik Azure'a yönlendirilir.

![Başarısız olmadan önce şirket içinde Azure'a](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

Bir olağanüstü durum durumunda, A Şirketi Azure'da bir [başarısızlığı](site-recovery-failover.md) tetikleyebilir ve uygulamalarını Azure'da kurtarabilir. Azure Trafik Yöneticisi **Birincil** bitiş noktasının artık sağlıklı olmadığını algıladığında, DNS yanıtındaki **Failover** bitiş noktasını otomatik olarak kullanır ve kullanıcılar Azure'da kurtarılan uygulamaya bağlanır.

![Başarısız olduktan sonra şirket içinde Azure'a](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

İşletme gereksinimlerine bağlı olarak, **Şirket A,** bir olağanüstü hal durumunda şirket içinde Azure arasında geçiş yapmak ve kullanıcılar için en az kapalı kalma süresini sağlamak için daha yüksek veya daha düşük bir [sondalama sıklığı](../traffic-manager/traffic-manager-monitoring.md) seçebilir.

Felaket kontrol altına alınınca, **A Şirketi** Azure'dan Azure Site Kurtarma'yı kullanarak şirket içi ortamına[(VMware](vmware-azure-failback.md) veya [Hyper-V)](hyper-v-azure-failback.md)geri dönebilir. Şimdi, Trafik Yöneticisi **Birincil** bitiş noktasının yeniden sağlıklı olduğunu algıladığında, DNS yanıtlarında **Birincil** bitiş noktasını otomatik olarak kullanır.

## <a name="on-premises-to-azure-migration"></a>Azure geçişiiçin şirket içi

Olağanüstü durum kurtarmanın yanı sıra, Azure Site Kurtarma da [Azure'a geçişyapılmasını](migrate-overview.md)sağlar. Müşteriler, Azure Site Kurtarma'nın güçlü test başarısızlık özelliklerini kullanarak, şirket içi ortamlarını etkilemeden Azure'daki uygulama performansını değerlendirebilir. Müşteriler geçiş yapmaya hazır olduklarında, tüm iş yüklerini birlikte geçirmeyi veya yavaş yavaş geçiş yapmayı ve ölçeklendirmeyi seçebilirler.

Azure Trafik Yöneticisi'nin [Ağırlıklı](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) yönlendirme yöntemi, gelen trafiğin bir kısmını Azure'a yönlendirirken çoğunluğu şirket içi ortama yönlendirmek için kullanılabilir. Bu yaklaşım, iş yüklerinizden daha fazlaını Azure'a geçirerken Azure'a atanan ağırlığı artırmaya devam edebildiğiniz için ölçek performansını değerlendirmenize yardımcı olabilir.

Örneğin, **B Şirketi** aşamalar halinde geçiş yapmayı seçer ve geri kalanını şirket içinde tutarken uygulama ortamının bir kısmını hareket ettirirken. Ortamın çoğunun şirket içinde olduğu ilk aşamalarda, şirket içi ortama daha büyük bir ağırlık atanır. Trafik yöneticisi, kullanılabilir uç noktalara atanan ağırlıkları temel alan bir bitiş noktası döndürür.

![Şirket içi-Azure geçişi](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Geçiş sırasında, her iki uç nokta da etkindir ve trafiğin çoğu şirket içi ortama yönlendirilir. Geçiş ilerledikçe, Azure'daki bitiş noktasına daha büyük bir ağırlık atanabilir ve son olarak şirket içi bitiş noktası geçiş sonrası devre dışı bırakılabilir.

## <a name="azure-to-azure-failover"></a>Azure'dan Azure'a başarısız

Bu örnekte, tüm uygulama altyapısı Azure'u çalıştıran **C Şirketini** göz önünde bulundurun. Şirket **C,** iş sürekliliği ve uyumluluk nedenleriyle uygulamalarını korumak için Azure Site Kurtarma'yı kullanmaya karar verir.

**C şirketi,** genel uç noktaları olan uygulamaları çalıştırıyor ve bir felaket durumunda trafiği sorunsuz bir şekilde farklı bir Azure bölgesine yönlendirme olanağı istiyor. [Öncelikli](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafik yönlendirme **yöntemi, C şirketinin** bu hata deseni kolayca uygulamasına olanak tanır.

Kurulum aşağıdaki gibidir:
- **C şirketi** bir [Trafik Yöneticisi profili](../traffic-manager/traffic-manager-create-profile.md)oluşturur.
- **Öncelikli** yönlendirme yöntemini kullanan **Şirket C,** kaynak bölge için **birincil** (Azure Doğu Asya) ve kurtarma bölgesi için **Failover** (Azure Güneydoğu Asya) olmak üzere iki uç nokta oluşturur. **Birincil** Öncelik 1 atanır ve **Failover** Öncelik 2 atanır.
- **Birincil** bitiş noktası Azure'da barındırılan olduğundan, bitiş noktası bir [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) bitiş noktası olarak olabilir.
- Azure Site Kurtarma ile kurtarma Azure sitesinde başarısız olmadan önce çalışan sanal makine veya uygulama yoktur. Bu nedenle, **Failover** bitiş noktası [Harici](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç nokta olarak oluşturulabilir.
- Varsayılan olarak, kullanıcı trafiği kaynak bölgeye (Doğu Asya) uygulamasına yönlendirilir, çünkü bu uç nokta onunla ilişkili en yüksek önceliğe sahiptir. **Birincil** bitiş noktası sağlıklıysa kurtarma bölgesine yönlendirilen trafik yoktur.

![Başarısız olmadan önce Azure'dan Azure'a](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

Bir felaket durumunda, **C şirketi** bir [başarısızlığı](azure-to-azure-tutorial-failover-failback.md) tetikleyebilir ve kurtarma Azure bölgesinde uygulamalarını kurtarabilir. Azure Trafik Yöneticisi Birincil bitiş noktasının artık sağlıklı olmadığını algıladığında, DNS yanıtındaki **Failover** bitiş noktasını otomatik olarak kullanır ve kullanıcılar kurtarma Azure bölgesinde (Güneydoğu Asya) kurtarılan uygulamaya bağlanır.

![Azure'dan Azure'a başarısız olduktan sonra](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

İşletme gereksinimlerine bağlı olarak, **C Şirketi** kaynak ve kurtarma bölgeleri arasında geçiş yapmak için daha yüksek veya daha düşük bir [sondalama frekansı](../traffic-manager/traffic-manager-monitoring.md) seçebilir ve kullanıcılar için en az kapalı kalma süresini sağlayabilir.

Felaket içerdiğinde, **C şirketi** Azure'u kurtarma Azure bölgesinden Azure Site Kurtarma'yı kullanarak kaynak Azure bölgesine geri dönebilir. Şimdi, Trafik Yöneticisi **Birincil** bitiş noktasının yeniden sağlıklı olduğunu algıladığında, DNS yanıtlarında **Birincil** bitiş noktasını otomatik olarak kullanır.

## <a name="protecting-multi-region-enterprise-applications"></a>Çok bölgeli kurumsal uygulamaları koruma

Küresel işletmeler genellikle bölgesel ihtiyaçlara hizmet etmek için uygulamalarını uyarlayarak müşteri deneyimini geliştirirler. Yerelleştirme ve gecikme süresi azaltma, uygulama altyapısının bölgeler arasında bölünmesine neden olabilir. İşletmeler de belirli alanlarda bölgesel veri yasalarına bağlıdır ve uygulama altyapılarının bir kısmını bölgesel sınırlar içinde yalıtmayı tercih ederler.  

**D şirketinin** uygulama bitiş noktalarını Almanya'ya ve dünyanın geri kalanına ayrı ayrı hizmet etmek için böldüğü bir örnek ele alalım. **Şirket D,** bunu ayarlamak için Azure Trafik Yöneticisi'nin [Coğrafi](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) yönlendirme yöntemini kullanır. Almanya menşeli tüm trafik **Endpoint 1'e** yönlendirilir ve Almanya dışından gelen tüm trafik **Endpoint 2'ye**yönlendirilir.

Bu kurulumdaki sorun, **Endpoint 1** herhangi bir nedenle çalışmayı durdurursa, trafiğin **Bitiş Noktası 2'ye**yeniden yönlendirilmesi nin olmamasıdır. Almanya'dan gelen trafik, bitiş noktasının sağlığına bakılmaksızın **Endpoint 1'e** yönlendirilmeye devam ederek Alman kullanıcıları **D Şirketinin**uygulamasına erişemekten mahrum bırakılmaktadır. Benzer şekilde, **Endpoint 2** çevrimdışı giderse, **Endpoint 1'e**trafik yeniden yönlendirmesi yoktur.

![Çok bölgeli uygulama önce](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Bu sorunla karşı laşmayı önlemek ve uygulama esnekliğini sağlamak için **Şirket D,** Azure Site Kurtarma ile [iç içe geçen Trafik Yöneticisi profillerini](../traffic-manager/traffic-manager-nested-profiles.md) kullanır. İç içe geçen profil kurulumunda trafik tek tek uç noktalara değil, diğer Trafik Yöneticisi profillerine yönlendirilir. Bu kurulum şu şekilde çalışır:
- **Şirket D,** coğrafi yönlendirmeyi tek tek uç noktalarla kullanmak yerine, Trafik Yöneticisi profilleriyle Coğrafi yönlendirme yi kullanır.
- Her alt Trafik Yöneticisi profili, birincil ve kurtarma bitiş noktasıyla **Öncelikli** yönlendirmeyi kullanır, bu nedenle **Coğrafi** yönlendirme içinde **Öncelik** yönlendirmesini iç içe geçme.
- Uygulama esnekliğini etkinleştirmek için, her iş yükü dağıtımı, bir olağanüstü durum durumunda bir kurtarma bölgesine geçememesi için Azure Site Kurtarma'yı kullanır.
- Üst Trafik Yöneticisi bir DNS sorgusu aldığında, sorguyu kullanılabilir bir bitiş noktasıyla yanıtlayan ilgili alt Trafik Yöneticisine yönlendirilir.

![Çok bölgeli uygulama dan sonra](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Örneğin, Almanya Merkezi'ndeki bitiş noktası başarısız olursa, uygulama hızla Almanya Kuzeydoğu'ya geri kazanılabilir. Yeni bitiş noktası, Kullanıcılar için en az kapalı kalma süresiyle Almanya'dan gelen trafiği işler. Benzer şekilde, Batı Avrupa'daki bir uç nokta kesintisi, uygulama iş yükünü Kuzey Avrupa'ya kurtararak işlenebilir ve Azure Trafik Yöneticisi'nin DNS işlemesi kullanılabilir bitiş noktasına yönlendirir.

Yukarıdaki kurulum, gereken çok sayıda bölge ve uç nokta birleşimini içerecek şekilde genişletilebilir. Trafik Yöneticisi iç içe profiller en fazla 10 düzeyleri sağlar ve iç içe yapılandırma içinde döngüler izin vermez.

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma Süresi Hedefi (RTO) hususları

Çoğu kuruluşta, DNS kayıtlarının eklenmesi veya değiştirilmesi ayrı bir ekip veya kuruluş dışındaki biri tarafından gerçekleştirilir. Bu, DNS kayıtlarını değiştirme görevini çok zorlaştırır. DNS altyapısını yöneten diğer ekipler veya kuruluşlar tarafından DNS kayıtlarını güncelleştirmek için geçen süre kuruluştan kuruluşa değişir ve uygulamanın RTO'su etkiler.

Trafik Yöneticisi'ni kullanarak, DNS güncelleştirmeleri için gereken işi ön yükleme yapabilirsiniz. Gerçek hata sırasında el ile veya komut dosyasıyla yazılmış bir eylem gerekmez. Bu yaklaşım, hızlı anahtarlama (ve dolayısıyla RTO'nun düşürülmesi) yanı sıra bir felaket olayında maliyetli zaman alan DNS değişiklik hatalarından kaçınmaya yardımcı olur. Traffic Manager ile, aksi takdirde ayrı ayrı yönetilmesi gerekir başarısız adım otomatiktir.

Temel veya hızlı aralıklı sistem durumu denetimleri aracılığıyla doğru [sondalama aralığının](../traffic-manager/traffic-manager-monitoring.md) ayarlanması, başarısızlık sırasında RTO'yu önemli ölçüde düşürebilir ve kullanıcılar için kapalı kalma süresini azaltabilir.

Ayrıca Trafik Yöneticisi profili için DNS Time to Live (TTL) değerini de optimize edebilirsiniz. TTL, DNS girişinin istemci tarafından önbelleğe alınacağı değerdir. Bir kayıt için, DNS TTL süresi içinde iki kez sorgulanmaz. Her DNS kaydının bir TTL'si vardır. Bu değeri azaltmak, Trafik Yöneticisi'ne daha fazla DNS sorgusu yla sonuçlanır, ancak kesintileri daha hızlı keşfederek RTO'u azaltabilir.

İstemci ile yetkili DNS sunucusu arasındaki DNS çözümleyicilerinin sayısı artarsa, istemcinin yaşadığı TTL de artmaz. DNS çözümleyicileri TTL'yi 'geri sayar' ve yalnızca kaydın önbelleğe geçirilmesinden bu yana geçen süreyi yansıtan bir TTL değerini aktarın. Bu, zincirdeki DNS Çözümleyicilerinin sayısına bakılmaksızın, TTL'den sonra istemcide DNS kaydının yenilenmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Trafik Yöneticisi [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.
- [İç içe trafik yöneticisi profilleri](../traffic-manager/traffic-manager-nested-profiles.md)hakkında daha fazla bilgi edinin.
- [Uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md)hakkında daha fazla bilgi edinin.
- Uygulama başarısızlığını otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.
