---
title: Azure Site Recovery ile Azure Traffic Manager | Microsoft Docs
description: Olağanüstü durum kurtarma ve geçiş için Azure Site Recovery ile Azure Traffic Manager 'in nasıl kullanılacağını açıklar
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: mayg
ms.openlocfilehash: 6c77cd43231d4596535c11564313a0fe90633cdb
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "60947819"
---
# <a name="azure-traffic-manager-with-azure-site-recovery"></a>Azure Site Recovery ile Azure Traffic Manager

Azure Traffic Manager, uygulama uç noktalarınız genelinde trafiğin dağıtımını denetlemenize olanak sağlar. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Traffic Manager, istemci isteklerini bir trafik yönlendirme yöntemine ve uç noktaların sistem durumuna bağlı olarak en uygun uç noktaya yönlendirmek için etki alanı adı sistemi 'ni (DNS) kullanır. Traffic Manager, farklı uygulama ihtiyaçlarına ve otomatik yük devretme modellerine uyan farklı [trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md) ve [uç nokta izleme seçenekleri](../traffic-manager/traffic-manager-monitoring.md) sunar. İstemciler doğrudan seçilen uç noktaya bağlanır. Traffic Manager bir proxy veya ağ geçidi değil ve istemci ile hizmet arasında geçen trafiği görmez.

Bu makalede, Azure Trafik Izleyicisi 'nin akıllı yönlendirmesini Azure Site Recovery güçlü olağanüstü durum kurtarma ve geçiş özellikleri ile nasıl birleştirebileceğiniz açıklanır.

## <a name="on-premises-to-azure-failover"></a>Şirket içinden Azure 'a yük devretme

İlk senaryo için, şirket içi ortamda çalışan tüm uygulama altyapısına sahip olan **şirkete** göz önünde bulundurun. **Şirket A** , iş sürekliliği ve uyumluluk nedenleriyle, uygulamalarını korumak için Azure Site Recovery kullanmaya karar veriyor.

**A şirketi** ortak uç noktalara sahip uygulamalar çalıştırıyor ve bir olağanüstü durum olayında trafiği sorunsuz bir şekilde Azure 'a yeniden yönlendirebilme olanağı istiyor. Azure Traffic Manager [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafiği yönlendirme yöntemi, Şirket A 'nın bu yük devretme modelini kolayca uygulamasına olanak tanır.

Kurulum aşağıdaki gibidir:
- **A şirketi** bir [Traffic Manager profili](../traffic-manager/traffic-manager-create-profile.md)oluşturur.
- **Öncelik** yönlendirme yönteminden yararlanarak, **Şirket A** iki uç nokta oluşturur – şirket Içi ve Azure Için **Yük devretme** için **birincil** . **Birincil** öncelik 1 olarak atanır ve **yük devretmeye** öncelik 2 atanır.
- **Birincil** uç nokta Azure dışında barındırıldığından, uç nokta bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç nokta olarak oluşturulur.
- Azure Site Recovery, Azure sitesinde yük devretmeden önce çalışan sanal makineler veya uygulamalar yok. Bu nedenle, **Yük devretme** uç noktası bir **dış** uç nokta olarak da oluşturulur.
- Varsayılan olarak, bu uç noktanın ilişkilendirildiği en yüksek önceliğe sahip olduğu için Kullanıcı trafiği şirket içi uygulamaya yönlendirilir. **Birincil** uç nokta sağlıklı ise Azure 'a hiçbir trafik yönlendirilmelidir.

