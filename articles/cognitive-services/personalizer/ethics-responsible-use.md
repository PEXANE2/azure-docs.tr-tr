---
title: Etik ve sorumlu kullanım - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu yönergeler, şirketinizde ve hizmetinizde güven oluşturmanıza yardımcı olacak şekilde kişiselleştirmeyi uygulamanıza yardımcı olmayı amaçlamaktadır. Araştırma, öğrenmek ve insanların yaşamları üzerinde kişiselleştirme etkisi üzerinde kasıtlı duraklatmak emin olun. Şüpheye düştüğünüzde, yol gösterin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 11b626c0033814f0886ac76fff0c5d4087a80554
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71720232"
---
# <a name="guidelines-for-responsible-implementation-of-personalizer"></a>Personalizer sorumlu uygulanması için yönergeler

İnsanların ve toplumun AI'nin tam potansiyelini gerçekleştirebilmek için, uygulamaların uygulamalarına AI ekleyenlerin ve AI ile oluşturulmuş uygulamaların kullanıcılarının güvenini kazanacak şekilde tasarlanmalıdır. Bu yönergeler, Personalizer'ı şirketinize ve hizmetinize güven oluşturmanıza yardımcı olacak şekilde uygulamanıza yardımcı olmayı amaçlamaktadır. Araştırma, öğrenmek ve insanların yaşamları üzerinde kişiselleştirme etkisi üzerinde kasıtlı duraklatmak emin olun. Şüpheye düştüğünüzde, yol gösterin.

Bu yönergeler yasal tavsiye olarak tasarlanmamıştır ve başvurunuzun bu alandaki ve sektörünüzdeki yasalardaki hızlı gelişmelere uygun olduğundan ayrı ayrı emin olmalısınız.

Ayrıca, Personalizer kullanarak uygulamanızı tasarlarken, etik, gizlilik, güvenlik, güvenlik, dahil etme, şeffaflık ve hesap verebilirlik de dahil olmak üzere veri merkezli bir bilgi sistemi geliştirirken sahip olduğunuz geniş bir sorumluluk kümesini göz önünde bulundurmalısınız. Bu konuda daha fazla bilgi için [Önerilen okuma](#recommended-reading) bölümünden okuyabilirsiniz.

Aşağıdaki içeriği başlangıç denetim listesi olarak kullanabilir ve senaryonuza göre özelleştirebilir ve hassaslaştırabilirsiniz. Bu belgenin iki ana bölümü vardır: İlki, Personalizer için senaryolar, özellikler ve ödüller seçerken sorumlu kullanım hususlarını vurgulamaya adanmıştır. İkinci, Microsoft'un AI sistemleri inşa ederken göz önünde bulundurulması gerektiğine inandığı bir dizi değer almak ve Kilikörü kullanımınızın onları nasıl etkilediğine ilişkin eyleme geçirilebilir öneriler ve riskler sağlar. 


## <a name="your-responsibility"></a>Sizin sorumluluğunuz

Sorumlu uygulama için tüm kurallar, Personalizer kullanan geliştiricilerin ve işletmelerin bu algoritmaları toplumda kullanmanın etkilerinden sorumlu ve sorumlu oldukları temeline dayanmaktadır. Kuruluşunuzun dağıtacağı bir uygulama geliştiriyorsanız, bu uygulamanın çalışması ve insanları nasıl etkilediği için rolünüzü ve sorumlulukunuzu tanımalısınız. Üçüncü bir tarafça dağıtılacak bir uygulama tasarlıyorsanız, uygulamanın davranışından sonuçta kimin sorumlu olduğuna dair onlarla paylaşılan bir anlayışa gelin ve bu anlayışı belgeleyin.

Güven, yerine getirilmiş taahhütler kavramı üzerine kuruludur - kullanıcılarınızı, toplumunuzu ve uygulamalarınızın içinde çalıştığı yasal çerçeveyi göz önünde bulundurun, sahip olabilecekleri açık ve örtülü taahhütleri belirleyin.

Microsoft, bu sorumluluklar üzerinde hareket etmenize yardımcı olmak için araçları ve belgeleri için sürekli çaba sarf etmektedir. Ek araçların, ürün özelliklerinin ve belgelerin Personalizer'ı kullanmak için bu yönergeleri uygulamanıza yardımcı olacağına inanıyorsanız [Microsoft'a geri bildirim](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) sağlayın.


