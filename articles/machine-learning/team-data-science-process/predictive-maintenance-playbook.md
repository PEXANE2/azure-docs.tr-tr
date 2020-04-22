---
title: Tahmine dayalı bakım çözümleri için Azure AI kılavuzu - Ekip Veri Bilimi Süreci
description: Birden fazla dikey endüstride tahmine dayalı bakım çözümlerine güç veren veri biliminin kapsamlı bir açıklaması.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: previous-author=fboylu, previous-ms.author=fboylu
ms.openlocfilehash: 301e2be0c8b971a0236de6a8b5c8bd5f278c3aee
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686745"
---
# <a name="azure-ai-guide-for-predictive-maintenance-solutions"></a>Tahmine dayalı bakım çözümleri için Azure AI kılavuzu

## <a name="summary"></a>Özet

Tahmine dayalı bakım **(PdM),** çeşitli sektörlerdeki işletmelerin operasyonel maliyetlerde yüksek varlık kullanımı ve tasarruf elde etmelerine yardımcı olabilecek tahmine dayalı analizlerin popüler bir uygulamasıdır. Bu kılavuz, [Microsoft Azure AI platform](https://azure.microsoft.com/overview/ai-platform) teknolojisini kullanarak PdM çözümlerini başarıyla geliştirmek ve dağıtmak için iş ve analitik yönergeleri ve en iyi uygulamaları bir araya getirir.

Yeni başlayanlar için, bu kılavuz sektöre özel iş senaryoları ve PdM için bu senaryoları niteleme sürecini tanıetmektedir. PdM çözümleri oluşturmak için veri gereksinimleri ve modelleme teknikleri de sağlanır. Kılavuzun ana içeriği veri bilimi süreci üzerindedir - veri hazırlama adımları da dahil olmak üzere, özellik mühendisliği, model oluşturma ve model operasyonelleştirme. Bu kılavuz, bu anahtar kavramları tamamlamak için PdM uygulama geliştirmeyi hızlandırmaya yardımcı olacak bir çözüm şablonları kümesini listeler. Kılavuz ayrıca, uygulayıcının veri biliminin arkasındaki AI hakkında daha fazla bilgi edinebilmek için yararlı eğitim kaynaklarına da işaret ediyor. 

### <a name="data-science-guide-overview-and-target-audience"></a>Veri Bilimi kılavuzuna genel bakış ve hedef kitle
Bu kılavuzun ilk yarısında tipik iş sorunları, bu sorunları gidermek için PdM uygulamanın yararları açıklanır ve bazı yaygın kullanım örnekleri listeler. İş karar vericileri (BDM'ler) bu içerikten yararlanacaktır. İkinci yarı, PdM'nin arkasındaki veri bilimini açıklar ve bu kılavuzda belirtilen ilkeler kullanılarak oluşturulmuş PDM çözümlerinin bir listesini sağlar. Ayrıca öğrenme yolları ve eğitim materyali için işaretçiler sağlar. Teknik karar vericiler (TDM'ler) bu içeriği yararlı bulacaktır.

| Ile başlayın ... | Eğer iseniz ... |
|:---------------|:---------------|
| [Tahmine dayalı bakım için iş durumu](#business-case-for-predictive-maintenance) |bir iş karar vericisi (BDM) kesinti ve işletme maliyetlerini azaltmak ve ekipman kullanımını geliştirmek isteyen |
| [Tahmine dayalı bakım için Veri Bilimi](#data-science-for-predictive-maintenance) |benzersiz veri işleme ve tahmine dayalı bakım için AI gereksinimlerini anlamak için PdM teknolojilerini değerlendiren teknik bir karar verici (TDM) |
| [Tahmine dayalı bakım için çözüm şablonları](#solution-templates-for-predictive-maintenance)|bir yazılım mimarı veya AI Geliştirici hızla bir demo veya bir proof-of-concept stand-of-up isteyen |
| [Tahmine dayalı bakım için eğitim kaynakları](#training-resources-for-predictive-maintenance) | yukarıdakilerin herhangi birini veya tamamını ve veri bilimi, araçları ve teknikleriarkasındaki temel kavramları öğrenmek istiyorum.

### <a name="prerequisite-knowledge"></a>Önkoşul bilgileri
BDM içeriği okuyucunun önceden veri bilimi bilgisine sahip olmasını beklemez. TDM içeriği için, istatistik ve veri bilimi temel bilgi yararlıdır. Azure Veri ve AI hizmetleri, Python, R, XML ve JSON hakkında bilgi önerilir. AI teknikleri Python ve R paketlerinde uygulanır. Çözüm şablonları Azure hizmetleri, geliştirme araçları ve SDK'lar kullanılarak uygulanır.

## <a name="business-case-for-predictive-maintenance"></a>Tahmine dayalı bakım için iş durumu

İşletmeler, sermaye yatırımlarının getirisini gerçekleştirmek için kritik ekipmanların en yüksek verimlilik ve kullanım da çalışıyor olması gerekir. Bu varlıklar uçak motorları, türbinler, asansörler veya endüstriyel soğutucular arasında olabilir - bu milyonlara mal - fotokopi makineleri, kahve makineleri veya su soğutucular gibi günlük aletleri aşağı.
- Varsayılan olarak, çoğu işletme, parçaların başarısız olduğu ve değiştirildiği _düzeltici bakıma_güvenir. Düzeltici bakım parçaların tamamen kullanılmasını (bu nedenle bileşen ömrünün boşa harcanmasını gerektirmez), ancak işletmeye çalışmama süresi, işçilik ve planlanmamış bakım gereksinimlerinde (kapalı saatler veya uygunsuz konumlar) maliyeti sağlar.
- Bir sonraki düzeyde, işletmeler _önleyici bakım_uygularlar, burada bir parça için yararlı ömrü belirler ler ve bir hatadan önce bunu korur veya değiştirirler. Önleyici bakım, planlanmamış ve yıkıcı hataları önler. Ancak planlanan kesinti süresinin yüksek maliyetleri, bileşenin yararlı ömrü boyunca yeterince kullanılmaması ve işçilik hala devam etmektedir.
- _Öngörülebilir bakımın_ amacı, bileşenlerin _zamanında_ değiştirilmesini sağlayarak düzeltici ve önleyici bakım arasındaki dengeyi optimize etmektir. Bu yaklaşım, yalnızca bir hataya yakın olduklarında bu bileşenlerin yerini alır. Bileşen ömrünü uzatarak (önleyici bakımla karşılaştırıldığında) ve planlanmamış bakım ve işçilik maliyetlerini (düzeltici bakım üzerinde) azaltarak, işletmeler maliyet tasarrufu ve rekabet avantajları elde edebilir.

## <a name="business-problems-in-pdm"></a>PdM'de iş sorunları
İşletmeler beklenmeyen arızalar nedeniyle yüksek operasyonel riskle karşı karşıya dır ve karmaşık sistemlerdeki sorunların temel nedeni hakkında sınırlı bir içgörüye sahiptir. Bazı önemli iş soruları şunlardır:

- Ekipman veya sistem performansı veya işlevselliğindeki anormallikleri tespit edin.
- Bir varlığın yakın gelecekte başarısız olup olmadığını tahmin edin.
- Bir varlığın kalan yararlı ömrünü tahmin edin.
- Bir varlığın başarısızlığının ana nedenlerini belirleyin.
- Bir varlık üzerinde ne zaman, hangi bakım eylemlerinin yapılması gerektiğini belirleyin.

PDM'nin tipik hedef ifadeleri şunlardır:

- Kritik görev ekipmanının operasyonel riskini azaltın.
- Hataları nönce ortaya çıkmadan önce tahmin ederek varlıkların getiri oranını artırın.
- Tam zamanında bakım işlemlerini etkinleştirerek bakım maliyetini kontrol edin.
- Müşteri yıpranmayı düşürün, marka imajını geliştirin ve satışları kaybetti.
- Yeniden sipariş noktasını tahmin ederek stok düzeylerini azaltarak stok maliyetlerini düşürün.
- Çeşitli bakım sorunlarına bağlı desenleri keşfedin.
- Varlık koşullarına ilişkin sağlık puanları gibi KP'ler (temel performans göstergeleri) sağlayın.
- Varlıkların kalan ömrünü tahmin edin.
- Zamanında bakım faaliyetleri önerin.
- Parçaların değiştirilmesi için sipariş tarihlerini tahmin ederek tam zamanında envanterini etkinleştirin.

Bu hedef ifadeleri için başlangıç noktaları şunlardır:

- _veri bilim adamları_ analiz etmek ve belirli tahmin sorunları çözmek için.
- _bulut mimarlar ve geliştiriciler_ birlikte bir son çözüm koymak.

## <a name="qualifying-problems-for-predictive-maintenance"></a>Tahmine dayalı bakım için eleme sorunları
Tüm kullanım örneklerinin veya iş sorunlarının PDM tarafından etkin bir şekilde çözülebileceğini vurgulamak önemlidir. Sorun seçimi sırasında göz önünde bulundurulması gereken üç önemli eleme ölçütleri vardır:

- Sorun doğada öngörülü olmalıdır; yani, bir hedef ya da tahmin etmek için bir sonuç olmalıdır. Sorun, algılandıklarında hataları önlemek için açık bir eylem yolu da olmalıdır.
- Sorun _hem iyi hem de kötü sonuçlar_içeren ekipman ın operasyonel geçmişi bir kayıt olmalıdır. Kötü sonuçları azaltmak için alınan eylemler kümesi de bu kayıtların bir parçası olarak kullanılabilir olmalıdır. Hata raporları, performans bozulması, onarım ve değiştirme günlükleri bakım günlükleri de önemlidir. Buna ek olarak, bunları geliştirmek için üstlenilen onarımlar ve değiştirme kayıtları da yararlıdır.
- Kaydedilen geçmiş, kullanım örneğini desteklemek için _yeterli_ kalitede _olan ilgili_ verilere yansıtılmalıdır. Veri alaka düzeyi ve yeterlilik hakkında daha fazla bilgi için, [tahmine dayalı bakım için Veri gereksinimlerine](#data-requirements-for-predictive-maintenance)bakın.
- Son olarak, iş sorunu net bir anlayışa sahip etki alanı uzmanları olmalıdır. Analistin verileri anlamasına ve yorumlayabilmeleri için iç süreçlerin ve uygulamaların farkında olmalıdırlar. Ayrıca, gerekirse sorunlar için doğru verilerin toplanmasına yardımcı olmak için varolan iş süreçlerinde gerekli değişiklikleri yapabilmelidirler.

## <a name="sample-pdm-use-cases"></a>Örnek PdM kullanım örnekleri
Bu bölümde, Havacılık, Kamu Hizmetleri ve Ulaştırma gibi çeşitli sektörlerden pdm kullanım örnekleri koleksiyonu üzerinde duruluyor. Her bölüm bir iş sorunuyla başlar ve PDM'nin yararlarını, iş sorununu çevreleyen ilgili verileri ve son olarak bir PdM çözümünün avantajlarını tartışır.

| İş Sorunu | PDM'den Avantajlar |
|:-----------------|-------------------|
|**Havacılık**      |                   |
|Mekanik sorunlar nedeniyle _uçuş gecikmesi ve iptalleri._ Zamanında onarılamayacak arızalar uçuşların iptal edilmesine ve zamanlama ve işlemleri kesintiye uğratabilir. |PDM çözümleri, mekanik arızalar nedeniyle bir uçağın gecikme veya iptal olasılığını tahmin edebilir.|
|_Uçak motoru parçaları arızası_: Uçak motoru parça değişimi havayolu endüstrisinde en yaygın bakım görevleri arasındadır. Bakım çözümleri, bileşen stok kullanılabilirliğinin, teslimatının ve planlamanın dikkatli bir şekilde yönetilmesini gerektirir|Bileşen güvenilirliği konusunda istihbarat toplayabilmek yatırım maliyetlerinde önemli bir azalmaya yol açmaktadır.|
|**Finans** |                         |
|_ATM başarısızlığı_ bankacılık sektöründe yaygın bir sorundur. Buradaki sorun, bir ATM nakit çekim işleminin kağıt sıkışması veya nakit dağıtıcıdaki parça hatası nedeniyle kesintiye uğrama olasılığını bildirmektir. İşlem hataları öngörülerine bağlı olarak, ATM'lere hataların oluşmasını önlemek için proaktif olarak servis edilebilir.| Makinenin bir işlemin ortasında başarısız olması yerine, istenen alternatif, makineyi tahmine dayalı olarak hizmeti reddetmek üzere programlamaktır.|
|**Enerji** |                          |
|_Rüzgar türbini arızaları_: Rüzgar türbinleri çevreye duyarlı ülkelerde/bölgelerde ana enerji kaynağıdır ve yüksek sermaye maliyetleri içerir. Rüzgar türbinlerinde önemli bir bileşen olan başarısızlık türbin etkisiz hale jeneratör motoru vardır. Düzeltmek de son derece pahalıdır.|MTTF (ortalama arıza süresi) gibi KP'leri tahmin etmek, enerji şirketlerinin türbin arızalarını önlemesine ve minimum kapalı kalma süresini sağlamasına yardımcı olabilir. Arıza olasılıkları, teknisyenleri yakında başarısız olma olasılığı olan türbinleri izlemeleri ve zamana dayalı bakım rejimlerini planlamaları konusunda bilgilendirecektir. Tahmine dayalı modeller, teknisyenlerin sorunların temel nedenlerini daha iyi anlamalarına yardımcı olan hataya katkıda bulunan farklı etkenler hakkında öngörüler sağlar.|
|_Devre kesici arızaları_: Elektriklerin evlere ve işyerlerine dağıtımı, enerji dağıtımını garanti etmek için elektrik hatlarının her zaman çalışır hale olmasını gerektirir. Devre kesiciler, aşırı yükleme veya olumsuz hava koşullarında güç hatlarının sınırlandırılmasına veya hasar görmesini önlemeye yardımcı olur. Buradaki iş problemi devre kesici arızalarını tahmin etmektir.| PDM çözümleri, onarım maliyetlerini azaltmaya ve devre kesiciler gibi ekipmanların kullanım ömrünü artırmaya yardımcı olur. Beklenmeyen arızaları ve hizmet kesintilerini azaltarak güç ağının kalitesinin artırılmasına yardımcı olurlar.|
|**Taşımacılık ve lojistik** |    |
|_Asansör kapı arızaları_: Büyük asansör firmaları, dünya çapında milyonlarca fonksiyonel asansör için tam bir yığın hizmeti sunmaktadır. Asansör güvenliği, güvenilirlik ve çalışma süresi müşterileri için başlıca kaygılardır. Bu şirketler, düzeltici ve önleyici bakım ile onlara yardımcı olmak için, sensörler aracılığıyla bu ve diğer çeşitli özellikleri izlemek. Asansörde en önemli müşteri problemi arızalı asansör kapılarıdır. Bu durumda iş sorunu kapı hatalarının olası nedenlerini tahmin eden bir bilgi tabanı tahmin uygulaması sağlamaktır.| Asansörler potansiyel olarak 20-30 yıllık bir yaşam süresi için sermaye yatırımlarıdır. Yani her potansiyel satış son derece rekabetçi olabilir; bu nedenle hizmet ve destek beklentileri yüksektir. Tahmine dayalı bakım, bu şirketlere ürün ve hizmet tekliflerinde rakiplerine göre avantaj sağlayabilir.|
|_Tekerlek arızaları_: Tekerlek arızaları tüm tren raydan çıkmaların yarısını oluşturuyor ve küresel demiryolu endüstrisine milyarlarca dolara mal oluyor. Tekerlek arızaları da rayların bozulmasına neden olur ve bazen rayın erken kırılmasına neden olur. Demiryolu molaları raydan çıkma gibi felaket lere yol açar. Bu gibi durumlardan kaçınmak için, demiryolları tekerleklerin performansını izler ve önleyici bir şekilde değiştirir. Buradaki iş problemi tekerlek arızalarının tahmin ilerli.| Tekerleklerin öngörülebilir bakımı, tekerleklerin tam zamanında değiştirilmesine yardımcı olacaktır |
|_Metro tren kapı arızaları_: Metro işlemlerindeki gecikmelerin en önemli nedeni tren vagonlarının kapı arızalarıdır. Buradaki iş problemi tren kapısı arızalarını tahmin etmek.|Bir kapı arızası erken farkındalık, ya da bir kapı arızası kadar gün sayısı, iş tren kapısı servis programları optimize yardımcı olacaktır.|

Bir sonraki bölümde, yukarıda tartışılan PdM avantajlarının nasıl gerçekleştirilebildiğinin ayrıntıları nasıI yer almaktadır.

## <a name="data-science-for-predictive-maintenance"></a>Tahmine dayalı bakım için Veri Bilimi

Bu bölümde PdM için veri bilimi ilkeleri ve uygulamaları genel yönergeleri verilmektedir. Bir TDM, çözüm mimarı veya geliştiricinin PdM için uçtan uca AI uygulamaları oluşturmak için ön koşulları ve süreci anlamasına yardımcı olmak için tasarlanmıştır. Bu bölümü, [tahmine dayalı bakım için Çözüm Şablonları'nda](#solution-templates-for-predictive-maintenance)listelenen demoların ve kavram kanıtı şablonlarının gözden geçirilmesiyle birlikte okuyabilirsiniz. Ardından, PdM çözümünüzü Azure'da uygulamak için bu ilkeleri ve en iyi uygulamaları kullanabilirsiniz.

> [!NOTE]
> Bu kılavuz okuyucu Veri Bilimi öğretmek için tasarlanmıştır DeğİlDİr. Çeşitli yararlı kaynaklar [tahmine dayalı bakım için eğitim kaynakları](#training-resources-for-predictive-maintenance)için bölümünde daha fazla okuma için sağlanmaktadır. Kılavuzda listelenen [çözüm şablonları,](#solution-templates-for-predictive-maintenance) belirli PdM sorunları için bu AI tekniklerinden bazılarını gösterir.

## <a name="data-requirements-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri gereksinimleri

Herhangi bir öğrenmenin başarısı (a) öğretilenlerin kalitesine ve (b) öğrencinin yeteneğine bağlıdır. Tahmine dayalı modeller tarihsel verilerden örüntüler öğrenir ve bu gözlenen desenlere dayanarak belirli olasılıklar ile gelecekteki sonuçları tahmin. Bir modelin tahmin doğruluğu, eğitim ve test verilerinin alaka durumuna, yeterliliğe ve kalitesine bağlıdır. Bu modeli kullanarak 'puanlı' yeni veriler, eğitim/test verileriyle aynı özelliklere ve şemalara sahip olmalıdır. Yeni verilerin özellik özellikleri (türü, yoğunluğu, dağılımı vb.) eğitim ve test veri kümelerinin özellikleriyle eşleşmelidir. Bu bölümün odak noktası bu tür veri gereksinimleridir.

### <a name="relevant-data"></a>İlgili veriler

İlk olarak, veri _sorunla ilgili_olmalıdır. Yukarıda tartışılan _tekerlek hatası_ kullanım örneğini göz önünde bulundurun - eğitim verileri tekerlek işlemleriyle ilgili özellikler içermelidir. Sorun _çekiş sisteminin_başarısızlığını tahmin etmek için ise, eğitim verileri çekiş sistemi için tüm farklı bileşenleri kapsayacak şekilde vardır. İlk servis talebi belirli bir bileşeni hedeflerken, ikinci servis talebi daha büyük bir alt sistemin başarısızlığını hedefler. Genel öneri, daha büyük alt sistemler yerine belirli bileşenler hakkında tahmin sistemleri tasarlamaktır, çünkü ikincisi daha dağınık verilere sahip olacaktır. Etki alanı uzmanı (bkz. [tahmine dayalı bakım için Eleme sorunları)](#qualifying-problems-for-predictive-maintenance)çözümleme için en alakalı veri alt kümelerini seçmede yardımcı olmalıdır. İlgili veri [kaynakları, tahmine dayalı bakım için veri hazırlığında](#data-preparation-for-predictive-maintenance)daha ayrıntılı olarak ele alınmıştır.

### <a name="sufficient-data"></a>Yeterli veri
Hata geçmişi verileriyle ilgili olarak genellikle iki soru sorulur: (1) "Bir modeli eğitmek için kaç hata olayı gerekir?" (2) "Kaç kayıt "yeterli" olarak kabul edilir?" Kesin bir cevap yok, ama sadece başparmak kuralları var. Için (1), daha fazla başarısızlık olayları sayısı, daha iyi model. Için (2) ve hata olaylarının tam sayısı verilere ve çözülmekte olan sorunun bağlamına bağlıdır. Ama diğer taraftan, bir makine çok sık başarısız olursa, o zaman iş, başarısızlık örnekleri azaltacaktır yerini alacak. Burada yine, etki alanı uzmanının rehberliği önemlidir. Ancak, _nadir olaylar_sorunu ile başa çıkmak için yöntemler vardır. Bunlar [dengesiz verileri işleme](#handling-imbalanced-data)bölümünde ele alınmıştır.

### <a name="quality-data"></a>Kalite verileri
Verilerin kalitesi önemlidir - her bir tahminör özniteliği değeri hedef değişkenin değeri ile birlikte _doğru_ olmalıdır. Veri kalitesi, istatistik ve veri yönetiminde iyi çalışılmış bir alandır ve bu nedenle bu kılavuz için kapsam dışıdır.

> [!NOTE]
> Kaliteli veri sunmak için çeşitli kaynaklar ve kurumsal ürünler vardır. Referansların bir örneği aşağıda verilmiştir:
> - Dasu, T, Johnson, T., Keşif Veri Madenciliği ve Veri Temizleme, Wiley, 2003.
> - [Araştırmacı Veri Analizi, Vikipedi](https://en.wikipedia.org/wiki/Exploratory_data_analysis)
> - [Hellerstein, J, Büyük Veritabanları için Nicel Veri Temizleme](http://db.cs.berkeley.edu/jmh/papers/cleaning-unece.pdf)
> - [de Jonge, E, van der loo, M, R ile Veri Temizliğine Giriş](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)

## <a name="data-preparation-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri hazırlama

### <a name="data-sources"></a>Veri kaynakları

Tahmine dayalı bakım için ilgili veri kaynakları şunları içerir, ancak bunlarla sınırlı değildir:
- Hata geçmişi
- Bakım/onarım geçmişi
- Makine çalışma koşulları
- Ekipman meta verileri

#### <a name="failure-history"></a>Hata geçmişi
PdM uygulamalarında hata olayları nadirdir. Ancak, tahmin modelleri inşa ederken, algoritmanın bir bileşenin normal çalışma deseni ve hata desenleri hakkında bilgi edinmesi gerekir. Bu nedenle, eğitim verileri her iki kategoriden de yeterli sayıda örnek içermelidir. Bakım kayıtları ve parça değiştirme geçmişi, hata olaylarını bulmak için iyi kaynaklardır. Bazı etki alanı bilgisi yardımıyla, eğitim verilerindeki anormallikler de hata olarak tanımlanabilir.

#### <a name="maintenancerepair-history"></a>Bakım/onarım geçmişi
Bir varlığın bakım geçmişi değiştirilen bileşenler, gerçekleştirilen onarım faaliyetleri vb. hakkında ayrıntılar içerir. Bu olaylar bozulma desenleri kaydeder. Eğitim verilerindeki bu önemli bilgilerin olmaması yanıltıcı model sonuçlarına yol açabilir. Hata geçmişi, bakım geçmişinde özel hata kodları veya parçalar için sipariş tarihleri olarak da bulunabilir. Hata kalıplarını etkileyen ek veri kaynakları araştırılmalı ve etki alanı uzmanları tarafından sağlanmalıdır.

#### <a name="machine-operating-conditions"></a>Makine çalışma koşulları
Çalışmadaki ekipmanın sensör tabanlı (veya diğer) akış verileri önemli bir veri kaynağıdır. PdM'deki önemli bir varsayım, bir makinenin sistem durumu rutin çalışması sırasında zaman içinde bozulur. Verilerin, bu yaşlanma modelini yakalayan zaman alabilen özellikler ve bozulmaya yol açan anormallikleri içermesi beklenmektedir. Algoritmanın zaman içinde hata ve hata olmayan desenleri öğrenmesi için verilerin zamansal yönü gereklidir. Bu veri noktalarına dayanarak, algoritma bir makinenin başarısız olmadan önce kaç birim daha çalışmaya devam edebileceğini tahmin etmeyi öğrenir.

#### <a name="static-feature-data"></a>Statik özellik verileri
Statik özellikler ekipman la ilgili meta verilerdir. Bunlara örnek olarak, ekipman yapımı, modeli, imal tarihi, hizmet başlangıç tarihi, sistemin konumu ve diğer teknik özellikler verilebilir.

[Örnek PdM kullanım örnekleri](#sample-pdm-use-cases) için ilgili veri örnekleri aşağıda tabloya işlenir:

| Kullanım Örneği | İlgili veri örnekleri |
|:---------|---------------------------|
|_Uçuş gecikmesi ve iptalleri_ | Uçuş ayakları ve sayfa günlükleri şeklinde uçuş rotası bilgileri. Uçuş ayağı verileri, kalkış/varış tarihi, saat, havaalanı, konaklama vb. gibi yönlendirme ayrıntılarını içerir. Sayfa günlüğü, yer bakım personeli tarafından kaydedilen bir dizi hata ve bakım kodunu içerir.|
|_Uçak motor parçaları arızası_ | Çeşitli parçaların durumu hakkında bilgi sağlayan uçaktaki sensörlerden toplanan veriler. Bakım kayıtları, bileşen hatalarının ne zaman oluştuğunu ve ne zaman değiştirildiklerini belirlemeye yardımcı olur.|
|_ATM Hatası_ | Her işlem için sensör okumaları (nakit/çek yatırma) ve nakit dağıtma. Notlar arasındaki boşluk ölçümü, not kalınlığı, not geliş mesafesi, çek öznitelikleri vb. hakkında bilgi Hata kodları sağlayan bakım kayıtları, onarım bilgileri, en son ne zaman nakit dağıtıcısı dolduruldu.|
|_Rüzgar türbini arızası_ | Sensörler sıcaklık, rüzgar yönü, üretilen güç, jeneratör hızı vb. türbin koşullarını izler. Veriler, çeşitli bölgelerde bulunan rüzgar çiftliklerinden birden fazla rüzgar türbininden toplanır. Tipik olarak, her türbinin belirli bir zaman aralığında ölçümleri ileten birden fazla sensör okuması olacaktır.|
|_Devre kesici arızaları_ | Düzeltici, önleyici ve sistematik eylemler içeren bakım günlükleri. Açık ve yakın eylemler gibi devre kesicilere gönderilen otomatik ve manuel komutları içeren operasyonel veriler. Üretim tarihi, konum, model vb. aygıt meta verileri. Voltaj seviyeleri, coğrafi konum, ortam koşulları gibi devre kesici özellikleri.|
|_Asansör kapısı arızaları_| Asansör tipi, üretilen tarih, bakım sıklığı, bina tipi ve benzeri asansör meta verileri. Kapı devir sayısı, ortalama kapı kapatma süresi gibi operasyonel bilgiler. Nedenleri ile başarısızlık geçmişi.|
|_Tekerlek arızaları_ | Tekerlek ivmesi, frenleme örnekleri, sürüş mesafesi, hız vb. ölçüleri ölçen sensör verileri Üretici gibi tekerlekler üzerinde statik bilgi, üretilen tarih. Sipariş tarihlerini ve miktarlarını izleyen parça sipariş veritabanından çıkarılan hata verileri.|
|_Metro tren kapısı arızaları_ | Kapı açma ve kapama saatleri, tren kapılarının mevcut durumu gibi diğer operasyonel veriler. Statik veriler varlık tanımlayıcısı, zaman ve koşul değeri sütunlarını içerir.|

### <a name="data-types"></a>Veri türleri
Yukarıdaki veri kaynakları göz önüne alındığında, PdM etki alanında gözlenen iki ana veri türleri şunlardır:

- _Zamansal veriler_: Operasyonel telemetri, makine koşulları, iş emri türleri, kayıt sırasında zaman damgası olacak öncelik kodları. Arıza, bakım/onarım ve kullanım geçmişi de her olayla ilişkili zaman damgaları olacaktır.
- _Statik veri_: Makinelerin veya operatör özelliklerinin teknik özelliklerini tanımladıkları için makine özellikleri ve genel olarak operatör özellikleri statiktir. Bu özellikler zaman içinde değişebilirse, bunlarla ilişkili zaman damgaları da olmalıdır.

Predictor ve hedef değişkenler, kullanılan algoritmaya bağlı olarak önceden işlenmeli/sayısal, [kategorik ve diğer veri türlerine](https://www.statsdirect.com/help/basics/measurement_scales.htm) dönüştürülmelidir.

### <a name="data-preprocessing"></a>Veri ön işleme
_Mühendislik özelliği_için bir ön koşul olarak, hangi özellikleri oluşturmak kolay bir şema oluşturmak için çeşitli akışlardan verileri hazırlamak. Verileri önce bir kayıt tablosu olarak görselleştirin. Tablodaki her satır bir eğitim örneğini temsil eder ve sütunlar _tahminci_ özelliklerini (bağımsız öznitelikler veya değişkenler olarak da adlandırılır) temsil eder. Son sütun(lar) _hedef_ (bağımlı değişken) olacak şekilde verileri düzenleyin. Her eğitim örneği için, bu sütunun değeri olarak bir _etiket_ atayın.

Zamansal veriler için, sensör verilerinin süresini zaman birimlerine bölün. Her kayıt bir kıymet için bir zaman birimine ait olmalı _ve farklı bilgiler sunmalıdır._ Zaman birimleri, saniye, dakika, saat, gün, ay ve benzeri katları iş ihtiyaçlarına göre tanımlanır. Zaman birimi _veri toplama sıklığı ile aynı olmak zorunda değildir._ Sıklık yüksekse, veriler bir birimden diğerine önemli bir fark göstermeyebilir. Örneğin, ortam sıcaklığının her 10 saniyede bir toplandığını varsayalım. Eğitim verileri için aynı aralığı kullanmak, ek bilgi vermeden yalnızca örnek sayısını şişirir. Bu durumda, daha iyi bir strateji 10 dakika veya bir saat iş gerekçesine dayalı üzerinden ortalama veri kullanmak olacaktır.

Statik veriler için,
- _Bakım kayıtları_: Ham bakım verilerinde, belirli bir zamanda gerçekleştirilen bakım faaliyetlerine ilişkin bilgileri içeren bir varlık tanımlayıcısı ve zaman damgası vardır. Bakım etkinliklerini, her kategori tanımlayıcının belirli bir bakım eylemiyle benzersiz bir şekilde eşleştiği _kategorik_ sütunlara dönüştürün. Bakım kayıtları için şema varlık tanımlayıcısı, zaman ve bakım eylem içerecektir.

- _Hata kayıtları_: Hata veya hata nedenleri belirli hata kodları veya belirli iş koşulları tarafından tanımlanan hata olayları olarak kaydedilebilir. Ekipmanın birden çok hata kodu na sahip olduğu durumlarda, etki alanı uzmanı hedef değişkenle ilgili olanları belirlemeye yardımcı olmalıdır. Bu hatalarla ilişkili tahmin _özellikleri_ oluşturmak için kalan hata kodlarını veya koşullarını kullanın. Hata kayıtları için şema varlık tanımlayıcısı, zaman, hata veya hata nedeni - varsa içerir.

- _Makine ve işleç meta verileri_: Bir varlığı işleticisi ile ilişkilendirmek için makine ve işleç verilerini tek bir şemada birleştirin. Makine koşulları şeması varlık tanımlayıcısı, varlık özellikleri, operatör tanımlayıcısı ve işleç özelliklerini içerir.

Diğer veri önişleme adımları _eksik değerleri işleme_ ve öznitelik değerlerinin _normalleştirilmesi_ içerir. Ayrıntılı bir tartışma bu kılavuzun kapsamı dışındadır - bazı yararlı başvurular için sonraki bölüme bakın.

Yukarıda önceden işlenmiş veri kaynakları nın devreye sokulmasıyla, özellik mühendisliğinden önceki son dönüşüm, varlık tanımlayıcısı ve zaman damgasına dayalı olarak yukarıdaki tablolara katılmaktır. Makine normal çalışma olduğunda ortaya çıkan tabloda hata sütunu için null değerleri olacaktır. Bu null değerleri normal çalışma için bir gösterge tarafından imputed olabilir. Tahminmodeli için etiketler oluşturmak için bu hata _sütununa_kullanın. Daha fazla bilgi için, [tahmine dayalı bakım için modelleme teknikleri bölümüne](#modeling-techniques-for-predictive-maintenance)bakın.

## <a name="feature-engineering"></a>Özellik mühendisliği
Özellik mühendisliği, verileri modellemeden önceki ilk adımdır. Veri bilimi sürecindeki rolü [burada açıklanmıştır.](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/create-features) _Bir özellik_ model için tahmine dayalı bir özelliktir - sıcaklık, basınç, titreşim ve benzeri. PdM için özellik mühendisliği, bir makinenin durumunu, büyük çesitli bir süre boyunca toplanan geçmiş veriler üzerinde soyutlamayı içerir. Bu anlamda uzaktan izleme, anomali tespiti ve arıza tespiti gibi akranlarından farklıdır. 

### <a name="time-windows"></a>Zaman pencereleri
Uzaktan izleme, zaman içinde noktalar itibariyle meydana gelen olayları _raporlamayı_gerektirir. Anomali algılama modelleri, gelen veri akışlarını zaman içinde puan olarak anomalileri işaretlemek için değerlendirir (puan). Hata algılama, zaman içinde nokta oluştukça belirli türlerde olmak için hataları sınıflar. Buna karşılık, _PdM, geçmiş zaman diliminde_makine davranışını temsil eden özelliklere dayalı olarak, gelecekteki bir zaman _dilimindeki_hataları tahmin etme içerir. PdM için, zaman bireysel noktalarından özellik verileri tahmin edilebilir olmak için çok gürültülü. Bu nedenle, her bir özelliğin verilerinin zaman pencerelerine veri noktaları toplayarak _düzeltilmesi_ gerekir.

### <a name="lag-features"></a>Gecikme özellikleri
İş gereksinimleri, modelin geleceğe ne kadar tahmin de etmesi gerektiğini tanımlar. Buna karşılık, bu süre, bu tahminleri yapmak için 'modelin ne kadar geriye görünmesi gerektiğini' tanımlamaya yardımcı olur. Bu 'geriye bakma' dönemine _gecikme_denir ve bu gecikme süresi boyunca tasarlanmış özelliklere _gecikme özellikleri_denir. Bu bölümde, zaman damgalı veri kaynaklarından oluşturulabilen gecikme özellikleri ve statik veri kaynaklarından özellik oluşturma tartışılır. Gecikme özellikleri genellikle doğada _sayısaldır._

> [!IMPORTANT]
> Pencere boyutu deneme yoluyla belirlenir ve bir etki alanı uzmanı yardımıyla sonuçlandırılmalıdır. Aynı uyarı, gecikme özelliklerinin, bunların toplamalarının ve pencerelerin türünün seçimi ve tanımı için de geçerlidir.

#### <a name="rolling-aggregates"></a>Yuvarlanan agregalar
Bir varlığın her kaydı için, toplamları hesaplamak için zaman birimlerinin sayısı olarak "W" boyutunda bir yuvarlanma penceresi seçilir. Gecikme özellikleri daha sonra bu kayıt _tarihinden önceki_ W dönemleri kullanılarak hesaplanır. Şekil 1'de, mavi çizgiler her bir zaman birimi için bir varlık için kaydedilen sensör değerlerini gösterir. W=3 boyutupenceresindeki özellik değerlerinin yuvarlanma ortalamalarını gösterirler. Yuvarlanma ortalaması, t<sub>1</sub> (turuncu) ile t<sub>2</sub> (yeşil) aralığındaki zaman damgaları ile tüm kayıtlar üzerinde hesaplanır. W değeri genellikle verilerin yapısına bağlı olarak dakika veya saat olarak dır. Ancak bazı sorunlar için, büyük bir W (örneğin 12 ay) seçmek, bir varlığın tüm geçmişini kayıt zamanına kadar sağlayabilir.

![Şekil 1. Yuvarlama toplu özellikleri](./media/predictive-maintenance-playbook/rolling-aggregate-features.png)

Şekil 1. Yuvarlama toplu özellikleri

Bir zaman penceresi içinde yuvarlanan agregalara örnek olarak sayım, ortalama, CUMESUM (kümülatif toplam) ölçüleri, min/max değerleri verilebilir. Buna ek olarak, varyans, standart sapma ve N standart sapmaları ötesinde outliers sayısı genellikle kullanılır. Bu kılavuzda [kullanım örnekleri](#sample-pdm-use-cases) için uygulanabilecek agrega örnekleri aşağıda listelenmiştir. 
- _Uçuş gecikmesi_: son gün/haftadaki hata kodlarının sayısı.
- _Uçak motoru parça arızası_: haddeleme anlamına gelir, standart sapma, ve toplamı son gün, hafta vb. Bu metrik, iş etki alanı uzmanı ile birlikte belirlenmelidir.
- _ATM hataları_: haddeleme araçları, medyan, aralık, standart sapmalar, üç standart sapma, üst ve alt CUMESUM ötesinde aykırı ların sayısı.
- _Metro tren kapı arızaları_: Geçmiş gün, hafta, iki hafta vb. olayların sayısı
- _Devre kesici arızaları_: Geçmiş hafta, yıl, üç yıl vb. hatalar sayar.

PdM'deki bir diğer yararlı teknik de, verilerdeki anormallikleri algılayan algoritmaları kullanarak eğilim değişikliklerini, ani artışları ve düzey değişikliklerini yakalamaktır.

#### <a name="tumbling-aggregates"></a>Yuvarlanan agregalar
Bir varlığın her etiketli kaydı için, _<sub>W-k</sub> _ boyutupenceresi tanımlanır ve _k,_ _W_boyutundaki pencere sayısıdır. Agregalar daha sonra bir kaydın zaman damgası önce dönemler için _k_ _yuvarlanan pencereler_ _W-k, W-<sub>(k-1)</sub>, ..., W-<sub>2</sub>, W-<sub>1</sub> _ üzerinde oluşturulur. _k_ kısa vadeli etkileri yakalamak için küçük bir sayı veya uzun vadeli bozulma desenleri yakalamak için çok sayıda olabilir. (bkz. Şekil 2).

![Şekil 2. Yuvarlanan toplam özellikleri](./media/predictive-maintenance-playbook/tumbling-aggregate-features.png)

Şekil 2. Yuvarlanan toplam özellikleri

Örneğin, rüzgar türbinleri kullanım durumda için gecikme özellikleri W=1 ve k=3 ile oluşturulabilir. Onlar üst ve alt outliers kullanarak son üç ay her biri için gecikme anlamına geliyor.

### <a name="static-features"></a>Statik özellikler

Üretim tarihi, model numarası, konum gibi ekipmanların teknik özellikleri statik özelliklerin bazı örnekleridir. Bunlar modelleme için _kategorik_ değişkenler olarak kabul edilir. Devre kesici kullanım örneğine örnek olarak gerilim, akım, güç kapasitesi, transformatör tipi ve güç kaynağı verilebilir. Tekerlek arızaları için, lastik jantların türü (alaşım vs çelik) bir örnektir.

Şimdiye kadar tartışılan veri hazırlama çabaları, aşağıda gösterildiği gibi düzenlenen verilere yol açmalıdır. Eğitim, test ve doğrulama verilerinde bu mantıksal şema olmalıdır (bu örnek, gün birimlerinde zamanı gösterir).

| Varlık Kimliği | Zaman | \<Özellik Sütunları> | Etiketle |
| ---- | ---- | --- | --- |
| A123 |1. Gün | . . . | . |
| A123 |2. Gün | . . . | . |
| ...  |...   | . . . | . |
| B234 |1. Gün | . . . | . |
| B234 |2. Gün | . . . | . |
| ...  |...   | . . . | . |

Özellik mühendisliğindeki son adım hedef değişkenin **etiketlemesidir.** Bu işlem modelleme tekniğine bağlıdır. Buna karşılık, modelleme tekniği iş sorununa ve kullanılabilir verilerin doğasına bağlıdır. Etiketleme sonraki bölümde ele alınmıştır.

> [!IMPORTANT]
> Veri hazırlama ve özellik mühendisliği, başarılı PDM çözümlerine ulaşmak için modelleme teknikleri kadar önemlidir. Etki alanı uzmanı ve uygulayıcısı model için doğru özellikleri ve verileri elde etmek için önemli zaman yatırım yapmalıdır. Özellik mühendisliği ile ilgili birçok kitaptan küçük bir örnek aşağıda listelenmiştir:
> - Pyle, D. Veri Madenciliği için Veri Hazırlama (Veri Yönetim Sistemlerinde Morgan Kaufmann Serisi), 1999
> - Zheng, A., Casari, A. Makine Öğrenimi Için Özellik Mühendisliği: Veri Bilimcileri için İlkeler ve Teknikler, O'Reilly, 2018.
> - Dong, G. Liu, H. (Editörler), Makine Öğrenimi ve Veri Analitiği Için Özellik Mühendisliği (Chapman & Hall/CRC Veri Madenciliği ve Bilgi Bulma Serisi), CRC Press, 2018.

## <a name="modeling-techniques-for-predictive-maintenance"></a>Tahmine dayalı bakım için modelleme teknikleri

Bu bölümde, PdM sorunları için ana modelleme teknikleri nin yanı sıra özel etiket yapım yöntemleri açıklanmaktadır. Tek bir modelleme tekniğinin farklı sektörlerde kullanılabileceğini unutmayın. Modelleme tekniği, eldeki verilerin bağlamından ziyade veri bilimi sorunuyla eşlenir.

> [!IMPORTANT]
> Arıza durumları için etiket seçimi ve etiketleme stratejisi  
> etki alanı uzmanına danışarak belirlenmelidir.

### <a name="binary-classification"></a>İkili sınıflandırma
İkili sınıflandırma, _bir ekipman parçasının gelecekteki bir zaman dilimi içinde başarısız olma olasılığını tahmin_ etmek için kullanılır - gelecekteki ufuk dönemi _X_olarak adlandırılır. X, etki alanı uzmanına danışarak iş sorunu ve eldeki veriler tarafından belirlenir. Örnekler şunlardır:
- bileşenleri değiştirmek, bakım kaynaklarını dağıtmak, o dönemde oluşabilecek bir sorunu önlemek için bakım gerçekleştirmek için gereken _minimum müşteri adayı süresi._
- bir sorun oluşmadan önce meydana gelebilecek _olayların en az sayısı._

Bu teknikte iki tür eğitim örneği tanımlanmışverilmiştir. Etiket = 1 ile _bir hata yılıman_olumlu bir örnek. Etiket = 0 ile normal işlemleri gösteren negatif bir örnek. Hedef değişken ve dolayısıyla etiket değerleri _kategoriktir._ Model, her yeni örneği bir sonraki X zaman birimleri üzerinde başarısız olma veya normal olarak çalışma olasılığı olarak tanımlamalıdır.

#### <a name="label-construction-for-binary-classification"></a>İkili sınıflandırma için etiket yapısı
Buradaki soru şudur: "Varlığın bir sonraki X biriminde başarısız olma olasılığı nedir?" Bu soruyu yanıtlamak için, bir varlığın başarısızlığa uğratılmasından önce X kayıtlarını "başarısız olmak üzere" olarak etiketle (etiket = 1) ve diğer tüm kayıtları "normal" (etiket =0) olarak etiketle. (bkz. Şekil 3).

![Şekil 3. İkili sınıflandırma için etiketleme](./media/predictive-maintenance-playbook/labelling-for-binary-classification.png)

Şekil 3. İkili sınıflandırma için etiketleme

Bazı kullanım örnekleri için etiketleme stratejisi örnekleri aşağıda listelenmiştir.
- _Uçuş gecikmeleri_: X, önümüzdeki 24 saat içinde gecikmeleri tahmin etmek için bir gün olarak seçilebilir. Daha sonra arızalardan önce 24 saat içinde olan tüm uçuşlar 1 olarak etiketlenir.
- _ATM nakit dağıtım hataları_: Amaç, bir işlemin sonraki bir saat içinde hata olasılığını belirlemek olabilir. Bu durumda, hatadan sonraki son saat içinde gerçekleşen tüm hareketler 1 olarak etiketlenir. Dağıtılan sonraki N para birimi notları üzerindeki hata olasılığını tahmin etmek için, bir hatanın son N notları içinde dağıtılan tüm notlar 1 olarak etiketlenir.
- _Devre kesici arızaları_: Amaç bir sonraki devre kesici komut uyruyanımını tahmin etmek olabilir. Bu durumda, X gelecekteki bir komut olarak seçilir.
- _Tren kapısı arızaları_: X iki gün olarak seçilebilir.
- _Rüzgar türbini arızaları_: X iki ay olarak seçilebilir.

### <a name="regression-for-predictive-maintenance"></a>Tahmine dayalı bakım için regresyon
Regresyon modelleri, _bir varlığın kalan yararlı ömrünü (RUL) hesaplamak_için kullanılır. RUL, bir varlığın bir sonraki hata oluşmadan önce çalışır durumda olduğu süre olarak tanımlanır. Her eğitim örneği, _n'nin_ birden çok olduğu bir varlık için bir zaman birimi _nY'ye_ ait bir kayıttır. Model, her yeni örneğin RUL'ini sürekli bir _sayı_olarak hesaplamalıdır. Bu sayı, hatadan önce kalan süreyi gösterir.

#### <a name="label-construction-for-regression"></a>Regresyon için etiket yapısı
Buradaki soru şudur: "Ekipmanın kalan yararlı ömrü (RUL) nedir?" Hatadan önceki her kayıt için, etiketi bir sonraki hatadan önce kalan süre birimi sayısı olarak hesaplayın. Bu yöntemde, etiketler sürekli değişkenlerdir. (Bkz. Şekil 4)

![Şekil 4. Regresyon için etiketleme](./media/predictive-maintenance-playbook/labelling-for-regression.png)

Şekil 4. Regresyon için etiketleme

Regresyon için, etiketleme bir hata noktasına atıfta bulunarak yapılır. Bu hesaplama, varlığın bir başarısızlıktan önce ne kadar süre hayatta kaldığını bilmeden mümkün değildir. Bu nedenle ikili sınıflandırmanın aksine, verilerde herhangi bir hata olmayan varlıklar modelleme için kullanılamaz. Bu sorun en iyi [Survival Analysis](https://en.wikipedia.org/wiki/Survival_analysis)denilen başka bir istatistiksel teknik tarafından ele alınmıştır. Ancak bu tekniği niçin pdm kullanımına uygularken sık aralıklarla zaman değişen veriler içeren olası komplikasyonlar ortaya çıkabilir. Survival Analysis hakkında daha fazla bilgi için [bu tek sayfalık](https://www.cscu.cornell.edu/news/news.php/stnews78.pdf)sayfaya bakın.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Tahmine dayalı bakım için çok sınıflı sınıflandırma
Çok sınıflı sınıflandırma teknikleri iki senaryo için PdM çözümlerinde kullanılabilir:
- _Gelecekteki iki sonucu_tahmin edin: İlk sonuç, bir varlığın başarısız olması için bir dizi _zaman_ aralığıdır. Kıymet, birden çok olası zaman döneminden birine atanır. İkinci sonuç, _birden çok kök nedenlerden biri_nedeniyle gelecek bir dönemde başarısızlık olasılığıdır. Bu tahmin, bakım ekibinin semptomları izlemesini ve bakım programlarını planlamasına olanak tanır.
- Belirli bir _hatanın en olası kök nedenini_ tahmin edin. Bu sonuç, bir hatayı düzeltmek için doğru bakım eylemleri kümesini önerir. Kök nedenlerin ve önerilen onarımların sıralanmış bir listesi, teknisyenlerin bir hatadan sonra onarım eylemlerine öncelik belirlemelerine yardımcı olabilir.

#### <a name="label-construction-for-multi-class-classification"></a>Çok sınıflı sınıflandırma için etiket yapımı
Buradaki soru şudur: "Bir varlığın bir sonraki _nZ_ zaman birimlerinde _n_ başarısız olma olasılığı nedir? Bu soruyu yanıtlamak için, bir varlığın zaman kovalarını (3Z, 2Z, Z) kullanmamasından önce nZ kayıtlarını etiketle. Diğer tüm kayıtları "normal" (etiket = 0) olarak etiketle. Bu yöntemde, hedef değişken _kategorik_ değerleri tutar. (Bkz. Şekil 5).

![Şekil 5. Çok sınıflı sınıflandırma için hata zamanı tahmin etiketleri](./media/predictive-maintenance-playbook/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Şekil 5. Hata zamanı tahmini için çok sınıflı sınıflandırma için etiketleme

Buradaki soru şudur: "Varlığın kök neden/problem _P<sub>i</sub>_ nedeniyle bir sonraki X biriminde başarısız olma olasılığı nedir?" _nerede i_ olası kök nedenleri sayısıdır. Bu soruyu yanıtlamak için, bir varlığın başarısızlığından önce X kayıtlarını "kök neden _P<sub>i</sub>_" (etiket = _P<sub>i)</sub>_ nedeniyle başarısız olmak üzere olarak etiketle. Diğer tüm kayıtları "normal" olarak etiketle (etiket = 0). Bu yöntemde de etiketler kategoriktir (Bkz. Şekil 6).

![Şekil 6. Çok sınıflı sınıflandırma için kök neden tahmin etiketleri](./media/predictive-maintenance-playbook/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Şekil 6. Kök neden tahmini için çok sınıflı sınıflandırma için etiketleme

Model, her _P<sub>i'den</sub> _ kaynaklanan bir hata olasılığı ve hata olmaması olasılığını atar. Bu olasılıklar, gelecekte ortaya çıkma olasılığı en yüksek olan sorunların tahmin edilmesine olanak sağlamak için büyüklük olarak sıralanabilir.

Buradaki soru şudur: "Bir arızadan sonra hangi bakım işlemlerini önerirsiniz?" Bu soruyu yanıtlamak için, modelleme, model gelecekte başarısızlığı tahmin etmediğinden, _gelecekteki bir ufkun seçilmesini gerektirmez._ Sadece _başarısızlık zaten oldu kez_en olası kök nedeni tahmin ediyor.

## <a name="training-validation-and-testing-methods-for-predictive-maintenance"></a>Tahmine dayalı bakım için eğitim, doğrulama ve test yöntemleri
[Takım Veri Bilimi Süreci,](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) model tren-test doğrulama döngüsünün tam kapsamını sağlar. Bu bölümde PdM'ye özgü yönleri anlatılmaktadır.

### <a name="cross-validation"></a>Çapraz doğrulama
[Çapraz doğrulamanın](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) amacı, eğitim aşamasında modeli "sınamak" için bir veri kümesi tanımlamaktır. Bu veri kümesidoğrulama _kümesi_olarak adlandırılır. Bu _teknik, aşırı montaj_ gibi sorunları sınırlamaya yardımcı olur ve modelin bağımsız bir veri kümesine nasıl genelleştireceği hakkında bir fikir verir. Yani, bilinmeyen bir veri kümesi, gerçek bir sorun olabilir. PDM için eğitim ve test rutini, görünmeyen gelecekteki veriler üzerinde daha iyi genelleme yapmak için zaman değişen yönleri dikkate almalıdır.

Birçok makine öğrenme algoritması, model performansını önemli ölçüde değiştirebilecek bir dizi hiperparametreye bağlıdır. Bu hiperparametrelerin en uygun değerleri modeli eğitirken otomatik olarak hesaplanmaz. Bunlar veri bilimci tarafından belirtilmelidir. Hiperparametrelerin iyi değerlerini bulmanın çeşitli yolları vardır.

En yaygın olanı, örnekleri rasgele _k_ kıvrımlarına bölen _k-kat çapraz doğrulamadır._ Her hiperparametre değeri kümesi için, öğrenme algoritması _k_ kez çalıştırın. Her yinelemede, geçerli kıvrımdaki örnekleri doğrulama kümesi olarak, diğer örnekleri ise bir eğitim kümesi olarak kullanın. Algoritmayı eğitim örnekleri üzerinde eğitin ve doğrulama örnekleri üzerinden performans ölçümlerini hesapla. Bu döngünün sonunda, _k_ performans ölçümlerinin ortalamasını hesapla. Her hiperparametre değeri kümesi için, en iyi ortalama performansa sahip olanları seçin. Hiperparametreleri seçme görevi genellikle doğada deneyseldir.

PdM sorunlarında, veriler çeşitli veri kaynaklarından gelen bir zaman dizisi olarak kaydedilir. Bu kayıtlar etiketleme zamanına göre sıralanabilir. Bu nedenle, veri kümesi _rasgele_ eğitim ve doğrulama kümesine bölünürse, bazı eğitim örnekleri bazı _doğrulama örneklerinden daha geç zaman içinde olabilir._ Hiperparametre değerlerinin gelecekteki performansı, model eğitilmeden _önce_ gelen bazı verilere göre tahmin edilecektir. Bu tahminler, özellikle zaman serisi sabit değilse ve zaman içinde gelişiyorsa, aşırı iyimser olabilir. Sonuç olarak, seçilen hiperparametre değerleri suboptimal olabilir.

Önerilen yol, örnekleri _zamana bağlı_ bir şekilde belirlenen eğitim ve doğrulamaya bölmektir ve tüm doğrulama örnekleri tüm eğitim örneklerinden daha geç zamanda gelir. Her hiperparametre değerleri kümesi için algoritmayı eğitim veri kümesi üzerinde eğitin. Modelin performansını aynı doğrulama kümesi üzerinde ölçün. En iyi performansı gösteren hiperparametre değerlerini seçin. Tren/doğrulama bölünmesi ile seçilen hiperparametre değerleri, çapraz doğrulama tarafından rasgele seçilen değerlere göre daha iyi bir gelecek modeli performansı sağlar.

Son model, en iyi hiperparametre değerlerini kullanarak tüm eğitim verilerinin üzerinde bir öğrenme algoritması eğitilerek oluşturulabilir.

### <a name="testing-for-model-performance"></a>Model performansı için test
Bir model oluşturultuktan sonra, yeni veriler üzerindeki gelecekteki performansının tahmini gereklidir. İyi bir tahmin, doğrulama kümesi üzerinden hesaplanan hiperparametre değerlerinin performans ölçüsü veya çapraz doğrulamadan hesaplanan ortalama bir performans ölçüsüdür. Bu tahminler genellikle aşırı iyimser. İşletmenin genellikle modeli nasıl sınamak istediklerine ilişkin bazı ek yönergeleri olabilir.

PdM için önerilen yol, örnekleri _zamana bağlı_ bir şekilde eğitim, doğrulama ve test veri kümelerine bölmektir. Tüm test örnekleri, tüm eğitim ve doğrulama örneklerinden daha geç zaman içinde olmalıdır. Bölmeden sonra, modeli oluşturun ve daha önce açıklandığı gibi performansını ölçün.

Zaman serileri sabit ve tahmin etmesi kolay olduğunda, hem rasgele hem de zamana bağlı yaklaşımlar gelecekteki performansın benzer tahminlerini oluşturur. Ancak zaman serileri sabit olmadığında ve/veya tahmin etmesi zor olduğunda, zamana bağlı yaklaşım gelecekteki performansa ilişkin daha gerçekçi tahminler oluşturur.

### <a name="time-dependent-split"></a>Zamana bağlı bölme
Bu bölümde, zamana bağlı bölme uygulamak için en iyi uygulamaları açıklanır. Eğitim ve test kümeleri arasında zamana bağlı iki yönlü bölünmüş aşağıda açıklanmıştır.

Çeşitli sensörlerden ölçümler gibi zaman damgalı olaylar akışı varsayalım. Birden çok olay içeren zaman dilimleri nde eğitim ve test örneklerinin özelliklerini ve etiketlerini tanımlayın. Örneğin, ikili sınıflandırma için, geçmiş olaylara dayalı özellikler oluşturun ve gelecekte "X" zaman birimleri içindeki gelecekteki olaylara dayalı etiketler oluşturun [(özellik mühendisliği](#feature-engineering) ve modelleme teknikleri bölümlerine bakın). Bu nedenle, bir örneğin etiketleme zaman dilimi özelliklerinin zaman diliminden daha geç gelir.

Zamana bağlı bölme için, bir modeli eğitmek için bir _eğitim kesme süresi T<sub>c</sub> _ seçin, t<sub>c'ye</sub>kadar geçmiş veriler kullanılarak ayarlanmış hiperparametreler. T<sub>c'nin</sub> ötesindeki gelecekteki etiketlerin eğitim verilerine sızmasını önlemek için, eğitim örneklerini T<sub>c'den</sub>önce X birimi olarak etiketlemek için en son zamanı seçin. Şekil 7'de gösterilen örnekte, her kare, özelliklerin ve etiketlerin yukarıda açıklandığı şekilde hesaplandığı veri kümesindeki bir kaydı temsil eder. Şekil, X=2 ve W=3 için eğitim ve test kümelerine gitmesi gereken kayıtları gösterir:

![Şekil 7. İkili sınıflandırma için zamana bağlı bölme](./media/predictive-maintenance-playbook/time-dependent-split-for-binary-classification.png)

Şekil 7. İkili sınıflandırma için zamana bağlı bölme

Yeşil kareler, eğitim için kullanılabilecek zaman birimlerine ait kayıtları temsil eder. Her eğitim örneği özellik üretimi için son üç dönem ve T<sub>c'den</sub>önce etiketleme için iki sonraki dönem göz önünde bulundurularak oluşturulur. Gelecek iki dönemin herhangi bir bölümü T<sub>c'nin</sub>ötesinde olduğunda, t<sub>c'nin</sub>ötesinde görünürlük olmadığı için bu örneği eğitim veri setinden hariç tutabilirsiniz.

Siyah kareler, yukarıdaki kısıtlama göz önüne alındığında, eğitim veri kümesinde kullanılmaması gereken son etiketli veri kümesinin kayıtlarını temsil eder. Bu kayıtlar, T<sub>c'den</sub>önce olduğu için verileri test etmede de kullanılmayacaktır. Buna ek olarak, etiketleme zaman dilimleri kısmen ideal olmayan eğitim süresi çerçevesine bağlıdır. Eğitim ve test verilerinin, etiket bilgilerinin sızmasını önlemek için ayrı etiketleme zaman dilimleri olmalıdır.

Şimdiye kadar tartışılan teknik, T<sub>c'ye</sub>yakın zaman damgaları olan eğitim ve test örnekleri arasında örtüşmeye olanak sağlar. Daha fazla ayırma elde etmek için bir çözüm test kümesinden T<sub>c</sub> W zaman birimleri içinde olan örnekleri dışlamaktır. Ancak böyle agresif bir bölünme yeterli veri kullanılabilirliğine bağlıdır.

RUL'u tahmin etmek için kullanılan regresyon modelleri sızıntı sorunundan daha ciddi şekilde etkilenir. Rasgele bölme yöntemini kullanarak aşırı montaj yol açar. Gerileme sorunları için, bölünme, T<sub>c</sub> eğitim setine girmeden önce arızalı varlıklara ait kayıtların olması gerekir. Kesmeden sonra hataları olan varlıkların kayıtları test kümesine gider.

Eğitim ve sınama için veri bölme için başka bir en iyi yöntem varlık kimliği ile bir bölme kullanmaktır. Bölme, eğitim kümesinde kullanılan varlıkların hiçbirinin model performansını test etmede kullanılmaması gibi olmalıdır. Bu yaklaşımı kullanarak, bir modelin yeni varlıklarla daha gerçekçi sonuçlar sağlama şansı daha yüksektür.

### <a name="handling-imbalanced-data"></a>Dengesiz verileri işleme
Sınıflandırma problemlerinde, bir sınıfın diğerlerinden daha fazla örneği varsa, veri kümesinin _dengesiz_olduğu söylenir. İdeal olarak, farklı sınıflar arasında farklılaşmayı sağlamak için eğitim verilerinde her sınıftan yeterli sayıda temsilci tercih edilir. Bir sınıf verilerin %10'undan azsa, veriler dengesiz olarak kabul edilir. Az temsil edilen sınıfa _azınlık sınıfı_denir.

Birçok PdM sorunu, bir sınıfın diğer sınıfa veya sınıflara göre ciddi şekilde yeterince temsil edilemesiyle bu tür dengesiz veri kümeleri ile karşı karşıya dır. Bazı durumlarda, azınlık sınıfı toplam veri noktalarının yalnızca %0,001'ini oluşturabilir. Sınıf dengesizliği PDM'ye özgü değildir. Hataların ve anormalliklerin nadir olduğu diğer etki alanları da benzer bir sorunla karşı karşıya dır, örneğin, sahtekarlık tespiti ve ağ izinsiz girişi. Bu başarısızlıklar azınlık sınıfı örneklerini ortaya çıkarmaktadır.

Verilerdeki sınıf dengesizliği ile, genel hata oranını en aza indirmeyi amaçladıkları için çoğu standart öğrenme algoritmasının performansı tehlikeye girer. %99 negatif ve %1 pozitif örnekleriçeren bir veri seti için, bir modelin tüm örnekleri negatif olarak etiketleyerek %99 doğruluk gösterdiği gösterilebilir. Ama model yanlış sınıflandırmak tüm olumlu örnekler; bu yüzden doğruluğu yüksek olsa bile, algoritma yararlı bir değildir. Sonuç olarak, _hata oranı_ genel doğruluk gibi geleneksel değerlendirme ölçümleri dengesiz öğrenme için yetersizdir. Dengesiz veri kümeleriyle karşılaştığında, model değerlendirmesi için diğer ölçümler kullanılır:
- Duyarlık
- Geri Çağırma
- F1 puanları
- Maliyet ayarlı ROC (alıcı çalışma özellikleri)

Bu ölçümler hakkında daha fazla bilgi için [model değerlendirmesine](#model-evaluation)bakın.

Ancak, sınıf dengesizliği sorunu gidermek yardımcı bazı yöntemler vardır. İki önemli olanlar _örnekleme teknikleri_ ve maliyet duyarlı _öğrenme_vardır.

#### <a name="sampling-methods"></a>Örnekleme yöntemleri
Dengesiz öğrenme, eğitim veri kümesini dengeli bir veri kümesiolarak değiştirmek için örnekleme yöntemlerinin kullanımını içerir. Örnekleme yöntemleri test kümesine uygulanmamalıdır. Çeşitli örnekleme teknikleri olmasına rağmen, en düz ileri olanlar _rasgele örnekleme_ ve _örnekleme altında._

_Rasgele örnekleme,_ azınlık sınıfından rastgele bir örnek seçmeyi, bu örnekleri çoğaltmayı ve bunları eğitim veri kümesine eklemeyi içerir. Sonuç olarak, azınlık sınıfındaki örneklerin sayısı artar ve sonunda farklı sınıfların örnek lerinin sayısı dengelenir. Aşırı örneklemenin bir dezavantajı, belirli örneklerin birden çok örneğinin sınıflandırıcının çok belirgin olmasına ve aşırı montaja yol açmasıdır. Model yüksek eğitim doğruluğu gösterebilir, ancak görünmeyen test verileri üzerindeki performansı yetersiz olabilir.

Tersine, _rasgele örnekleme altında_ bir çoğunluk sınıfından rasgele bir örnek seçiyor ve bu örnekleri eğitim veri kümesinden kaldırıyor. Ancak, örnekleri çoğunluk sınıfından kaldırmak, sınıflandırıcının çoğunluk sınıfıyla ilgili önemli kavramları kaçırmasına neden olabilir. Azınlık sınıfının aşırı örneklendiği ve çoğunluk sınıfının aynı anda az örneklendiği _hibrit örnekleme_ de başka bir uygulanabilir yaklaşımdır.

Birçok gelişmiş örnekleme teknikleri vardır. Seçilen teknik, veri bilimcinin yinelemeli deneylerinin veri özelliklerine ve sonuçlarına bağlıdır.

#### <a name="cost-sensitive-learning"></a>Maliyetduyarlı öğrenme
PDM'de azınlık sınıfını oluşturan hatalar normal örneklerden daha fazla ilgi çekicidir. Yani odak noktası daha çok algoritmanın hatalar üzerindeki performansıdır. Pozitif bir sınıfı negatif sınıf olarak yanlış tahmin etmek, tam tersi bir maliyetten daha pahalıya mal olabilir. Bu durum genellikle eşit olmayan kayıp veya farklı sınıflara yanlış sınıflandırma elemanlarının asimetrik maliyet olarak adlandırılır. İdeal sınıflandırıcı, çoğunluk sınıfının doğruluğundan ödün vermeden azınlık sınıfı üzerinde yüksek tahmin doğruluğu sağlamalıdır.

Bu dengeyi sağlamak için birden çok yol vardır. Eşit olmayan kayıp sorununu azaltmak için, azınlık sınıfının yanlış sınıflandırılmasına yüksek bir maliyet atayın ve toplam maliyeti en aza indirmeye çalışın. _SVM'ler (Destek Vektör Makineleri)_ gibi algoritmalar, eğitim sırasında pozitif ve olumsuz örneklerin maliyetinin belirtilmesine izin vererek bu yöntemi doğal olarak benimser. Benzer şekilde, _artırılmış karar ağaçları_ gibi artırıcı yöntemler genellikle dengesiz verilerle iyi performans gösterir.

## <a name="model-evaluation"></a>Model değerlendirmesi
Yanlış sınıflandırma, işletmeye yanlış alarm maliyetinin yüksek olduğu PdM senaryoları için önemli bir sorundur. Örneğin, bir uçağı motor arızasının yanlış bir tahminini temel alan bir yere topraklama kararı zamanlamaları ve seyahat planlarını bozabilir. Bir makineyi montaj hattından çevrimdışı almak gelir kaybına neden olabilir. Bu nedenle, yeni test verilerine karşı doğru performans ölçümleriyle model değerlendirmesi çok önemlidir.

PdM modellerini değerlendirmek için kullanılan tipik performans ölçümleri aşağıda açıklanmıştır:

- [Doğruluk,](https://en.wikipedia.org/wiki/Accuracy_and_precision) bir sınıflandırıcının performansını tanımlamak için kullanılan en popüler ölçümdür. Ancak doğruluk veri dağıtımlarına duyarlıdır ve dengesiz veri kümeleri olan senaryolar için etkisiz bir ölçüdür. Bunun yerine diğer ölçümler kullanılır. [Karışıklık matrisi](https://en.wikipedia.org/wiki/Confusion_matrix) gibi araçlar, modelin doğruluğunu hesaplamak ve mantık la ilgili olarak kullanılır.
- PdM modellerinin [hassasiyeti](https://en.wikipedia.org/wiki/Precision_and_recall) yanlış alarm oranıyla ilgilidir. Modelin daha düşük hassasiyeti genellikle daha yüksek bir yanlış alarm oranına karşılık gelir.
- [Geri çekme](https://en.wikipedia.org/wiki/Precision_and_recall) oranı, test kümesindeki hatalardan kaç tanesinin model tarafından doğru şekilde tanımlandığını gösterir. Daha yüksek geri çağırma oranları, modelin gerçek hataları tanımlamada başarılı olduğu anlamına gelir.
- [F1 skoru,](https://en.wikipedia.org/wiki/F1_score) değeri 0 (en kötü) ile 1 (en iyi) arasında değişen hassaslık ve hatırlamanın harmonik ortalamasıdır.

İkili sınıflandırma için,
- [Alıcı çalışma eğrileri (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) da popüler bir metriktir. ROC eğrilerinde, model performansı ROC'taki tek bir sabit çalışma noktasına göre yorumlanır.
- Ama PdM sorunları için, _decile tablolar_ ve _asansör grafikleri_ daha bilgilendirici. Yalnızca pozitif sınıfa (hatalara) odaklanırlar ve algoritma performansının ROC eğrilerinden daha karmaşık bir resmini sağlarlar.
  - _Decile tabloları,_ hata olasılıklarının azalan sırasına göre test örnekleri kullanılarak oluşturulur. Daha sonra sıralanan numuneler deciles (en yüksek olasılık, daha sonra% 20, % 30, vb) ile örneklerin% 10 olarak gruplanır. Her decile için oran (gerçek pozitif oran)/(rasgele taban çizgisi) her decile algoritma performansını tahmin yardımcı olur. Rasgele taban çizgisi 0.1, 0.2 ve benzeri değerleri alır.
  - [Asansör grafikleri](http://www2.cs.uregina.ca/~dbd/cs831/notes/lift_chart/lift_chart.html) tüm deciles için rasgele doğru pozitif oranı karşı decile gerçek pozitif oranı arsa. En büyük kazanımları gösterdikleri için ilk deciles genellikle sonuçların odak noktasıdır. PdM için kullanıldığında ilk deciles "risk altında" için temsilcisi olarak da görülebilir.

## <a name="model-operationalization-for-predictive-maintenance"></a>Tahmine dayalı bakım için model operasyonelizasyonu

Veri bilimi alıştırmasının yararı, yalnızca eğitilmiş model faaliyete geçtiğinde gerçekleştirilir. Diğer bir diğer olarak, modelin yeni, daha önce görünmeyen verilere dayalı öngörüler yapmak için iş sistemlerine dağıtılması gerekir.  Yeni veriler, eğitilmiş modelin _model imzasına_ tam olarak iki şekilde uymalıdır:
- tüm özellikler, yeni verilerin her mantıksal örneğinde (tabloda bir satır deyin) bulunmalıdır.
- yeni veriler önceden işlenmeli ve her bir özellik, eğitim verileriyle aynı şekilde tasarlanmalıdır.

Yukarıdaki süreç akademik ve endüstri literatüründe birçok yönden belirtilmiştir. Ama tüm aşağıdaki ifadeler aynı anlama gelir:
- Modeli kullanarak _yeni veri puan_
- _Modeli_ yeni verilere uygulama
- Modeli _operasyonel hale_
- Modeli _dağıtma_
- _Modeli_ yeni verilere karşı çalıştırma

Daha önce de belirtildiği gibi, PDM için model operasyonelleştirme emsallerinden farklıdır. Anormallik tespiti ve hata tespiti içeren senaryolar genellikle _çevrimiçi puanlamayı_ uygular _(gerçek zamanlı puanlama_olarak da adlandırılır). Burada, model gelen her kaydı _puanlar_ ve bir tahmin döndürür. Anomali tespiti için tahmin, bir anomalinin oluştuğunun bir göstergesidir (Örnek: Tek sınıf SVM). Hata algılama için, hata türü veya sınıfı olacaktır.

Buna karşılık, PdM _toplu puanlama_içerir. Model imzasına uymak için, yeni verilerdeki özelliklerin eğitim verileriyle aynı şekilde tasarlanmış olması gerekir. Yeni veriler için tipik olan büyük veri kümeleri için özellikler zaman pencerelerinde toplanır ve toplu olarak puanlandırılır. Toplu puanlama genellikle [Spark](https://spark.apache.org/) veya Azure [Toplu İşlemgibi](https://docs.microsoft.com/azure/batch/batch-api-basics)dağıtılmış sistemlerde yapılır. Birkaç alternatif vardır - her ikisi de suboptimal:
- Veri akitleri, bellekteki pencereler üzerinde toplamayı destekler. Bu yüzden online puanlama destek iddia edilebilir. Ancak bu sistemler, dar zaman pencerelerinde yoğun veriler veya daha geniş pencereler üzerindeki seyrek öğeler için uygundur. PdM senaryolarında görüldüğü gibi, daha geniş zaman pencerelerinde yoğun veriler için iyi ölçeklendirme olmayabilirler.
- Toplu puanlama yoksa, çözüm, bir seferde küçük gruplar halinde yeni verileri işlemek için çevrimiçi puanlama uyarlamaktır.

## <a name="solution-templates-for-predictive-maintenance"></a>Tahmine dayalı bakım için çözüm şablonları

Bu kılavuzun son bölümü, Azure'da uygulanan PdM çözüm şablonlarının, öğreticilerin ve denemelerin bir listesini sağlar. Bu PdM uygulamaları bazı durumlarda birkaç dakika içinde Azure aboneliğine dağıtılabilir. Bunlar, alternatiflerle denemeler yapmak için kavram kanıtı demoları, kum kutuları veya gerçek üretim uygulamaları için hızlandırıcılar olarak kullanılabilir. Bu şablonlar [Azure AI Galerisi'nde](https://gallery.azure.ai) veya [Azure GitHub'da](https://github.com/Azure)bulunur. Bu farklı örnekler zaman içinde bu çözüm şablonuna yuvarlanır.

| # | Başlık | Açıklama |
|--:|:------|-------------|
| 2 | [Azure Tahmine Dayalı Bakım Çözümü Şablonu](https://github.com/Azure/AI-PredictiveMaintenance) | Azure ML modellemesini ve IoT uzaktan izleme bağlamında Tahmine Dayalı Bakım senaryolarını destekleyebilecek eksiksiz bir Azure altyapısını gösteren açık kaynak kodlu bir çözüm şablonu. |
| 3 | [Tahmine Dayalı Bakım için Derin Öğrenme](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) | Öngörülü Bakım için LSTM (Uzun Kısa Süreli Bellek) ağlarını (Bir dizi Tekrarlayan Sinir Ağları) kullanmanın demo çözümüne sahip Azure Notebook, [bu örnekte](https://azure.microsoft.com/blog/deep-learning-for-predictive-maintenance)bir blog yazısı ile .|
| 4 | [R'de Tahmine Dayalı Bakım Modelleme Kılavuzu](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) | R komut dosyaları ile PdM modelleme kılavuzu.|
| 5 | [Havacılık için Azure Öngörübakım](https://gallery.azure.ai/Solution/Predictive-Maintenance-for-Aerospace-1) | Uçak bakımı için Azure ML v1.0'ı temel alan ilk PdM çözüm şablonlarından biri. Bu kılavuz bu projeden kaynaklanmaktadır. |
| 6 | [IoT Edge için Azure AI Araç Seti](https://github.com/Azure/ai-toolkit-iot-edge) | TensorFlow kullanarak IoT Edge'de AI; araç seti, Azure IoT Edge uyumlu Docker kaplarında derin öğrenme modellerini paketler ve bu modelleri REST API'leri olarak ortaya çıkarır.
| 7 | [Azure IoT Tahmine Dayalı Bakım](https://github.com/Azure/azure-iot-predictive-maintenance) | Azure IoT Suite PCS - Önceden Yapılandırılmış Çözüm. IoT Suite ile uçak bakım PdM şablonu. Aynı projeyle ilgili [başka bir belge](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-overview) ve gözden [geçirme.](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-walkthrough) |
| 8 | [SQL Server R Hizmetlerini Kullanarak Tahmine Dayalı Bakım şablonu](https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1) | R hizmetlerine dayalı kalan yararlı yaşam senaryosudemo. |
| 9 | [Öngörülebilir Bakım Modelleme Kılavuzu](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) | Uçak bakım veri seti özelliği, [denemeler](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Experiment-1) ve [veri kümeleri](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Data-Sets-1) ve [Azure dizüstü bilgisayar](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) ile R kullanılarak tasarlanmış ve AzureML v1.0'daki [denemeler](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2)|

## <a name="training-resources-for-predictive-maintenance"></a>Tahmine dayalı bakım için eğitim kaynakları

Microsoft Azure, genel AI kavramları ve uygulamaları yla ilgili içerik ve eğitimin yanı sıra PdM tekniklerinin arkasındaki temel kavramlar için öğrenme yolları sunar.

| Eğitim kaynağı  | Kullanılabilirlik |
|:-------------------|--------------|
| [Ağaçlar ve Rastgele Orman Kullanarak PdM için Öğrenme Yolu](https://aischool.microsoft.com/learning-paths/1H5vH5wAYcAy88CoQWQcA8) | Genel | 
| [Derin Öğrenme yi kullanarak PdM için Öğrenme Yolu](https://aischool.microsoft.com/learning-paths/FSIXxYkOGcauo0eUO8qAS) | Genel |
| [Azure'da AI Geliştiricisi](https://azure.microsoft.com/training/learning-paths/azure-ai-developer) | Genel |
| [Microsoft AI Okulu](https://aischool.microsoft.com/learning-paths) | Genel |
| [GitHub'dan Azure AI Öğrenimi](https://github.com/Azure/connectthedots/blob/master/readme.md) | Genel |
| [LinkedIn Learning](https://www.linkedin.com/learning) | Genel |
| [Microsoft AI YouTube Web seminerleri](https://www.youtube.com/watch?v=NvrH7_KKzoM&t=4s) | Genel |
| [Microsoft AI Göster](https://channel9.msdn.com/Shows/AI-Show) | Genel |
| [LearnAI@MS](https://learnanalytics.microsoft.com) | İş Ortakları |
| [Microsoft İş Ortağı Ağı](https://partner.microsoft.com/training/training-center) | İş Ortakları |

Buna ek olarak, Ücretsiz MOOCS (büyük açık online kurslar) AI Stanford ve MIT gibi akademik kurumlar tarafından online olarak sunulmaktadır, ve diğer eğitim şirketleri.