![Yük devretmeden önce Şirket içinden Azure 'a](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-before.png)

Bir olağanüstü durum olayında, A şirketi Azure 'a [Yük devretme](site-recovery-failover.md) tetikleyip Azure 'da uygulamalarını kurtarabilir. Azure Traffic Manager **birincil** uç noktanın artık sağlıklı olmadığını algıladığında, OTOMATIK olarak DNS yanıtında **Yük devretme** uç noktasını kullanır ve kullanıcılar Azure 'da kurtarılan uygulamaya bağlanır.

![Yük devretmeden sonra Şirket içinden Azure 'a](./media/concepts-traffic-manager-with-site-recovery/on-premises-failover-after.png)

**Şirket A** , iş gereksinimlerine bağlı olarak, bir olağanüstü durum olayında şirket Içi ile Azure arasında geçiş yapmak için daha yüksek veya daha düşük bir [yoklama sıklığı](../traffic-manager/traffic-manager-monitoring.md) seçebilir ve kullanıcılar için en düşük kapalı kalma süresini garanti edebilir.

Olağanüstü durum dahil edildiğinde **Şirket A** , Azure Site Recovery kullanarak Azure 'dan şirket içi ortama ([VMware](vmware-azure-failback.md) veya [Hyper-V](hyper-v-azure-failback.md)) yeniden kullanılabilir. Artık Traffic Manager **birincil** uç noktanın sağlıklı olduğunu ALGıLADıĞıNDA, DNS yanıtlarındaki **birincil** uç noktayı otomatik olarak kullanır.

## <a name="on-premises-to-azure-migration"></a>Şirket içinden Azure 'a geçiş

Olağanüstü durum kurtarma 'ya ek olarak, Azure Site Recovery [Azure 'a geçişlere](migrate-overview.md)de olanak sağlar. Müşteriler, Azure Site Recovery güçlü yük devretme yeteneklerini kullanarak, şirket içi ortamlarını etkilemeden Azure 'da uygulama performansını değerlendirebilirler. Müşteriler geçişe hazırsanız, tüm iş yüklerini birlikte geçirmeyi seçebilir veya kademeli olarak geçiş ve ölçek ölçeği seçebilirsiniz.

Azure Traffic Manager [ağırlıklı](../traffic-manager/traffic-manager-configure-weighted-routing-method.md) yönlendirme yöntemi, büyük bir kısmı şirket içi ortama yönlendirirken, gelen trafiğin bir kısmını Azure 'a yönlendirmek için kullanılabilir. Bu yaklaşım, iş yüklerinizi daha fazla ve daha fazlasını Azure 'a geçirirken Azure 'a atanan ağırlığı artırmaya devam edebilmeniz için ölçek performansının değerlendirilmesine yardımcı olabilir.

Örneğin, **Şirket B** , Rest 'te geçiş yapmayı tercih ederken, Rest şirket içi tutarken uygulama ortamının bir kısmını taşır. Ortamın çoğunun şirket içi olması durumunda ilk aşamada, şirket içi ortama daha büyük bir ağırlık atanır. Traffic Manager, kullanılabilir uç noktalara atanan ağırlıkları temel alan bir uç nokta döndürür.

![Şirket içinden Azure 'a geçiş](./media/concepts-traffic-manager-with-site-recovery/on-premises-migration.png)

Geçiş sırasında her iki uç nokta de etkindir ve trafiğin çoğu şirket içi ortama yönlendirilir. Geçiş işlemi devam ederken, Azure 'daki uç noktaya daha büyük bir ağırlık atanabilir ve son olarak şirket içi uç noktanın geçiş sonrası devre dışı bırakılabilir.

## <a name="azure-to-azure-failover"></a>Azure 'dan Azure 'a yük devretme

Bu örnek için, Azure çalıştıran tüm uygulama altyapısına sahip olan **Şirket C** 'yi düşünün. İş sürekliliği ve uyumluluk nedenleriyle, **Şirket C** 'nin uygulamalarını korumak için Azure Site Recovery kullanmasına karar verir.

**Şirket C** , genel uç noktalara sahip uygulamalar çalıştırıyor ve bir olağanüstü durum olayında trafiği sorunsuz bir şekilde farklı bir Azure bölgesine yeniden yönlendirebilme olanağı istiyor. [Öncelik](../traffic-manager/traffic-manager-configure-priority-routing-method.md) trafiği-yönlendirme yöntemi, **Şirket C** 'nin bu yük devretme modelini kolayca uygulamasına olanak tanır.

Kurulum aşağıdaki gibidir:
- **Şirket C** bir [Traffic Manager profili](../traffic-manager/traffic-manager-create-profile.md)oluşturur.
- **Öncelik** yönlendirme yönteminin kullanıldığı **Şirket C** , kaynak bölge (Azure Doğu Asya) için **birincil** ve kurtarma bölgesi (Azure Güneydoğu Asya) için **Yük devretme** olmak üzere iki uç nokta oluşturur. **Birincil** öncelik 1 olarak atanır ve **yük devretmeye** öncelik 2 atanır.
- **Birincil** uç nokta Azure 'da barındırıldığından, uç nokta bir [Azure](../traffic-manager/traffic-manager-endpoint-types.md#azure-endpoints) uç noktası olabilir.
- Azure Site Recovery ile, kurtarma Azure sitesinde yük devretme öncesinde çalışan sanal makine veya uygulamalar yok. Bu nedenle, **Yük devretme** uç noktası bir [dış](../traffic-manager/traffic-manager-endpoint-types.md#external-endpoints) uç nokta olarak oluşturulabilir.
- Varsayılan olarak, bu uç noktanın ilişkilendirildiği en yüksek önceliğe sahip olduğu için Kullanıcı trafiği, kaynak bölge (Doğu Asya) uygulamasına yönlendirilir. **Birincil** uç nokta sağlıklı ise, kurtarma bölgesine hiçbir trafik yönlendirilmelidir.

![Yük devretmeden önce Azure 'dan Azure 'a](./media/concepts-traffic-manager-with-site-recovery/azure-failover-before.png)

Bir olağanüstü durum olayında, **Şirket C** , bir [Yük devretme](azure-to-azure-tutorial-failover-failback.md) tetikleyip uygulamalarını kurtarma Azure bölgesinde kurtarabilirler. Azure Traffic Manager birincil uç noktanın artık sağlıklı olmadığını algıladığında, otomatik olarak DNS yanıtında **Yük devretme** uç noktasını kullanır ve kullanıcılar kurtarma Azure bölgesinde (Güneydoğu Asya) kurtarılan uygulamaya bağlanır.

![Yük devretmeden sonra Azure 'dan Azure 'a](./media/concepts-traffic-manager-with-site-recovery/azure-failover-after.png)

**Şirket C** , iş gereksinimlerine bağlı olarak, kaynak ve kurtarma bölgeleri arasında geçiş yapmak için daha yüksek veya daha düşük bir [yoklama frekansı](../traffic-manager/traffic-manager-monitoring.md) seçebilir ve kullanıcılar için en düşük kapalı kalma süresini garanti edebilir

Olağanüstü durum dahil edildiğinde, **Şirket C** , kurtarma Azure bölgesinden kaynak azure bölgesine Azure Site Recovery kullanarak yeniden kullanılabilir. Artık Traffic Manager **birincil** uç noktanın sağlıklı olduğunu ALGıLADıĞıNDA, DNS yanıtlarındaki **birincil** uç noktayı otomatik olarak kullanır.

## <a name="protecting-multi-region-enterprise-applications"></a>Çok bölgeli kurumsal uygulamaları koruma

Küresel kuruluşlar, genellikle uygulamalarını bölgesel gereksinimlere uygun hale getirerek müşteri deneyimini geliştirir. Yerelleştirme ve gecikme süresi azaltma, bölgeler arasında bölünmüş uygulama altyapısına yol açabilir. Kuruluşlar aynı zamanda belirli alanlardaki bölgesel veri yasaları tarafından da bağlanır ve bir parçayı bölge sınırları içinde yalıtmak üzere tercih edilir.  

**Şirket D şirketinin** , uygulama uç noktalarını ayrı olarak Almanya 'ya ve dünyanın geri kalanına göre böldüğü bir örnek ele alalım. **Şirket D** , bunu ayarlamak için Azure Traffic Manager [coğrafi](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) yönlendirme yöntemini kullanır. Almanya 'dan kaynaklanan tüm trafik, **uç nokta 1** ' e yönlendirilir ve Almanya dışında gelen tüm trafik, **uç nokta 2**' ye yönlendirilir.

Bu kurulumla ilgili sorun, **uç nokta 1** her nedenden dolayı çalışmayı durdurduğundan, **uç nokta 2**' ye trafiğin yeniden yönlendirilmesi olmaz. Almanya 'dan kaynaklanan trafik, uç noktanın sistem durumu ne olursa olsun, uç **nokta 1** ' e yönlendirilmeye devam eder ve **Şirket D**'Nin uygulamasına erişimi olmadan Almanya kullanıcılarını bırakır. Benzer şekilde, **uç nokta 2** çevrimdışı olursa, **uç nokta 1**' e trafik yönlendirmesi olmaz.

![Daha önce çok bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-before.png)

Bu soruna karşı çalışmayı önlemek ve uygulama dayanıklılığı sağlamak için **Şirket D** , Azure Site Recovery birlikte [iç içe Traffic Manager profillerini](../traffic-manager/traffic-manager-nested-profiles.md) kullanır. İç içe geçmiş profil kurulumunda trafik, diğer Traffic Manager profillerine değil, tek tek uç noktalara yönlendirilmez. Bu kurulumun nasıl çalıştığı aşağıda verilmiştir:
- **Şirket D** , tek uç noktalarla coğrafi yönlendirmeyi kullanmak yerine Traffic Manager profilleriyle coğrafi yönlendirme kullanır.
- Her bir alt Traffic Manager profili, birincil ve kurtarma uç noktasıyla **Öncelik** yönlendirmeyi kullanır ve bu nedenle, **coğrafi** yönlendirme içinde **Öncelik** yönlendirmeyi iç içe geçirerek.
- Uygulama dayanıklılığı sağlamak için her iş yükü dağıtımı, bir olağanüstü durum olayı olması durumunda kurtarma bölgesine yük devretme Azure Site Recovery kullanır.
- Üst Traffic Manager bir DNS sorgusu aldığında, kullanılabilir bir uç nokta ile sorguya yanıt veren ilgili alt Traffic Manager yönlendirilir.

![Sonrasında çok bölgeli uygulama](./media/concepts-traffic-manager-with-site-recovery/geographic-application-after.png)

Örneğin, Almanya Orta uç noktası başarısız olursa, uygulama Almanya Kuzeydoğu hızlı bir şekilde kurtarılabilir. Yeni uç nokta, Almanya 'dan kaynaklanan trafiği, kullanıcılar için en az kapalı kalma süresiyle işler. Benzer şekilde Batı Avrupa bir uç nokta kesinti, Azure Traffic Manager, kullanılabilir uç noktaya DNS yeniden yönlendirmelerini işleyerek, uygulama iş yükünü Kuzey Avrupa kurtararak işlenebilirler.

Yukarıdaki kurulum, gereken sayıda bölge ve uç nokta kombinasyonu içerecek şekilde genişletilebilir. Traffic Manager, iç içe geçmiş profillerin 10 ' a kadar düzeyi sağlar ve iç içe yapılandırma içindeki döngülere izin vermez.

## <a name="recovery-time-objective-rto-considerations"></a>Kurtarma süresi hedefi (RTO) konuları

Çoğu kuruluşta, DNS kayıtlarını ekleme veya değiştirme işlemi ayrı bir ekip ya da kuruluşun dışında bir kişi tarafından işlenir. Bu, DNS kayıtlarını değiştirme görevini çok zorlayıcı hale getirir. DNS altyapısını yöneten diğer takımlar veya kuruluşlar tarafından DNS kayıtlarını güncelleştirmek için geçen süre, kuruluştan kuruluşa farklılık gösterir ve uygulamanın RTO 'ı etkiler.

Traffic Manager kullanarak, DNS güncelleştirmeleri için gereken işi önde yükleyebilirsiniz. Gerçek yük devretme sırasında el ile veya betikleştirilmiş bir eylem gerekli değildir. Bu yaklaşım, hızlı geçiş (ve bu nedenle RTO 'nin azaltmasına) ve bir olağanüstü durum olayında pahalı zaman alan DNS değişiklik hatalarını önlemeye yardımcı olur. Traffic Manager ile, yeniden çalışma adımı otomatik hale gelir, aksi takdirde ayrı olarak yönetilmelidir.

Doğru [yoklama aralığının](../traffic-manager/traffic-manager-monitoring.md) temel veya hızlı Aralık durum denetimleri aracılığıyla ayarlanması, yük devretme sırasında RTO 'ı önemli ölçüde azaltabilir ve kullanıcılar için kapalı kalma süresini azaltır.

Ayrıca, Traffic Manager profili için DNS yaşam süresi (TTL) değerini de iyileştirebilirsiniz. TTL, bir DNS girişinin bir istemci tarafından önbelleğe alınacağı değerdir. Bir kayıt için DNS, TTL 'nin kapsamı içinde iki kez sorgulanmaz. Her DNS kaydına ilişkili bir TTL vardır. Bu değeri azaltmak, Traffic Manager için daha fazla DNS sorgusu sağlar ancak kesintileri daha hızlı bir şekilde bularak RTO 'ı azaltabilir.

İstemci ile yetkili DNS sunucusu arasındaki DNS çözümleyiciler sayısı arttıkça istemci tarafından karşılaşılan TTL de artmaz. DNS Çözümleyenler ' i aşağı say ' ve yalnızca kayıt önbelleğe alındığı için geçen süreyi yansıtan bir TTL değerini geçti. Bu, zincirdeki DNS çözümleyicilerine bakılmaksızın, DNS kaydının, TTL 'den sonra istemcide yenilenmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar
- Traffic Manager [yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.
- [İç içe Traffic Manager profilleri](../traffic-manager/traffic-manager-nested-profiles.md)hakkında daha fazla bilgi edinin.
- [Uç nokta izleme](../traffic-manager/traffic-manager-monitoring.md)hakkında daha fazla bilgi edinin.
- Uygulama yük devretmesini otomatikleştirmek için [kurtarma planları](site-recovery-create-recovery-plans.md) hakkında daha fazla bilgi edinin.