## <a name="factors-for-responsibly-implementing-personalizer"></a>Sorumlu Personalizer uygulamak için faktörler

Personalizer uygulamak kullanıcılarınız ve işletmeniz için çok değerli olabilir. Personalizer'ı sorumlu bir şekilde uygulamak için aşağıdaki yönergeleri göz önünde bulundurarak başlayın:

* Kişiselleştirme uygulamak için kullanım durumlarını seçme.
* [Bina ödül fonksiyonları](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-rewards.md).
* Kişiselleştirme için kullanacağınız bağlam ve olası eylemler le ilgili [özellikleri](https://github.com/Azure/personalization-rl/blob/master/docs/concepts-features.md) seçme.


## <a name="choosing-use-cases-for-personalizer"></a>Personalizer için kullanım örnekleri nin seçilmesi

İçeriği ve kullanıcı arabirimlerini kişiselleştirmeyi öğrenen bir hizmeti kullanmak yararlıdır. Kişiselleştirmenin gerçek dünyada olumsuz yan etkiler yaratma biçimi, kullanıcıların içerik kişiselleştirmeden habersiz olup olmadığı da dahil olmak üzere, yanlış da uygulanabilir. 

Olumsuz yan etkiler veya şeffaflık eksikliği için artan potansiyeli olan Personalizer kullanımörnekleri", "ödül"ün, acil bir ödüle aşırı basitleştirildiğinde olumsuz olabileceği birçok uzun vadeli karmaşık faktöre bağlı olduğu senaryoları içerir. bireyler için sonuçlar. Bunlar "sonuç" seçenekleri veya zarar riski içeren seçimler olarak kabul edilme eğilimindedir. Örnek: 


* **Finans**: Risk faktörlerinin bireylerin bilmediği, elde edilebilmediği veya itiraz edilemeyecek verilere dayandığı kredi, finans ve sigorta ürünlerindeki teklifleri kişiselleştirme. 
* **Eğitim**: Önerilerin önyargıları yayabileceği ve kullanıcıların diğer seçeneklere ilişkin bilincini azaltabileceği okul kursları ve eğitim kurumları için safları kişiselleştirmek.
* **Demokrasi ve Toplumsal Katılım**: Görüşleri etkilemek amacıyla kullanıcılar için içeriği kişiselleştirmek dolaylı ve manipülatiftir.
* **Üçüncü taraf ödül değerlendirmesi**: Ödülün kullanıcının kendi davranışı tarafından oluşturulan bir ödül yerine, kullanıcının ikinci bir üçüncü taraf değerlendirmesine dayandığı öğeleri kişiselleştirme.
* **Keşif lere tahammülsüzlük**: Kiskileyicinin keşif davranışının zarar verebileceği durumlar.

Personalizer için kullanım örnekleri seçerken:

* Kişiselleştirmenin kullanıcılarınıza nasıl yardımcı olduğunu göz önünde bulundurarak tasarım işlemini başlatın.
* Bazı öğeler kişiselleştirme desenleri veya keşif nedeniyle kullanıcılar için sıralanmış değilse gerçek dünyada olumsuz sonuçları göz önünde bulundurun.
* Kullanım [durumunuzun, GDPR](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32016R0679) Madde 22 veya diğer yasalar uyarınca düzenlenen veri konularını önemli ölçüde etkileyen otomatik işleme oluşturup oluşturmadığını göz önünde bulundurun.
* Kendini gerçekleştiren kehanet döngüleri düşünün. Bu durum, bir kişiselleştirme ödülü bir modeli eğitiyorsa, böylece daha sonra bir demografik grubun ilgili içeriğe erişmelerini daha da dışlayabilir. Örneğin, düşük gelirli bir mahallede çoğu insan prim sigorta teklifi almaz, ve yavaş yavaş mahallede hiç kimse yeterli keşif yoksa teklif görmek eğilimindedir.
* Gelecekte Personalizer'ı yeniden üretmek için gerekli olması durumunda modellerin ve öğrenme politikalarının kopyalarını kaydedin. Bunu periyodik olarak veya her model yenileme dönemini yapabilirsiniz.
* Uzay için yeterli keşif düzeyini ve "yankı odası" etkilerini azaltmak için bir araç olarak nasıl kullanılacağını düşünün.


## <a name="selecting-features-for-personalizer"></a>Personalizer için özellikleri seçme

İçeriği nisferin kişiselleştirilmesi, içerik ve kullanıcı hakkında yararlı bilgilere sahip olmasına bağlıdır. Bazı uygulamalar ve endüstriler için, bazı kullanıcı özelliklerinin doğrudan veya dolaylı olarak ayrımcı ve potansiyel olarak yasadışı olarak kabul edilebilenleri unutmayın.

Bu özelliklerin etkisini göz önünde bulundurun:

* **Kullanıcı demografisi**: Cinsiyet, cinsiyet, yaş, ırk, din ile ilgili özellikler: Bu özellikler bazı uygulamalarda düzenleyici nedenlerle izin verilmeyebilir ve kişiselleştirme genellemeleri ve önyargıları yayacağı için çevrelerinde kişiselleştirmek etik olmayabilir. Bu önyargı yayılımının bir örneği, mühendislik için yaşlı veya cinsiyete dayalı kitlelere gösterilmeyen bir iş ilanıdır.
* **Yerel bilgiler**: Dünyanın birçok yerinde, konum bilgileri (posta kodu, posta kodu veya mahalle adı gibi) gelir, ırk ve din ile son derece ilişkili olabilir.
* **Kullanıcı Adalet Algısı**: Uygulamanızın sağlam kararlar verdiği durumlarda bile, uygulamanızda görüntülenen içeriğin ayrımcı olacak özelliklerle ilişkili gibi görünecek şekilde değiştiğini algılayan kullanıcıların etkisini göz önünde bulundurun.
* **Özelliklerde İstenmeden Sapma**: Yalnızca popülasyonun bir alt kümesini etkileyen özellikler kullanılarak ortaya çıkabilecek önyargı türleri vardır. Bu, metindeki varlıkları keşfetmek için resim veya metin analitiğindeki öğeleri ayıklamak için görüntü çözümlemesi gibi algoritmik olarak oluşturuluyorsa ekstra dikkat gerektirir. Bu özellikleri oluşturmak için kullandığınız hizmetlerin özelliklerinden haberdar olun.

Bağlamve eylemlerde bulunmak üzere Kişiselleştirici'ye gönderilecek özellikleri seçerken aşağıdaki uygulamaları uygulayın:

* Bazı uygulamalar için belirli özellikleri kullanmanın yasallığını ve etiğini ve masum görünen özelliklerin kaçınmak istediğiniz veya kaçınmanız gereken başkaları için yakınlık olup olmadığını göz önünde bulundurun,
* Gördükleri seçenekleri kişiselleştirmek için algoritmaların ve veri çözümlemesi kullanılan kullanıcılara karşı saydam olun.
* Kendinize sorun: Bu bilgileri kendileri için içeriği kişiselleştirmek için kullanırsam kullanıcılarım bunu önemser ve mutlu olur mu? Bazı öğeleri vurgulama veya gizleme kararının nasıl olduğunu onlara göstermekten memnun olur muyum?
* Sınıflandırma veya segmentasyon verileri yerine davranışsal verileri diğer özelliklere göre kullanın. Demografik bilgiler geleneksel olarak perakendeciler tarafından tarihsel nedenlerle kullanılmıştır – demografik özelliklerin dijital bir çağdan önce toplanması ve üzerinde hareket etmesi kolay görünüyordu, ancak gerçek etkileşime sahip olduğunuzda demografik bilgilerin ne kadar alakalı olduğunu sorgulayın, kullanıcıların tercihleri ve kimlikleri ile daha yakından ilgili bağlamsal ve geçmiş veriler.
* Çok sayıda istismar edilirse kasıtlı olarak bazı kullanıcı sınıflarını bozmak, utandırmak ve taciz etmek için yanıltıcı şekillerde Personalizer'ı eğitebilecek kötü niyetli kullanıcılar tarafından özelliklerin 'taklit edilmesinin' nasıl önleyeceğiniz göz önünde bulundurun. 
* Uygun ve uygulanabilir olduğunda, uygulamanızı kullanıcılarınızın belirli kişisel özelliklerin kullanılmasını devre dışı bırakmalarına veya devre dışı bırakmalarına olanak sağlayacak şekilde tasarla. Bunlar "Konum bilgileri", "Aygıt Bilgileri", "Geçmiş Satın Alma Geçmişi" gibi gruplandırılabilir.


## <a name="computing-rewards-for-personalizer"></a>Personalizer için bilgisayar ödülleri

Personalizer uygulama iş mantığı tarafından sağlanan ödül puanına göre ödüllendirmek için hangi eylem seçimini geliştirmek için çaba göstermektedir.

İyi inşa edilmiş bir ödül puanı, bir kuruluşun misyonuna bağlı olan bir iş hedefine kısa vadeli bir vekil görevi olarak hareket edecektir.

Örneğin, tıklamaları ödüllendirmek, tıklanan şey dikkat dağıtıcı veya bir iş sonucuna bağlı olmasa bile, Personalizer Hizmetinin diğer her şey pahasına tıklama aramasını sağlayacaktır.

Zıt bir örnek olarak, bir haber sitesi tıklamalardan daha anlamlı bir şeye bağlı ödüller ayarlamak isteyebilir, örneğin "Kullanıcı içeriği okumak için yeterli zaman harcadı mı?" "İlgili makalelere veya referansları tıklattılar mı?". Personalizer ile ölçümleri ödüllere yakın bir şekilde bağlamak kolaydır. Ancak kısa vadeli kullanıcı etkileşimini iyi sonuçlarla şaşırtmamaya dikkat edin.

### <a name="unintended-consequences-from-reward-scores"></a>Ödül puanlarından istenmeyen sonuçlar
Ödül puanları en iyi niyetle oluşturulmuş olabilir, ancak yine de Personalizer'ın içeriği nasıl sıraladığı konusunda beklenmeyen sonuçlar veya istenmeyen sonuçlar oluşturabilir. 

Aşağıdaki örnekleri inceleyin:

* Video içeriğinin izlenme yüzdesi üzerinde ödüllendirici video içeriği kişiselleştirme muhtemelen daha kısa videolar sıralamak eğiliminde olacaktır.
* Sosyal medya paylaşımlarının nasıl paylaşıldığına veya içeriğin kendisine yönelik duygusal analizler yapılmadan ödüllendirilmesi, çok fazla "etkileşim" teşvik etme eğiliminde olan, ancak çok az değer katan saldırgan, yönetilmemiş veya kışkırtıcı içerik sıralamasına yol açabilir.
* Kullanıcıların değiştirmeyi beklemediği kullanıcı arabirimi öğeleri üzerindeki eylemi ödüllendirmek, düğmelerin uyarı yapılmadan şaşırtıcı bir şekilde konum veya amaç değiştirdiği ve belirli bir şekilde zorlaştığı kullanıcı arabiriminin kullanılabilirliğini ve öngörülebilirliğini etkileyebilir üretken kalmak için kullanıcı grupları.

Bu en iyi uygulamaları uygulayın:

* Etkiyi ve yan etkileri anlamak için farklı ödül yaklaşımlarını kullanarak sisteminizle çevrimdışı denemeler çalıştırın.
* Ödül fonksiyonlarınızı değerlendirin ve son derece saf bir insanın yorumunu nasıl bükeceğini ve onunla istenmeyen sonuçlara nasıl ulaşacağını kendinize sorun.


## <a name="responsible-design-considerations"></a>Sorumlu tasarım konuları

AI'nin sorumlu uygulamaları için tasarım alanları aşağıda veda edilebistir. [Gelecekte Hesaplanan](https://news.microsoft.com/futurecomputed/)bu çerçevede daha fazla abut öğrenin.

![Gelecekten Hesaplanan AI Değerleri](media/ethics-and-responsible-use/ai-values-future-computed.png)

### <a name="accountability"></a>Sorumluluk
*AI Sistemlerini tasarlayan ve dağıtan kişiler, sistemlerinin çalışma larından sorumlu olmalıdır.* 

* Personalizer'ı nasıl uygulayacağınız, belgelemeyi ve bunları ekibinize, yöneticilerinize ve tedarikçilerinize nasıl ileteceğimize ilişkin dahili yönergeler oluşturun.
* Ödül puanlarının nasıl hesaplandırılındığını periyodik olarak gözden geçirin, Hangi özelliklerin Kistirici'yi etkilediğini görmek için çevrimdışı değerlendirmeler yapın ve gereksiz ve gereksiz özellikleri ortadan kaldırmak için sonuçları kullanın.
* Personalizer'ın nasıl kullanıldığını, hangi amaçla ve hangi verilerle kullanıldığını kullanıcılarınıza açıkça iletin.
* Kişiselleştiricinin işlevlerini yerine getirmek, sonuçları yeniden üretebilmek için kullandığı modeller, öğrenme politikaları ve diğer veriler gibi arşiv bilgileri ve varlıkları.

### <a name="transparency"></a>Saydamlık
*AI Sistemleri Anlaşılabilir Olmalıdır.* Personalizer ile:

* *Kullanıcılara içeriğin nasıl kişiselleştirildiği hakkinda bilgi verin.* Örneğin, kullanıcılarınıza, Kullanıcının üst `Why These Suggestions?` özelliklerinin ve eylemlerin Personalizer'ın sonuçlarında rol oynadığını gösteren etiketli bir düğme gösterebilirsiniz.
* Kullanım koşullarınızın, deneyimi kişiselleştirmek için kullanıcılar ve davranışları hakkındaki bilgileri kullanacağınızdan bahsettiğinizden emin olun.

### <a name="fairness"></a>Adalet
*AI Sistemleri tüm insanlara adil davranmalıdır.*

* Sonuçlarıuzun uzun vadeli, sonuç olarak sıralı veya gerçek zarar içeren kullanım durumları için Personalizer kullanmayın.
* İçeriği kişiselleştirmek için uygun olmayan veya istenmeyen önyargıları yaymaya yardımcı olabilecek özellikler kullanmayın. Örneğin, benzer finansal koşullara sahip herkes finansal ürünler için aynı kişiselleştirilmiş önerileri görmelidir.
* Editörlerden, algoritmik araçlardan veya kullanıcıların kendilerinden gelen özelliklerde var olabilecek önyargıları anlayın.

### <a name="reliability-and-safety"></a>Güvenilirlik ve güvenlik
*AI Sistemleri güvenilir ve güvenli bir şekilde gerçekleştirmelidir.* Personalizer için:

* *Personalizer'a seçilmemesi gereken eylemler de sunmayın.* Örneğin, anonim veya reşit olmayan bir kullanıcı için bir öneride bulunurken, uygunsuz filmler kişiselleştirmek için eylemlerden filtrelenmelidir.
* *Personalizer modelinizi bir iş varlığı olarak yönetin.*  Personalizer Loop'unuzun arkasındaki modeli ve öğrenme politikalarını ne sıklıkta kaydedip yedekleştireceğimize ve bunu önemli bir iş varlığı olarak değerlendireceğimize göz önünde bulundurun. Geçmiş sonuçların çoğaltılması, öz denetim ve iyileştirmenin ölçülmesi için önemlidir.
* *Kullanıcılardan doğrudan geri bildirim almak için kanallar sağlayın.* Yalnızca doğru hedef kitlelerin doğru içeriği görmesini sağlamak için güvenlik denetimlerini kodlamaya ek olarak, kullanıcıların şaşırtıcı veya rahatsız edici olabilecek içerikleri bildirmeleri için bir geri bildirim mekanizması sağlayın. Özellikle içeriğiniz kullanıcılardan veya üçüncü taraflardan geliyorsa, içeriği gözden geçirmek ve doğrulamak için Microsoft İçerik Moderatörü veya ek araçlar kullanmayı düşünün.
* *Sık sık çevrimdışı Değerlendirmeler gerçekleştirin.* Bu eğilimleri izlemek ve etkinliğin bilindiğinden emin olun yardımcı olacaktır.
* *Kötü amaçlı manipülasyonu algılamak ve harekete getirmek için bir işlem kurun.* Makine öğreniminden ve Yapay Eğitim sistemlerinin çevrelerinden öğrenme yeteneğinden yararlanarak sonucu hedeflerine kaydıracak aktörler vardır. Personalizer kullanımınız önemli seçimleri etkileyebilmek için bir konumdaysa, uygun koşullarda insan incelemesi de dahil olmak üzere bu saldırı sınıflarını tespit etmek ve azaltmak için uygun araçlara sahip olduğundan emin olun.

### <a name="security-and-privacy"></a>Güvenlik ve gizlilik
*AI Sistemleri güvenli olmalı ve mahremiyete saygı göstermelidir.* Personalizer kullanırken:

* Yerel ve sektörel yönetmeliklerinizden *sonra, toplanan veriler ve nasıl kullanıldığı konusunda kullanıcıları önceden bilgilendirin ve onaylarını önceden alın.*
* *Gizliliği koruyan kullanıcı denetimleri sağlayın.* Kişisel bilgileri depolayan uygulamalarda, şu işlevler için bulunması kolay bir düğme sağlamayı düşünün: 
   * `Show me all you know about me`    
   * `Forget my last interaction` 
   * `Delete all you know about me`

Bazı durumlarda, bunlar yasal olarak gerekli olabilir. Silinen verilerin izlerini içermemesi için modelleri düzenli aralıklarla yeniden eğitme deki dengeleri göz önünde bulundurun.

### <a name="inclusiveness"></a>Kapsayıcılık
*Çok çeşitli insan ihtiyaç ve deneyimlerine hitap edin.*
* *Erişilebilirlik özellikli arabirimler için kişiselleştirilmiş deneyimler sağlayın.* İyi kişiselleştirme gelen verimlilik - etkileşimlerde çaba, hareket ve gereksiz tekrarı miktarını azaltmak için uygulanan - özellikle engelli insanlar için yararlı olabilir.
* *Uygulama davranışını içeriğe ayarlayın.* Bir sohbet botu niyetleri arasında disambiguate için Personalizer kullanabilirsiniz, örneğin, doğru yorumu bağlamsal olabilir ve bir boyutu tüm uymayabilir. 


## <a name="proactive-readiness-for-increased-data-protection-and-governance"></a>Artırılmış veri koruması ve yönetişim için proaktif hazırlık

Düzenleyici bağlamlarda belirli değişiklikleri tahmin etmek zordur, ancak genel olarak kişisel verilerin saygılı kullanımını sağlamak ve algoritmik karar verme ile ilgili şeffaflık ve seçim sağlamak için minimum yasal çerçevenin ötesine geçmek akıllıca olacaktır.


* Bireylerden toplanan verilerüzerinde yeni kısıtlamalar olabileceği ve bunun karar vermek için nasıl kullanıldığını gösterme nin gerekebileceği bir durumu önceden planlamayı düşünün.
* Kullanıcıların marjinalleştirilmiş savunmasız popülasyonları, çocukları, ekonomik güvenlik açığı olan kullanıcıları veya algoritmik manipülasyondan etkilenmeye açık kullanıcıları içerebileceği ekstra hazırlık durumunu göz önünde bulundurun.
* İzleyici hedefleme ve hedef kitleyi etkileyen veri toplama programlarının ve algoritmalarının nasıl oynandıği ve kanıtlanmış stratejik hatalardan nasıl kaçınılabilmek konusunda yaygın memnuniyetsizliği göz önünde bulundurun.


## <a name="proactive-assessments-during-your-project-lifecycle"></a>Proje yaşam döngünüz sırasında proaktif değerlendirmeler

Ekip üyeleri, kullanıcılar ve işletme sahipleri için sorumlu kullanımla ilgili endişeleri bildirmek için yöntemler oluşturmayı ve çözüme öncelik veren ve misillemeyi önleyen bir süreç oluşturmayı düşünün.

Herhangi bir teknolojikullanımının yan etkileri hakkında düşünme herhangi bir kişi kendi bakış açısı ve yaşam deneyimi ile sınırlıdır. Ekiplerinize, kullanıcılarınıza veya danışma kurullarınıza daha farklı sesler getirerek mevcut görüş yelpazesini genişletin; öyle ki, onların konuşması mümkün ve teşvik edilir. Bu alandaki ekip bilgilerini daha da genişletmek ve karmaşık ve hassas konuları tartışmak için yetenek eklemek için eğitim ve öğrenme materyallerini göz önünde bulundurun.

Kullanıcı deneyimi, güvenlik veya DevOps ile ilgili görevler gibi uygulama yaşam döngüsündeki diğer çapraz kesme görevleri gibi sorumlu kullanımla ilgili görevleri ele almayı düşünün. Bu görevler ve gereksinimleri sonradan düşünülemez. Sorumlu kullanım, uygulama yaşam döngüsü boyunca tartışılmalı ve doğrulanmalıdır.
 
## <a name="questions-and-feedback"></a>Sorular ve geri bildirimler

Microsoft, bu sorumluluklar üzerinde hareket etmenize yardımcı olmak için sürekli olarak araçlar ve belgeler üzerinde çaba harcanıdır. Ekibimiz, personalizer'ı kullanmak için bu yönergeleri uygulamanıza yardımcı olacağına inanıyorsanız, sizi [Microsoft'a geri bildirim de](mailto:cogsvcs-RL-feedback@microsoft.com?subject%3DPersonalizer%20Responsible%20Use%20Feedback&body%3D%5BPlease%20share%20any%20question%2C%20idea%20or%20concern%5D) sunmaya davet eder.

## <a name="recommended-reading"></a>Önerilen okuma

* Ocak 2018'de yayımlanan AI'nin sorumlu gelişimi için Microsoft'un altı ilkesine bakın, [Gelecek Hesaplandı](https://news.microsoft.com/futurecomputed/)
* [Geleceğin Sahibi Kim?](https://www.goodreads.com/book/show/15802693-who-owns-the-future) Jaron Lanier tarafından.
* [Matematik Destruction Silahlar](https://www.goodreads.com/book/show/28186015-weapons-of-math-destruction) tarafından - Cathy O'Neil
* [Etik ve Veri Bilimi](https://www.oreilly.com/library/view/ethics-and-data/9781492043898/) DJ Patil, Hilary Mason, Mike Loukides tarafından.
* [ACM Etik Kuralları](https://www.acm.org/code-of-ethics)
* [Genetik Bilgi Ayrımcılık Karşıtı Yasası - GINA](https://en.wikipedia.org/wiki/Genetic_Information_Nondiscrimination_Act)
* [Sorumlu Algoritmalar için FATML İlkeleri](https://www.fatml.org/resources/principles-for-accountable-algorithms)


## <a name="next-steps"></a>Sonraki adımlar

[Özellikler: eylem ve bağlam](concepts-features.md).
