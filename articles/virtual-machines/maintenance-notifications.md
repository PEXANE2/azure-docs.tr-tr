---
title: Bakım bildirimleri
description: Azure'da çalışan sanal makineler için bakım bildirimlerine genel bakış.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 68159577cb31145be5063bb19af6db71ca1727bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77115688"
---
# <a name="handling-planned-maintenance-notifications"></a>Planlı bakım bildirimlerinin işlenmesi

Azure sanal makine konak altyapısının güvenilirlik, performans ve güvenliğini iyileştirmek için düzenli olarak güncelleştirmeler yapar. Güncelleştirmeler, barındırma ortamını düzeltme veya donanımı yükseltme ve devre dışı bırakma gibi değişikliklerdir. Bu güncelleştirmelerin çoğu barındırılan sanal makinelere herhangi bir etki olmadan tamamlanır. Ancak, güncelleştirmelerin bir etkisi olduğu durumlar vardır:

- Bakım yeniden başlatma gerektirmiyorsa, Azure ana bilgisayar güncelleştirilirken VM'yi duraklatmak için yerinde geçiş kullanır. Bu tür bakım işlemleri hata etki alanı tarafından hata etki alanı uygulanır. Herhangi bir uyarı durumu sinyalleri alınırsa ilerleme durdurulur.

- Bakım yeniden başlatma gerektiriyorsa, bakımın ne zaman planlandığına dikkat edin. Yaklaşık 35 günlük bir zaman dilimi verilir.


Yeniden başlatma gerektiren planlı bakım dalgalar halinde zamanlanır. Her dalganın farklı kapsamı (bölgeleri) vardır.

