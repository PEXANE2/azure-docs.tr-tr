---
title: Azure Güvenlik Merkezi'nde güvenlik uyarıları | Microsoft Dokümanlar
description: Bu konu, güvenlik uyarılarının ne olduğunu ve Azure Güvenlik Merkezi'nde kullanılabilen farklı türleri açıklar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.topic: conceptual
ms.date: 03/15/2020
ms.author: memildin
ms.openlocfilehash: 697c038a2fefdde8e488dad23a4e38e0b2b7b288
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79415846"
---
# <a name="security-alerts-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik uyarıları

Azure Güvenlik Merkezi'nde, birçok farklı kaynak türü için çeşitli uyarılar vardır. Güvenlik Merkezi, Azure'da dağıtılan kaynaklar ve şirket içi ve karma bulut ortamlarında dağıtılan kaynaklar için uyarılar oluşturur.

Güvenlik uyarıları gelişmiş algılamalar tarafından tetiklenir ve yalnızca Azure Güvenlik Merkezi'nin Standart Katmanı'nda kullanılabilir. Ücretsiz deneme sürümü mevcuttur. [Güvenlik İlkesi](security-center-pricing.md) bölümündeki Fiyatlandırma Katmanı’ndan yükseltme yapabilirsiniz. Fiyatlandırma hakkında daha fazla bilgi almak için [Güvenlik Merkezi sayfasını](https://azure.microsoft.com/pricing/details/security-center/) ziyaret edin.

## <a name="responding-to-todays-threats"></a>Bugünün tehditlerine <a name="respond-threats"> </a> yanıt verme

Son 20 yılda tehdit kapsamında önemli değişiklikler olmuştur. Geçmişte, şirketler genellikle sadece çoğunlukla "ne yapabileceğini" görmek isteyen bireysel saldırganlar tarafından web sitesi tahrif endişe vardı. Bugünün saldırganları çok daha sofistike ve organize. Saldırganlar genellikle belirli finansal ve stratejik hedeflere sahiptir. Ayrıca, ulus devletler veya organize suç örgütleri tarafından finanse edilebildiklerinden ellerinde daha fazla kaynak bulunmaktadır.

Bu değişen gerçekler, saldırgan saflarında eşi görülmemiş bir profesyonellik düzeyine yol açmıştır. Saldırganlar artık web tahrifatı ile ilgilenmemektedir. Artık bilgi, finansal hesaplar ve özel verileri çalmakla ilgileniyorlar – bunların hepsi açık pazarda nakit oluşturmak veya belirli bir iş, politik veya askeri pozisyondan yararlanmak için kullanabilirler. Mali hedefleri olan saldırganlardan çok daha fazla endişe verici olan ise altyapıya ve insanlara zarar vermek için ağları ihlal eden saldırganlardır.

Buna yanıt olarak, kuruluşlar çoğunlukla bilinen saldırı imzalarını arayarak kurumsal çevrelerini veya uç noktalarını savunmaya odaklanan çeşitli nokta çözümleri dağıtmaktadır. Bu çözümler, güvenlik analizi uzmanının belirlemesini ve araştırmasını gerektiren yüksek miktarda düşük güvenilirlik uyarıları oluşturma eğilimindedir. Çoğu kurum bu uyarılara yanıt verecek zaman ve uzmanlığa sahip olmadığından birçoğu çözüme kavuşmamaktadır.  

Buna ek olarak, saldırganlar birçok imza tabanlı savunma yıkmak ve [bulut ortamlarına uyum](https://azure.microsoft.com/blog/detecting-threats-with-azure-security-center/)için kendi yöntemlerini geliştirdik. Ortaya çıkan tehditleri daha hızlı belirlemek ve algılama ile yanıtı hızlandırmak için yeni yaklaşımlar gereklidir.

## <a name="what-are-security-alerts-and-security-incidents"></a>Güvenlik uyarıları ve güvenlik olayları nelerdir? 

**Uyarılar,** Güvenlik Merkezi'nin kaynaklarınızdaki tehditleri algıladığında oluşturduğu bildirimlerdir. Güvenlik Merkezi, sorunu hızlı bir şekilde araştırmanız için gereken bilgilerle birlikte uyarıları n öncelir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler sağlar.

**Güvenlik olayı,** her uyarıyı ayrı ayrı listelemek yerine ilgili uyarılar topluluğudur. Güvenlik Merkezi, farklı uyarıları ve düşük doğruluk sinyallerini güvenlik olaylarına ilişkilendirmek için [Bulut Akıllı Uyarı Korelasyon'u](security-center-alerts-cloud-smart.md) kullanır.

Güvenlik Merkezi, olayları kullanarak bir saldırı kampanyasının ve ilgili tüm uyarıların tek bir görünümünü sağlar. Bu görünüm, saldırganın hangi eylemleri gerçekleştirdiğini ve hangi kaynakların etkilendiğini hızlı bir şekilde anlamanızı sağlar. Daha fazla bilgi için [Bulut akıllı uyarı korelasyonuna](security-center-alerts-cloud-smart.md)bakın.



## <a name="how-does-security-center-detect-threats"></a>Güvenlik Merkezi tehditleri nasıl algılar? <a name="detect-threats"> </a>

Microsoft güvenlik araştırmacıları sürekli olarak tehditleri araştırmaktadır. Microsoft'un bulutve şirket içi küresel varlığı sayesinde, geniş bir telemetri setine erişebilirler. Geniş kapsamlı ve çeşitli veri kümeleri koleksiyonu, şirket içi tüketici ve kurumsal ürünlerinin yanı sıra çevrimiçi hizmetlerinde yeni saldırı kalıplarının ve eğilimlerinin keşfedilmesini sağlar. Sonuç olarak, saldırganlar yeni ve giderek karmaşık hale gelen sömürülerini ortaya çıkardıkça Güvenlik Merkezi algılama algoritmalarını hızlı bir şekilde güncelleştirebilmektedir. Bu yaklaşık hızlı hareket eden bir ortama ayak uydurmanıza yardımcı olmaktadır.

Güvenlik Merkezi, gerçek tehditleri algılamak ve yanlış pozitifleri azaltmak için Azure kaynaklarınızdan ve ağınızdaki günlük verilerini toplar, analiz eder ve tümleştirir. Ayrıca, güvenlik duvarı ve uç nokta koruma çözümleri gibi bağlı iş ortağı çözümleriyle de çalışır. Güvenlik Merkezi, tehditleri tanımlamak için genellikle birden çok kaynaktan gelen bilgileri ilişkilendiren bu bilgileri analiz eder.

![Güvenlik Merkezi Veri toplama ve sunumu](./media/security-center-alerts-overview/security-center-detection-capabilities.png)

Güvenlik Merkezi, imza tabanlı yaklaşımların ötesine geçen gelişmiş güvenlik analizleri kullanır. Büyük veri ve [machine learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) teknolojilerindeki sıçramalar, tüm bulut yapısındaki olayları değerlendirmek için kullanılır: elle yaklaşımlar kullanılarak ve saldırıların gelişimi tahmin edilerek belirlenmesi imkansız olan tehditler algılanır. Bu güvenlik analizleri şunlardır:

* **Entegre tehdit istihbaratı**: Microsoft'un çok büyük miktarda küresel tehdit istihbaratı vardır. Telemetri, Azure, Office 365, Microsoft CRM çevrimiçi, Microsoft Dynamics AX, outlook.com, MSN.com, Microsoft Dijital Suçlar Birimi (DCU) ve Microsoft Güvenlik Yanıt Merkezi (MSRC) gibi birden çok kaynaktan akar. Araştırmacılar ayrıca, büyük bulut hizmeti sağlayıcıları arasında paylaşılan tehdit istihbaratı bilgilerini ve diğer üçüncü taraflardan gelen özet akışları da alırlar. Azure Güvenlik Merkezi bilinen kötü aktörlerden gelen tehditler konusunda sizi uyarmak için bu bilgileri kullanabilir.

* **Davranış analizi**: Davranış analizi, verileri bilinen desenler topluluğuyla analiz eden ve karşılaştıran bir tekniktir. Ancak, bu modeller basit imzalar değildir. Bunlar büyük veri kümelerine uygulanan karmaşık machine learning algoritmaları aracılığıyla belirlenir. Bunlar, kötü amaçlı davranışların uzman analistler tarafından dikkatlice çözümlenmesiyle de belirlenir. Azure Güvenlik Merkezi, sanal makine günlüklerinin, sanal ağ aygıtı günlüklerinin, kumaş günlüklerinin, kilitlenme dökümlerinin ve diğer kaynakların analizini temel alan, tehlikeye atılan kaynakları belirlemek için davranış analizini kullanabilir.

* **Anormallik algılama**: Azure Güvenlik Merkezi, tehditleri tanımlamak için anomali algılamayı da kullanır. Davranış salatiğinin (büyük veri kümelerinden elde edilen bilinen desenlere bağlı olarak) aksine, anomali algılaması daha "kişiselleştirilmiştir" ve dağıtımlarınıza özgü taban çizgilerine odaklanır. Dağıtımlarınızın normal etkinliğini belirlemek için machine learning uygulanır ve sonra bir güvenlik olayını gösterebilecek aykırı değer koşullarını tanımlamak üzere kurallar oluşturulur.

## <a name="how-are-alerts-classified"></a>Uyarılar nasıl sınıflandırılır?

Güvenlik Merkezi, her uyarıya katılma sırasının önceliklerini belirlemenize yardımcı olmak için uyarılara önem verir, böylece bir kaynağın gizliliği ihlal edildiğinde, bu uyarıya hemen ulaşabilirsiniz. Önem derecesi, Güvenlik Merkezi'nin bulguda ne kadar emin olduğuna veya uyarıyı vermek için kullanılan analize ve uyarıya yol açan etkinliğin arkasında kötü amaçlı niyet olduğuna dair güven düzeyine dayanır.

> [!NOTE]
> Uyarı şiddeti, 01-01-2019 tarihinden önceki REST API portalında ve sürümlerinde farklı şekilde görüntülenir. API'nin eski bir sürümünü kullanıyorsanız, aşağıda açıklanan tutarlı deneyimi yükseltin.

- **Yüksek:** Kaynağınızın tehlikeye girme olasılığı yüksektir. Hemen araştırmalısın. Güvenlik Merkezi hem kötü niyetli niyete hem de uyarıyı vermek için kullanılan bulgulara yüksek güvene sahiptir. Örneğin, kimlik bilgisi hırsızlığı için kullanılan yaygın bir araç olan Mimikatz gibi bilinen kötü amaçlı bir aracın yürütülmesini algılayan bir uyarı.
- **Orta:** Bu büyük olasılıkla şüpheli bir etkinlik, kaynağın ele geçirildiğini gösterebilir.
Güvenlik Merkezi'nin analitik veya bulguya olan güveni orta ve kötü niyetli niyetin güveni orta ve yüksektir. Bunlar genellikle makine öğrenimi veya anomali tabanlı algılamalar olacaktır. Örneğin, anormal bir konumdan bir oturum açma girişimi.
- **Düşük:** Bu iyi huylu bir pozitif veya engellenmiş bir saldırı olabilir.
   * Güvenlik Merkezi, niyetin kötü niyetli olduğundan ve etkinliğin masum olabileceğinden yeterince emin değildir. Örneğin, günlük açık bir saldırgan izlerini gizlemeye çalıştığında meydana gelebilecek bir eylemdir, ancak çoğu durumda yöneticiler tarafından gerçekleştirilen rutin bir işlemdir.
   * Güvenlik Merkezi, size genellikle saldırıların ne zaman engellendiğini söylemez. 
- **Bilgilendirme amaçlı bilgiler:** Yalnızca bir güvenlik olayına baktığınızda veya rest API'yi belirli bir uyarı kimliğiyle kullandığınızda bilgilendirme uyarıları görürsünüz. Bir olay genellikle bir dizi uyarıdan oluşur ve bunların bazıları yalnızca bilgilendirme amaçlı olarak görünebilir, ancak diğer uyarılar bağlamında daha yakından bakmaya değer olabilir. 

## <a name="continuous-monitoring-and-assessments"></a>Sürekli izleme ve değerlendirmeler

Azure Güvenlik Merkezi, Microsoft genelinde tehdit ortamındaki değişiklikleri sürekli olarak izleyen güvenlik araştırma ve veri bilimi ekiplerinin olmasından yararlanır. Buna aşağıdaki girişimler dahildir:

* **Tehdit istihbaratı izleme**: Tehdit istihbaratı, mevcut veya ortaya çıkan tehditler hakkında mekanizmalar, göstergeler, etkileri ve eyleme geçirilebilir tavsiyeler içerir. Bu bilgileri güvenlik topluluğu içinde paylaşılır ve Microsoft, iç ve dış kaynaklardan gelen tehdit bilgisi akışlarını sürekli olarak izler.
* **Sinyal paylaşımı**: Microsoft'un geniş bulut ve şirket içi hizmetler, sunucular ve istemci uç noktası aygıtlarından oluşan geniş portföyündeki güvenlik ekiplerinin öngörüleri paylaşılır ve analiz edilir.
* **Microsoft güvenlik uzmanları**: Microsoft’ta hukuk ve web saldırıcı algılama gibi uzman güvenlik alanlarında çalışan ekiplerle sürekli iletişim.
* **Algılama ayarı**: Gerçek müşteri veri kümelerine göre algoritmalar çalıştırılır ve güvenlik araştırmacıları, sonuçları doğrulamak üzere müşterilerle işbirliği yapar. Doğru ve yanlış pozitifler kullanılarak machine learning algoritmaları iyileştirilir.

Bu birleşik çabalar, anında yararlanabileceğiniz yeni ve geliştirilmiş algılamalarla sonuçlanır – yapabileceğiniz hiçbir işlem yoktur.


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Güvenlik Merkezi'nde kullanılabilen farklı uyarı türlerini öğrendiniz. Daha fazla bilgi için bkz.

* [Azure Güvenlik Merkezi'nde tehdit koruması](threat-protection.md) - Azure Güvenlik Merkezi tarafından görüntülenen güvenlik uyarılarının kaynaklarının kısa bir açıklaması için 
* **Azure Etkinlik Günlüğü'ndeki güvenlik uyarıları** - Azure portalında veya programlı olarak kullanılabilir olmanın yanı sıra, Güvenlik uyarıları ve olaylar [Azure Etkinlik Günlüğü'ndeki](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)olaylar olarak denetlenir. Etkinlik şeması hakkında daha fazla bilgi için [Azure Etkinliği günlüğündeki Güvenlik Uyarıları'na](https://go.microsoft.com/fwlink/?linkid=2114113) bakın

