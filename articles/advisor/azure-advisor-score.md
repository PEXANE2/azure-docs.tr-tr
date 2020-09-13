---
title: Danışman puanı kullanarak Azure iş yüklerini iyileştirin
description: Azure 'dan en iyi şekilde yararlanmak için danışman puanı kullanın
ms.topic: article
ms.date: 09/09/2020
ms.openlocfilehash: c9e6a13b39ac3436a71b6cecee4c91c609589378
ms.sourcegitcommit: 3fc3457b5a6d5773323237f6a06ccfb6955bfb2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90039097"
---
# <a name="advisor-score-to-get-the-most-out-of-azure"></a>Azure 'dan en iyi şekilde yararlanmak için danışman puanı

## <a name="introduction-to-advisor-score"></a>Danışman puanına giriş

Azure Advisor, iş yükleriniz için en iyi yöntem önerilerini sağlar. Bu öneriler kişiselleştirilmiş ve size yardımcı olmaya yönelik eylem yapılabilir:
* İş yüklerinizin duruşunu iyileştirin ve Azure dağıtımlarınızı iyileştirin
* En iyi uygulamaları takip ederek en iyi sorunları önleyici şekilde engelleyin
* Azure iş yüklerinizi [Microsoft Azure Iyi tasarlanmış Framework](https://docs.microsoft.com/azure/architecture/framework/) 'ün beş anına karşı değerlendirin

Advisor 'ın temel bir özelliği olarak, **danışman puanı** bu hedeflere etkin ve verimli bir şekilde ulaşmanıza yardımcı olacak şekilde denetimli hale getirilmelidir. 

Azure 'dan en iyi şekilde yararlanmak için, iş yükünün en iyi duruma getirme yolculuğunda nerede olduğunu, hangi hizmetlerin/kaynakların iyi tüketiğini ve hangilerinin hangilerinin olduğunu anlamanız çok önemlidir. Ayrıca, önerileri temel alarak eylemlerinizin önceliklerini en üst düzeye çıkarmak için nasıl önceliklendireceğimizi öğrenmek isteyeceksiniz. Bu en iyi duruma getirme yolculuğunda yaptığınız ilerlemeyi izlemek ve raporlamak de önemlidir. **Danışman puanı**sayesinde, her şeyi yeni bir kumla deneyimimize göre kolayca yapabilirsiniz. Kişiselleştirilmiş bulut danışmanınızın yanı sıra Azure Advisor, sektördeki en iyi uygulamaları denetlemek için kullanım telemetrinizi ve kaynak yapılandırmanızı sürekli olarak değerlendirir. Daha sonra danışman, bulgularını tek bir puana toplayarak, güvenilir, güvenli ve uygun maliyetli çözümler oluşturmak için gerekli adımları göz önünde bulundurularak anlayabilirsiniz. Danışman puanı, her bir Azure Advisor kategorisi için bir tane olmak üzere, doğru şekilde tasarlanmış olan Framework 'ün beş ve sonraki bir çerçevede daha fazla ayrılabilir olan genel bir puandan oluşur. Günlük, haftalık ve aylık eğilim ile genel puan ve kategori puanınızı görüntüleyerek zaman içinde yaptığınız ilerlemeyi izleyebilirsiniz ve hedeflerinizi elde etmenize yardımcı olması için kıyaslamalar belirleyebilirsiniz. 

 ![Danışman puan deneyimi](./media/advisor-score-1.png)

## <a name="how-to-consume-advisor-score"></a>Danışman Puanını kullanma
Advisor, danışman kategorilerinin Genel Danışman puanınızı ve dökümünü yüzde cinsinden görüntüler. Herhangi bir kategoride %100 puanı, Advisor tarafından değerlendirilen tüm kaynaklarınızın, Advisor 'ın önerdiği en iyi uygulamaları izlediği anlamına gelir. Spektrumun diğer ucunda, %0 puanı, hiçbir kaynakınızın hiçbir kaynak tarafından, Advisor 'ın önerilerini izlemediği anlamına gelir. Bu puan ve bu puanı kullanarak aşağıdaki akışa kolayca ulaşabilirsiniz:
* İş yükünüzün/aboneliklerinizin danışman puanına göre nasıl çalıştığını temel almanıza yardımcı olacak **danışman puanı** . Eğilimin ne olduğunu anlamak için geçmiş eğilimlerini de görebilirsiniz.
* **Danışman kategori puanları** hangi kategorilerin daha fazla ilgilenilmesi gerektiğini anlamanıza yardımcı olur ve önceliklendirmenize yardımcı olur
* Her bir kategorinin düzeltme eylemlerinizi önceliklendirmenize yardımcı olmak için her bir önerinin **olası puanı artışı**

Puanınıza her bir önerinin katkısı, Azure portal genel bakış sayfasında net bir şekilde gösterilir. En iyi uygulamaları benimseerek Puanınızı artırabilir ve en yüksek **puan artışına** sahip olan önerilerin önceliklerini, sahip olduğunuz zamana göre en hızlı ilerlemeyi elde edebilirsiniz.  

![Danışman puan etkisi](./media/advisor-score-2.png)

Advisor 'ın Puanlama yöntemi uzun süreli önerilere sahip daha pahalı kaynaklara ek ağırlık uyguladığı için, en yüksek perakende maliyetiyle kaynakları inceleyerek en son ilerlemeyi yapabilirsiniz. Herhangi bir danışman önerisi tek bir kaynakla ilgili değilse, puan hesaplamasından dışlamak ve önerilerini geliştirmek üzere Danışmanya geri bildirim göndermek için bu önerileri ortadan kaldırabilirsiniz. 

## <a name="how-is-advisor-score-calculated"></a>Danışman puanı nasıl hesaplanır?
Danışman kategori puanlarınızı ve genel danışman puanınızı yüzde olarak görüntüler. Herhangi bir kategoride %100 puanı, *Advisor tarafından*değerlendirilen tüm kaynaklarınızın, Advisor 'ın önerdiği en iyi uygulamaları izlediği anlamına gelir. Spektrumun diğer ucunda, %0 puanı, hiçbir kaynakınızın değil danışman tarafından değerlendirilen, danışman 'nin önerilerini izlemediği anlamına gelir. 
**Beş kategorinin her biri 100 olan en yüksek puanı.** Genel Danışman puanınız, uygulanabilir her bir kategori puanının toplamı olarak hesaplanır. Bu, tüm uygulanabilir kategorilerden en yüksek potansiyel puanın toplamına bölünür. Çoğu abonelik için, Advisor her kategoriden puan ekler ve 500 'e böler. Ancak, **her kategori puanı yalnızca Advisor tarafından değerlendirilen kaynakları kullanıyorsanız hesaplanır.**

### <a name="scoring-methodology"></a>Puanlama yöntemi: 
Danışman puanı hesaplaması dört adımda özetlenebilir:
1. Danışman, danışman 'de en az bir önerisine sahip olan aboneliklerinizde bulunan kaynaklar olan **etkilenen kaynakların günlük perakende maliyetini**hesaplar.
2. Danışman, öneriler tarafından izlenen kaynaklar olan, her öneri olup olmadığına bakılmaksızın, değerlendirilen **kaynakların günlük perakende maliyetini**hesaplar. 
3. Her öneri türü için, danışman, etkilenen kaynakların maliyeti değerlendirilmiş kaynakların maliyeti kadar olan **sağlıklı kaynak oranını**hesaplar.
4. Danışman her kategoride sağlıklı kaynak oranına üç ek ağırlık uygular:
* Daha büyük etkilerle öneriler, daha düşük etkilerle kıyasla daha ağır bir şekilde ağırlıklı olur.
* Uzun süreli önerilere sahip kaynaklar puanınızdan daha fazla sayacaktır.
* Advisor 'da çıkarılmanıza neden olan kaynaklar, puanınızın hesaplamasından tamamen kaldırılır. 
    
Danışman bu modeli bir danışman kategori düzeyinde uygular (güvenlik, [güvenli puan](https://docs.microsoft.com/azure/security-center/secure-score-security-controls#introduction-to-secure-score) modeli kullanır), her kategori Için bize danışman puanı vermek ve bir basit ortalama, son danışman Puanını üretir.


## <a name="advisor-score-faq"></a>Danışman puanı SSS
* **Puanım ne sıklıkta yenilenir?**
Puanınız günde en az bir kez yenilenir. 
* **Puanma yönelik puan almak için Advisor 'daki önerileri görüntülemesi gerekir mi?**
Hayır. Puanınız, bu önerileri Advisor 'da hiçbir şekilde görüntülemeniz ve en iyi yöntemleri önceden benimsemeniz durumunda bile, Advisor 'ın önerdiği en iyi uygulamaları benimsemenizi gösterir.  
* **Danışman bir abonelikteki kaynakların günlük perakende maliyetini nasıl hesaplar?**
Danışman, Azure.com fiyatlandırma sayfasında yayınlanan *Kullandıkça Öde* tarifesine, geçerli indirimleri yansıtmayan, kaynağın ayrıldığı son gün üzerindeki kullanım miktarına çarpılarak, bu işlemi kullanır. Kaynak maliyeti hesaplamasından indirimleri hariç tutma, danışman 'nin indirimlerde farklılık gösterebileceği abonelikler, kiracılar ve kayıtlar arasında karşılaştırılabilir olmasını sağlar. 
* **Öneri ilgili değilse ne olur?**
Danışmandan bir öneriyi atlarsanız puanınızın hesaplanmasıyla atlanacaktır. Eksik önerilerin yanı sıra önerilerin kalitesini artırmaya yardımcı olur.
* **Puanım Neden değişsin?** Etkilenen kaynakları, Advisor 'ın önerdiği en iyi uygulamaları benimseerek düzeltdiğinizde, puan verebilirsiniz. Aboneliğiniz ya da aboneliğinizdeki izinlere sahip olan herkes yeni kaynaklar değiştirirse ya da oluşturulmuşsa, puanınız, tüm kaynakların toplam maliyetine göre etkilenen maliyet kaynakları oranını temel aldığı için puanınızdaki dalgalanmaları de görebilirsiniz.
* **Puanım, Azure 'da ne kadar harcadığım hakkında bağlıdır?**
Hayır. Puan, bir abonelik ve hizmet karışımı boyutunu denetlemek için tasarlanmıştır. 
* **Puanlama yöntemi üretim ile geliştirme ve test iş yüklerini birbirinden ayırt etmektedir mi?**
Hayır, şimdilik değil, ancak bu kaynaklar geliştirme ve test için kullanılıyorsa ve öneri uygulanmadığında tek tek kaynaklardaki önerileri kapatabilirsiniz.
* **100 kaynakla bir abonelik ile 100.000 kaynakla bir abonelik arasındaki puanları karşılaştırabilir miyim?**
Puanlama yöntemi bir abonelik ve hizmet karışımındaki kaynak sayısını denetlemek için tasarlanmıştır. bu nedenle, daha az kaynağa sahip abonelikler daha fazla kaynağa sahip olan aboneliklerden daha yüksek veya daha düşük puan elde edebilir. 

## <a name="how-to-access-advisor"></a>Advisor 'a erişme
Danışman puanı, Azure portal genel önizlemededir. Danışman bölümüne gitmeniz gerekir ve sol gezinti bölmesinde 2. menü öğesi olarak danışman puanı bulacaksınız. 

![Danışman puan giriş noktası](./media/advisor-score-3.png)

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz.
* [Advisor 'a giriş](advisor-overview.md)
* [Danışman'ı kullanmaya başlama](advisor-get-started.md)
* [Danışman maliyet önerileri](advisor-cost-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Danışman güvenlik önerileri](advisor-security-recommendations.md)
* [Danışman işletimsel üstün öneriler](advisor-operational-excellence-recommendations.md)