- Bir dalga müşterilere bir bildirim ile başlar. Varsayılan olarak, bildirim Hizmet Yöneticisi ve Yardımcı Yöneticilere gönderilir. [Etkinlik Günlüğü Uyarıları'nı](../service-health/alerts-activity-log-service-notifications.md)kullanarak e-posta, SMS ve webhooks gibi daha fazla alıcı ve mesajlaşma seçeneği ekleyebilirsiniz.  
- Bir bildirim çıktıktan sonra, *self servis penceresi* kullanıma hazır hale getirilir. Bu pencere sırasında, hangi sanal makinenizin etkilendiğini sorgulayabilir ve kendi zamanlama gereksinimlerinize göre bakıma başlayabilirsiniz. Self servis penceresi genellikle yaklaşık 35 gündür.
- Self servis penceresinden *sonra, zamanlanmış* bir bakım penceresi başlar. Bu pencere sırasında bir noktada, Azure sanal makinenize gerekli bakımı planlar ve uygular. 

İki pencereye sahip olmanın amacı, Azure'un otomatik olarak bakıma ne zaman başlayacağını bilerek bakım başlatmak ve sanal makinenizi yeniden başlatmak için yeterli zaman tanımaktır.


Azure portalını, PowerShell'i, REST API'sını ve CLI'yi kullanarak VM'lerinizin bakım pencerelerini sorgulayabilir ve self servis bakıma başlayabilirsiniz.

 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Self servis sırasında bakıma başlamalı mısın?  

Aşağıdaki yönergeler, bu özelliği kullanıp kullanmayacağınıza ve bakımı kendi zamanında başlatıp başlatmayacağınıza karar vermenize yardımcı olacaktır. 

> [!NOTE] 
> Self servis bakım tüm VM'leriniz için kullanılamayabilir. VM'niz için proaktif yeniden dağıtmanın kullanılabilir olup olmadığını belirlemek için bakım durumunda **Şimdi Başlat'ı** arayın. Bulut Hizmetleri (Web/İşçi Rolü) ve Servis Kumaşı için self servis bakım şu anda kullanılamıyor.


**Kullanılabilirlik kümelerini**kullanarak dağıtımlar için self servis bakım önerilmez. Kullanılabilirlik kümeleri zaten aynı anda yalnızca bir güncelleştirme etki alanı güncelleştirildi. 

- Azure'un bakımı tetiklesin. Yeniden başlatmayı gerektiren bakım için, bakım, etki alanını güncelleştirme alanına göre yapılır. Güncelleştirme etki alanları, bakım adlarını sırayla almaz ve güncelleştirme etki alanları arasında 30 dakikalık bir duraklama vardır. 
- Bazı kapasite (1 güncelleştirme etki alanı) geçici bir kayıp bir sorun ise, bakım döneminde örnekleri ekleyebilirsiniz. 
- Yeniden başlatma gerektirmeyen bakım için, güncelleştirmeler hata etki alanı düzeyinde uygulanır. 

**Don't** Aşağıdaki senaryolarda self servis bakım kullanmayın: 
- DevTest Labs kullanarak, otomatik kapatma kullanarak veya bir zamanlamayı izleyerek VM'lerinizi sık sık ,el ile kapatırsanız, bakım durumunu tersine çevirebilir ve bu nedenle ek kapalı kalma süresine neden olabilir.
- Bildiğiniz kısa ömürlü VM'lerde bakım dalgası sona ermeden önce silinir. 
- Güncelleştirme üzerine tutulması istenen yerel (kısa ömürlü) diskte depolanan büyük bir durum olan iş yükleri için. 
- Bakım durumunu geri alabileceğini zim'inizi sık sık yeniden boyutlandırdığınız durumlarda. 
- İş yükünüzün proaktif olarak başarısız olmasını veya zarif bir şekilde kapatılmasını sağlayan zamanlanmış olayları benimsediyseniz, bakım kapatma işleminin başlamasından 15 dakika önce

VM'inizi planlanan bakım aşamasında kesintisiz çalıştırmayı planlıyorsanız ve yukarıda belirtilen karşı göstergelerin hiçbiri geçerli değilse self servis bakımı **kullanın.** 

Aşağıdaki durumlarda self servis bakım kullanmak en iyisidir:
- Yönetiminize veya son müşterinize tam bir bakım penceresi iletmeniz gerekir. 
- Belirli bir tarihe kadar bakım tamamlamak gerekir. 
- Güvenli kurtarmayı garanti etmek için çok katmanlı uygulama gibi bakım sırasını denetlemeniz gerekir.
- İki güncelleştirme etki alanı (UDs) arasında 30 dakikadan fazla VM kurtarma süresi gerekir. Güncelleştirme etki alanları arasındaki süreyi denetlemek için, VM'lerinizdeki bir güncelleştirme etki alanınızdaki (UD) bakımı aynı anda tetiklemeniz gerekir.


## <a name="faq"></a>SSS


**S: Neden şimdi benim sanal makineleri yeniden başlatmanız gerekiyor?**

**A:** Azure platformundaki güncelleştirmelerin ve yükseltmelerin çoğu sanal makinenin kullanılabilirliğini etkilemese de, Azure'da barındırılan sanal makineleri yeniden başlatmaktan kaçınamadığımız durumlar vardır. Sanal makinelerin yeniden başlatılmasıyla sonuçlanacak sunucularımızı yeniden başlatmamızı gerektiren çeşitli değişiklikler yaptık.

**S: Kullanılabilirlik Kümesi'ni kullanarak Yüksek Kullanılabilirlik için önerilerinizi uygularsam, güvende miyim?**

**A:** Kullanılabilirlik kümesinde veya sanal makine ölçek kümelerinde dağıtılan sanal makineler, Etki Alanlarını Güncelleştir (UD) kavramına sahiptir. Azure, bakım yaparken UD kısıtlamasını onurlandırz ve sanal makineleri farklı UD'den (aynı kullanılabilirlik kümesi içinde) yeniden başlatmaz.  Azure, bir sonraki sanal makine grubuna geçmeden önce en az 30 dakika bekler. 

Yüksek kullanılabilirlik hakkında daha fazla bilgi için [Azure'daki sanal makineler için Kullanılabilirlik'e](./linux/availability.md)bakın.

**S: Planlı bakım hakkında nasıl bilgilendirilirim?**

**A:** Planlanmış bir bakım dalgası, bir veya daha fazla Azure bölgesine zamanlama ayarlayarak başlar. Kısa bir süre sonra, abonelik Yöneticileri'ne (abonelik başına bir e-posta) bir e-posta bildirimi gönderilir. Bu bildirimiçin ek kanallar ve alıcılar Etkinlik Günlüğü Uyarıları kullanılarak yapılandırılabilir. Sanal bir makineyi planlanan bakımın zaten zamanlandığı bir bölgeye dağıtırsanız, bildirimi almazsınız, bunun yerine VM'nin bakım durumunu denetlemeniz gerekir.

**S: Portalda, Powershell'de veya CLI'de planlı bakım olduğuna dair herhangi bir belirti göremiyorum. Ne oldu?**

**A:** Planlı bakımla ilgili bilgiler, yalnızca bundan etkilenecek olan VM'ler için planlanan bir bakım dalgası sırasında kullanılabilir. Başka bir deyişle, veri görmüyorsanız, bakım dalgası nın tamamlanmış (veya başlatılmadı) veya sanal makinenizin güncelleştirilmiş bir sunucuda zaten barındırılması olabilir.

**S: Sanal makinemin tam olarak ne zaman etkileneceğini bilmenin bir yolu var mı?**

**A:** Zamanlamayı ayarlarken, birkaç günlük bir zaman penceresi tanımlarız. Ancak, bu penceredeki sunucuların (ve VM'lerin) tam sıralaması bilinmemektedir. Sanal M'lerinin tam saatini öğrenmek isteyen müşteriler, sanal makinenin içinden [zamanlanmış olayları](./linux/scheduled-events.md) ve sorguyu kullanabilir ve VM yeniden başlatmadan önce 15 dakikalık bir bildirim alabilir.

**S: Sanal makinemi yeniden başlatmanız ne kadar sürer?**

**A:**  VM'nizin boyutuna bağlı olarak, self servis bakım penceresi sırasında yeniden başlatma birkaç dakika kadar sürebilir. Zamanlanan bakım penceresinde başlatılan Azure yeniden başlatma sırasında, yeniden başlatma genellikle yaklaşık 25 dakika sürer. Bulut Hizmetleri (Web/İşçi Rolü), Sanal Makine Ölçek Kümeleri veya kullanılabilirlik kümeleri kullanıyorsanız, zamanlanan bakım penceresi sırasında her grup VM (UD) arasında size 30 dakika verildiğini unutmayın.

**S: Sanal Makine Ölçek Setleri durumunda deneyim nedir?**

**A:** Sanal Makine Ölçek Setleri için planlı bakım artık kullanılabilir. Self servis bakım başlatılma yönergeleri [için sanal makine ölçeği setleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-maintenance-notifications.md) belge için planlanan bakım bakın.

**S: Bulut Hizmetleri (Web/İşçi Rolü) ve Hizmet Kumaşı durumunda ki deneyim nedir?**

**A:** Bu platformlar planlanan bakımdan etkilenirken, tek bir Yükseltme Etki Alanı'ndaki (UD) yalnızca VM'lerin herhangi bir zamanda etkileneceği göz önüne alındığında, bu platformları kullanan müşteriler güvenli kabul edilir. Bulut Hizmetleri (Web/İşçi Rolü) ve Servis Kumaşı için self servis bakım şu anda kullanılamıyor.

**S: VM'lerimde herhangi bir bakım bilgisi görmüyorum. Ters giden neydi?**

**A:** VM'lerinizde herhangi bir bakım bilgisi görmemenizin birkaç nedeni vardır:
1.  Microsoft dahili olarak işaretlenmiş bir abonelik kullanıyorsunuz.
2.  VM'leriniz bakım için zamanlanmadı. Bakım dalgası sona ermiş, iptal edilmiş veya değiştirilmiş olabilir, böylece VM'leriniz bundan etkilenmez.
3.  VM liste görünümünüze **Bakım** sütunu eklenmez. Bu sütunu varsayılan görünüme eklemiş olsak da, varsayılan olmayan sütunları görmek için yapılandırılan müşterilerin **Bakım** sütununa VM liste görünümüne el ile eklemeleri gerekir.

**S: VM'im ikinci kez bakım için zamanlandı. Neden?**

**A:** Bakım yeniden dağıtmanızı tamamladıktan sonra VM'nizin bakım için zamanlandığını göreceğiniz birkaç kullanım nedeni vardır:
1.  Bakım dalgasını iptal ettik ve farklı bir yük ile yeniden başlattık. Hatalı yük tespit etmiş olabiliriz ve ek bir yük dağıtmamız gerekiyor.
2.  VM'niz, bir donanım hatası nedeniyle başka bir düğüme *iyileşti.*
3.  VM'yi durdurmayı (anlaşma yapmayı) ve yeniden başlatmayı seçtiniz.
4.  VM için **otomatik kapatma** yı açtınız.



## <a name="next-steps"></a>Sonraki adımlar

Azure [CLI, Azure](maintenance-notifications-cli.md) [PowerShell](maintenance-notifications-powershell.md) veya [portalını](maintenance-notifications-portal.md)kullanarak planlı bakımı işleyebilirsiniz.

