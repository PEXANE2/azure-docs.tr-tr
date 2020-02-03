---
title: Tahmine dayalı bakım çözümleri - Team Data Science Process için Azure yapay ZEKA Kılavuzu
description: Veri bilimi, Tahmine dayalı bakım çözümleri birden çok dikey sektörler çalışmasını sağlayan kapsamlı bir açıklaması.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=fboylu, previous-ms.author=fboylu
ms.openlocfilehash: 5cd16280ba942404ffb23fd1c9d0e1a20af8c7c4
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721821"
---
# <a name="azure-ai-guide-for-predictive-maintenance-solutions"></a>Tahmine dayalı bakım çözümleri için Azure yapay ZEKA Kılavuzu

## <a name="summary"></a>Özet

Tahmine dayalı bakım (**PDM**), çeşitli sektörlerdeki işletmelerin operasyonel maliyetlerde yüksek miktarda varlık kullanımı ve tasarruf elde etmelerini sağlayan, tahmine dayalı analizler için popüler bir uygulamadır. Bu kılavuz, [MICROSOFT Azure AI platformu](https://azure.microsoft.com/overview/ai-platform) teknolojisini kullanarak PDM çözümlerini başarıyla geliştirmek ve dağıtmak için iş ve analitik yönergeleri ve en iyi yöntemleri sunar.

Yeni başlayanlar için bu kılavuz, sektöre özgü iş senaryoları ve bu senaryolar için PdM uygun işlemi sunar. Modelleme ve veri gereksinimleri PdM çözümler oluşturmak için de sağlanır. Ana Kılavuzu'nun veri hazırlama, özellik Mühendisliği, model oluşturma ve modeli kullanıma hazır hale getirme adımları dahil olmak üzere veri bilimi süreci üzerinde - içeriktir. Bu anahtar kavramlar tamamlamak için bu kılavuzu bir dizi PdM uygulama geliştirmeyi hızlandırmak için çözüm şablonu listeler. Kılavuzu, ayrıca pratik AI veri bilimi arkasında hakkında daha fazla bilgi edinmek için yararlı eğitim kaynaklarına işaret eder. 

### <a name="data-science-guide-overview-and-target-audience"></a>Veri bilimi Kılavuzu genel bakış ve hedef kitle
Bu kılavuzun ilk yarısında, bu sorunları gidermeye yönelik PdM uygulama avantajları, tipik iş sorunlarını açıklar ve bazı ortak kullanım durumları listeler. İş karar mekanizmalarına (BDMs) bu içeriğe ilişkin yararlı olacaktır. İkinci yarısında hiç veri bilimi PdM arkasında açıklar ve bu kılavuzda gösterilen ilkeleri kullanılarak oluşturulan PdM çözümlerinin bir listesini sağlar. Öğrenme yolları ve eğitim malzemesi işaretçileri de sağlar. Teknik karar verenler (TDMs) bu içeriğin yararlı bulabilirsiniz.

| İle Başlat... | Eğer... |
|:---------------|:---------------|
| [Tahmine dayalı bakım için iş durumu](#business-case-for-predictive-maintenance) |kapalı kalma süresi ve işletim giderlerini azaltmak ve donanım kullanımını iyileştirmek için arayan bir iş karar veren (BDM) |
| [Tahmine dayalı bakım için veri bilimi](#data-science-for-predictive-maintenance) |benzersiz veri işleme ve Tahmine dayalı bakım için yapay ZEKA gereksinimleri anlamak için PdM teknolojilerini değerlendirirken bir teknik karar veren (TDM) |
| [Tahmine dayalı bakım için çözüm şablonları](#solution-templates-for-predictive-maintenance)|bir yazılım Mimarı veya hızlı bir tanıtım veya bir kavram kanıtı yukarı bekleme isteyen yapay ZEKA geliştiricisi |
| [Tahmine dayalı bakım için eğitim kaynakları](#training-resources-for-predictive-maintenance) | Yukarıdaki, bir bölümünü veya tamamını ve temel kavramları veri bilimi, araçları ve teknikleri öğrenmek istersiniz.

### <a name="prerequisite-knowledge"></a>Önkoşul bilgileri
Önceki veri bilimi bilgisine sahip okuyucunun BDM içeriği beklemiyor. İstatistik ve veri bilimi temel bilgiye TDM içeriği için yararlıdır. Azure veri ve yapay ZEKA Hizmetleri, Python, R, XML ve JSON bilgi önerilir. Yapay ZEKA teknikleri, Python ve R paketleri uygulanır. Çözüm şablonları, Azure Hizmetleri, geliştirme araçları ve SDK'lar kullanarak uygulanır.

## <a name="business-case-for-predictive-maintenance"></a>Tahmine dayalı bakım için İş Gerekçesi

İşletmeler, en yüksek verimlilik ve kullanımı üzerinde sermaye yatırımlarınızı kendi dönüş hayata geçirmek için çalıştırılması için kritik donanımı gerektirir. Bu varlıklar, uçak motorları, turbines, elevators ya da milyonlarca - fotokopi, kahve makineler veya Su coolers gibi günlük Gereçleri aşağı maliyet endüstriyel chillers - değişiklik gösterebilir.
- Varsayılan olarak, çoğu işletme, parçaların değiştirildiği ve başarısız olduğu durumlarda _Düzeltme bakımını_kullanır. Düzeltme bakım sağlar bölümleri tamamen kullanılır (Bu nedenle değil İsraf bileşen yaşam), ancak maliyetlerini iş kapalı kalma süresi, işçilik ve zamanlanmamış bakım gereksinimlerini (kapalı, saat veya kullanışsız konumları).
- Bir sonraki düzeyde, işletmeler, bir bölüm için yararlı ömrü tespit ettikleri ve bir hatadan önce BT 'nin yerini alacak veya yerine geçecek şekilde _bakım_yapma alıştırması sağlar. Önleyici Bakım, zamanlanmamış ve yıkıcı hatalarını önler. Ancak, zamanlanan kapalı kalma süresi, bileşenin kullanım ömrü boyunca kullanılması ve işgücü hala devam etmektedir.
- Tahmine _dayalı bakımın_ hedefi, bileşenlerin _yalnızca yerinde_ değiştirilmesini etkinleştirerek, düzeltici ve önleyici bakım arasındaki dengeyi en uygun hale getirmektir. Yakın bir hata olduğunda bu yaklaşım yalnızca bu bileşenleri yerini alır. Bileşen lifespans (önleyici Bakımı karşılaştırıldığında) genişleterek ve zamanlanmamış Bakım ve işçi maliyetleri (düzeltme bakım) azaltma, işletmelerin maliyet tasarrufu ve rekabet avantajları elde edebilirsiniz.

## <a name="business-problems-in-pdm"></a>PdM iş sorunları
İşletmeler, beklenmeyen hatalar nedeniyle yüksek işletimsel risk yüz tanıma ve öngörü sorunların kök nedenini karmaşık sistemlerde sınırlı. İşle ilgili önemli sorulara bazıları şunlardır:

- Donanım ya da sistemin performansını veya işlev anomalileri algılayın.
- Bir varlık yakın gelecekte başlatılamayabilir olup olmadığını tahmin edin.
- Bir varlığın kalan faydalı ömrü tahmin edin.
- Bir varlığın hatası ana nedenlerini belirleyin.
- Bakım işlemleri, zaman göre bir varlık üzerinde yapılması gerektiğini belirleyin.

PdM tipik hedef deyimlerini şunlardır:

- Görev açısından kritik ekipman işletimsel riskini azaltır.
- Ortaya çıkmadan önce hatalarını tahmin varlıklar üzerinde oranını artırın.
- Bakım maliyeti, just-ın-time bakım işlemleri sağlayarak denetler.
- Müşteri attrition düşürün, marka resmi ve kayıp satış iyileştirin.
- Stok düzeylerini yeniden sıralama noktayı tahmin ederek çalışmasını azaltarak düşük stok maliyetlerini.
- Çeşitli bakım sorunlarına bağlı keşfedin.
- KPI'ları (ana performans göstergelerini) gibi sistem durumu puanları varlık koşullarını sağlayın.
- Kalan kullanım ömrü varlığını tahmin edin.
- Zamanında bakım etkinlikleri önerilir.
- Yalnızca zaman envanterinde değiştirme bölümleri için sipariş tarihlerini tahmin ederek etkinleştirin.

Bu hedef deyimleri için başlangıç noktaları şunlardır:

- _veri bilimcileri_ , belirli tahmine dayalı sorunları çözümleyip çözmelidir.
- _bulut mimarları ve geliştiricilerin_ uçtan uca çözümü bir araya koymasını sağlar.

## <a name="qualifying-problems-for-predictive-maintenance"></a>Tahmine dayalı bakım için sorunları niteleme
Tüm kullanım veya iş sorunlarını etkili bir şekilde PdM tarafından çözülebilir vurgulamak önemlidir. Sorun seçim sırasında düşünülmesi gereken üç önemli belirleme ölçütleri vardır:

- Sorun, doğası gereği Tahmine dayalı olmak zorundadır; diğer bir deyişle, olmalıdır bir hedef veya tahmin etmek için bir sonuç. Sorun da eyleminin algılandığında, bunların hatalarını önlemek için bir yol olmalıdır.
- Sorun, _hem iyi hem de hatalı sonuçları_içeren donanımların işletimsel geçmişinin bir kaydına sahip olmalıdır. Hatalı sonuçları azaltmak için gerçekleştirilen eylemleri kümesini de kullanılabilir olmalıdır bu kayıtları bir parçası olarak. Hata raporlarını, performans düşüşü bakım günlükleri onarın ve Değiştir günlükleri de önemlidir. Ayrıca, bunları ve değişiklik kayıtları geliştirmek için gerçekleştirilen onarım de yararlıdır.
- Kayıtlı geçmiş, kullanım durumunu desteklemeye _yetecek kadar kalite_ gerektiren _ilgili_ verilere yansıtılmalıdır. Veri ilgisi ve sufficiency hakkında daha fazla bilgi için bkz. tahmine [dayalı bakım Için veri gereksinimleri](#data-requirements-for-predictive-maintenance).
- Son olarak, iş sorunu açık bir anlayışa sahip etki alanı uzmanları olması gerekir. Bunlar, iç işlemler ve anlamak ve yorumlamak veri analisti yardımcı olacak yöntemler farkında olmalıdır. Bunlar ayrıca Yardım gerekirse sorunları için doğru verileri toplamak için mevcut iş süreçlerini için gerekli değişiklikleri yapabilir olmalıdır.

## <a name="sample-pdm-use-cases"></a>Örnek PdM kullanım örnekleri
Bu bölümde PdM Havacılık yardımcı programları ve taşıma gibi çeşitli sektörlerden kullanım örnekleri koleksiyonunu ele alınmaktadır. Her bölüm, bir iş sorununu ile başlar ve PdM, iş sorununu ve son olarak bir PdM çözüm avantajlarını çevreleyen ilgili verileri avantajları anlatılmaktadır.

| İş sorununu | PdM sağladığı avantajları |
|:-----------------|-------------------|
|**Meydan**      |                   |
|Mekanik sorunlar nedeniyle _Uçuş gecikmesi ve iptalleri_ . Sürede onarılamıyor hataları uçuşlar iptal edilmesine neden ve zamanlama ve işlemleri kesintiye uğratabilir. |PdM çözümleri mekanik hatalar nedeniyle iptal edildi veya Gecikmeli uçak olasılığını tahmin edebilirsiniz.|
|_Uçak motoru bölüm hatası_: airi motoru bölüm değişiklikleri hava yolu sektördeki en yaygın bakım görevlerinin arasındadır. Bakım çözümleri bileşen stok kullanılabilirliği, teslim ve planlama dikkatli yönetim gerektirir|Bileşen güvenilirlik ıntelligence'a yatırım maliyetlerini önemli ölçüde azalma için müşteri adayları toplamak işaretleyebilmesine.|
|**Vade** |                         |
|_ATM hatası_ , bankacılık sektörünün içindeki yaygın bir sorundur. Buradaki sorun, bir ATM nakit mevzuatı işlem nakit dağıtıcısı kağıt sıkıştı veya bölümü hata nedeniyle kesintiye uğrarsa olasılık rapor etmektir. İşlem hata tahminlere göre ATM proaktif olarak hatalarının oluşmasını önlemek için hizmet verebilir.| Makine sürecin yarısında bir işlem başarısız olmasına izin vermek yerine, istenen hizmetini reddetmek için makine üzerinde Tahmine dayalı programa alternatiftir.|
|**Enerji** |                          |
|_Rüzgar türbin hatalarıyla_: Rüzgar türbinler, çevre açısından sorumlu ülkelerde/bölgelerde ana enerji kaynağıdır ve yüksek sermaye maliyetlerini içerir. Rüzgar türbinler içindeki bir anahtar bileşen, hata türbin verimsiz bir şekilde işleyen Oluşturucu motorunındır. Ayrıca, düzeltmek son derece pahalı olur.|MTTF (ortalama süresi hatası) gibi KPI'leri tahmin etme, enerji şirketlerinin türbinin hatalarını önlemek ve çok az kesinti olun yardımcı olabilir. Hata olasılığını yakında başarısız olma olasılığı yüksek olan turbines izlemek için teknisyenleri bildirmek ve Bakım zaman tabanlı regimes zamanlayın. Tahmine dayalı modeller, sorunların kök nedenlerini teknisyenleri yardımcı olan hataya katkıda bulunan farklı faktörlerden Öngörüler daha iyi anlamak sağlar.|
|_Devre kesici sorunları: elektrik teslimatlarına_ve işletmelere enerji dağıtımı sağlamak için güç çizgilerinin her zaman çalışır durumda olmasını gerektirir. Devre kesicilerin yardımcı sınırlamak veya zarar gücüne önlemek satırları aşırı yüklemesi sırasında veya olumsuz koşullar hava durumu. İş sorununu Burada, devre kesici hataları tahmin etmektir.| PdM çözümleri onarım maliyetleri azaltıp devre Kesiciler gibi donanım ömrü yardımcı olur. Beklenmeyen hataları ve hizmet kesintilerine azaltarak güç ağ kalitesini artırmak yardımcı olurlar.|
|**Ulaşım ve Lojistik** |    |
|_Asansör kapısı sorunları_: büyük asansör şirketleri dünyanın dört bir yanındaki milyonlarca işlevsel yükseltme için tam yığın hizmeti sağlar. Asansör güvenlik, güvenilirlik ve çalışma süresi müşterileri için temel sorun var. Bu şirketler bu ve diğer çeşitli özniteliklerini düzeltme ve önleyici bakımla yardımcı olmak için algılayıcılar aracılığıyla izler. İçinde bir Asansör, en yaygın müşteri sorunu Asansör kapılar gerçekleştiriyor. Bu durumda iş sorununu kapı hatalarının olası neden tahmin eden bir Bilgi Bankası Tahmine dayalı uygulama sağlamaktır.| Elevators sermaye yatırımlarınızı için büyük olasılıkla bir 20-30 yıl kullanım ömrü ' dir. Bu nedenle her bir potansiyel satış son derece rekabetçi olabilir. Bu nedenle hizmeti ve Destek beklentileri yüksektir. Tahmine dayalı bakım, bu şirketler, rakiplerini, üründeki bir avantajı sağlar ve hizmet.|
|_Tekerlek arızaları_: tüm eğitim ve maliyet milyarlarının yarısını küresel RAIL sektörünün yarısı için tekerlek başarısızlığı hesabı. Tekerlek hataları de bazen erken ayırmak parmaklık neden bozulmasına rails neden olur. Parmaklık sonları derailments gibi yıkıcı olaylara kısalmasına neden olur. Bu tür örnekleri önlemek için railways tekerlekleri performansını izleyebilir ve bunları önleyici bir şekilde değiştirin. İş burada tekerleği hataları tahmin sorunudur.| Tekerlekleri, Tahmine dayalı bakım ile wheels, just-ın-time değiştirme yardımcı olur |
|_Subway kapı başarısızlıklarını eğitme_: alt yönlü işlemlerdeki gecikmelerin ana nedeni, arabaların eğileme arızalarına yöneliktir. Burada problemini train kapı arızaların tahmin edilmesine sağlamaktır.|Kapı hatası ya da bir kapı hatası kadar gün sayısını erken farkındalık bakım zamanlamaları kapı eğitim iş İyileştir yardımcı olur.|

Sonraki bölümde, yukarıda açıklanan PdM avantajlarından nasıl detayına alır.

## <a name="data-science-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri bilimi

Bu bölümde, PdM için veri bilimi ilkeleri ve uygulama genel yönergeleri sağlar. TDM, çözüm Mimarı yardımcı olmayı hedefler ya da bir geliştirici, önkoşulları ve PdM için uçtan uca yapay ZEKA uygulamaları oluşturmak için işleme anlama. Bu bölümü, tahmine [dayalı bakım Için çözüm şablonlarında](#solution-templates-for-predictive-maintenance)listelenen tanıtımlar ve kavram kanıtı şablonlarının incelenmesi ile birlikte okuyabilirsiniz. Ardından, Azure'da PdM çözümünüzü uygulamak için bu ilkeleri ve en iyi yöntemler kullanabilirsiniz.

> [!NOTE]
> Bu kılavuz, veri bilimi okuyucu öğretmek için tasarlanmamıştır. Tahmine [dayalı bakım için eğitim kaynakları](#training-resources-for-predictive-maintenance)bölümünde daha ayrıntılı bilgi için birkaç faydalı kaynak sunulmaktadır. Kılavuzda listelenen [Çözüm şablonları](#solution-templates-for-predictive-maintenance) , belirli PDM sorunları IÇIN Bu AI tekniklerinden bazılarını göstermektedir.

## <a name="data-requirements-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri gereksinimleri

Tüm öğrenme başarısını ne verilen kalite (a) ve (b) learner yeteneğini bağlıdır. Tahmine dayalı modelleri desenler geçmiş verilerden bilgi almak ve gözlemlenen bu eğilimlere bağlı olarak belirli bir olasılık ile gelecekteki sonuçları tahmin edin. Tahmine dayalı bir modelin doğruluğunu ilgi düzeyini, sufficiency ve eğitim ve test veri kalitesi bağlıdır. 'Bu model kullanılarak puanlanır' yeni veri, eğitim ve test verileri olarak aynı özellikleri ve şema olması gerekir. Yeni verileri özellik özellikleri (tür, yoğunluğu, dağıtım ve benzeri), eğitim ve test veri kümeleri eşleşmesi gerekir. Bu bölümde, bu tür veri gereksinimlerine biridir.

### <a name="relevant-data"></a>İlgili verileri

İlk olarak, verilerin _sorunla ilgili_olması gerekir. Yukarıda açıklanan _tekerlek hata_ kullanım durumunu göz önünde bulundurun. eğitim verileri, tekerlek işlemleriyle ilgili özellikler içermelidir. Sorun, _görmeyebilir sisteminin_başarısızlığını tahmin etmeye çalışıyorsa eğitim verilerinin, görmeyebilir sisteminin tüm farklı bileşenlerini kapsayacak olması gerekir. Daha büyük bir alt sistem hatasını ikinci koşul hedefleyen ise ilk harfi belirli bir bileşeni hedefler. İkinci daha veri dağınık olduğundan daha büyük alt sistemlerin yerine belirli bileşenleri hakkında öngörü sistemleri tasarlamak için genel kullanılması önerilir. Etki alanı uzmanı (tahmine [dayalı bakım Için uygun sorunları](#qualifying-problems-for-predictive-maintenance)gör) analizin için en ilgili veri alt kümelerini seçmeye yardımcı olmalıdır. İlgili veri kaynakları, tahmine [dayalı bakım Için veri hazırlama konusunda](#data-preparation-for-predictive-maintenance)daha ayrıntılı olarak ele alınmıştır.

### <a name="sufficient-data"></a>Yeterli veri
İki soruyla hatası geçmiş verileri ile ilgili sık sorulan: (1) "kaç hatası olaylarının bir model eğitip gerekli midir?" (2) "kaç kayıt" yeterince "kabul edilir?" Kesin yanıt yoktur, ancak yalnızca Thumb kuralları. (1) için daha hatası olaylarının sayısını daha iyi bir model. (2) ve veri ve sorun Çözüldü bağlamı hatası olaylarının sayısına bağlıdır. Ancak, bir makine çok sık başarısız olursa diğer taraftan, ardından işletme, hangi hata örnekleri azaltacak yerini alır. Burada yine etki alanında Uzman rehberlik önemlidir. Ancak, _nadir olay_sorunuyla ilgili olarak bu yöntemde olan yöntemler vardır. Bunlar, [imledengelenmiş verileri işleme](#handling-imbalanced-data)bölümünde ele alınmıştır.

### <a name="quality-data"></a>Kalite verileri
Verilerin kalitesi kritiktir-her bir tahmin özniteliği değeri, hedef değişkenin değeriyle birlikte _doğru_ olmalıdır. Veri Kalitesi istatistik ve veri yönetimi iyi studied alanındadır ve out dolayısıyla, bu kılavuzun kapsamı.

> [!NOTE]
> Çeşitli kaynaklar ve kalite verileri sunmak için Kurumsal ürünleri vardır. Başvurular bir örneği aşağıda verilmiştir:
> - Dasu, T, Johnson T., Keşif veri madenciliği ve verileri, 2003 Wiley, temizleme.
> - [Araştırmacı veri analizi, Vikipedi](https://en.wikipedia.org/wiki/Exploratory_data_analysis)
> - [Büyük veritabanları için Hellerstein, J, nicel veri temizleme](http://db.cs.berkeley.edu/jmh/papers/cleaning-unece.pdf)
> - [de Jonge, E, Van der ara, M, R ile veri temizlemeye giriş](https://cran.r-project.org/doc/contrib/de_Jonge+van_der_Loo-Introduction_to_data_cleaning_with_R.pdf)

## <a name="data-preparation-for-predictive-maintenance"></a>Tahmine dayalı bakım için veri hazırlama

### <a name="data-sources"></a>Veri kaynakları

Tahmine dayalı bakım için ilgili veri kaynaklarına içerir ancak bunlarla sınırlı değildir:
- Hata geçmişi
- Bakım/onarma geçmişi
- Makine çalışma koşullarını
- Donanım meta verileri

#### <a name="failure-history"></a>Hata geçmişi
Hata olayları PdM uygulamalarda seyrek kullanılır. Ancak, tahmin modellerini oluştururken, işletimsel desen normal bir bileşenin yanı sıra hakkında kendi hata desenleri öğrenmek algoritma gerekir. Bu nedenle eğitim verileri örnekler hem kategorilerden yeterli sayıda içermelidir. Bakım kayıt ve bölümleri değişiklik geçmişi hatası olaylarını bulmak için uygun kaynaklardır. Bazı etki alanı bilgisini yardımıyla eğitim verilerdeki anomaliler hatalar olarak tanımlanabilir.

#### <a name="maintenancerepair-history"></a>Bakım/onarma geçmişi
Bir varlığın bakım geçmişi, değişen bileşenlerle ilgili ayrıntıları ve gerçekleştirilen onarım etkinliklerini içerir. Bu olaylar, düşme düzenlerini kaydeder. Eğitim verileri bu çok önemli bilgileri olmaması yanıltıcı modeli sonuçlara yol açabilir. Özel hata kodlarını veya sipariş tarihlerini bölümleri için başarısızlık geçmişi de bakım geçmişi içinde bulunabilir. Hata desenleri etkileyen ek veri kaynaklarını araştırılması ve etki alanı uzmanları tarafından sağlanan.

#### <a name="machine-operating-conditions"></a>Makine çalışma koşullarını
Algılayıcı tabanlı (veya diğer) akış verilerini ekipmanın işleminde, bir önemli veri kaynağıdır. Bir anahtar PdM içinde bir makinenin sistem durumu, rutin işlemi sırasında zamanla düşürür varsayılır. Veriler, bu eskime düzeni ve anomalileri düşmesine yol açar ve zaman açısından değişkenlik gösteren Özellikler içermesi beklenir. Zamana bağlı veriler yönüyle algoritması hata ve hata olmayan desenleri zamanla öğrenmek için gereklidir. Bu veri noktalarında bağlı olarak, süresinin kaç tane daha fazla birimi bir makine başarısız önce çalışmaya devam edebilirsiniz tahmin etmek algoritma öğrenir.

#### <a name="static-feature-data"></a>Statik özellik verileri
Statik özellikler ekipman hakkındaki meta verileri alır. Örnekler donanım oluşturma, modeli, üretilen tarihi, tarih hizmetinin, sistem ve diğer teknik belirtimler konumunu başlatın.

[Örnek PDM kullanım örnekleri](#sample-pdm-use-cases) için ilgili verilerin örnekleri aşağıda verilmiştir:

| Kullanım Örneği | İlgili veri örnekleri |
|:---------|---------------------------|
|_Uçuş gecikmesi ve iptalleri_ | Rota bilgilerini uçuş Bacak biçiminde uçuş ve günlükleri sayfasında. Uçuş BAI verileri, ayrılma/varış tarihi, zaman, Havaalanı, layovers vb. yönlendirme ayrıntılarını içerir. Sayfa günlüğü, bir dizi hata ve bir hata ve bakım kodu içerir.|
|_Airi motoru bölüm hatası_ | Çeşitli parçalarının koşula bilgilerini uçak, sensörlerden alınan veri toplanmadı. Bakım kayıt bileşeni hataları oluştu ve bunların yerini belirlemenize yardımcı olur.|
|_ATM hatası_ | Sensör okumaları (nakit/onay ürünü) her bir işlem için ve nakit dağıtmak. Notlar, Not kalınlığı, Not varış uzaklığı, denetim öznitelikleri vb. arasında boşluk ölçümü hakkında bilgi. Hata kodları sağlayan bakım kayıtları, onarım bilgileri, nakit dağıtıcısının en son yeniden doldurulduğu zaman.|
|_Rüzgar türbin hatası_ | Algılayıcılar, sıcaklık, Rüzgar yönü, güç oluşturma, Oluşturucu hızı vb. gibi türbin koşullarını izler. Veriler, çeşitli bölgelerde bulunan Rüzgar gruplarından birden çok rüzgar türbinler tarafından toplanır. Genellikle, her türbinin ölçümleri bir sabit bir zaman aralığında geçiş birden çok sensör okumaları sahip olacaktır.|
|_Devre kesici sorunları_ | Bakım günlükleri düzeltme, önleyici ve sistematik Eylemler içerir. Gibi devre kesicileri açma ve kapama eylemler için gönderilen otomatik ve el ile komutları içeren işletimsel verileri. Üretim tarihi, konum, model vb. gibi cihaz meta verileri Voltaj düzeyleri, coğrafi konum, çevresel koşullar gibi devre kesici belirtimleri.|
|_Asansör kapısı başarısızlığı_| Asansör meta veri türü Asansör, üretilen tarih, bakım sıklığı, yapı türü ve benzeri gibi. Kapı döngüleri, ortalama kapağı kapatma zaman sayısı gibi işletimsel bilgiler. Hata geçmişi ile neden olur.|
|_Tekerlek başarısızlığı_ | Tekerlek hızlandırma, örgü örnekleri, gidiş uzaklığı, hız vb. ölçülü algılayıcı verileri. Üretici ve mamul tarihi gibi tekerlekler hakkında statik bilgiler. Sipariş tarihlerini ve miktarlar izleyen parça sipariş veritabanından olayla hatası verileri.|
|_Subway kapı başarısızlıklarını eğitme_ | Kapı açılış ve kapanış kez train kapı geçerli durumu gibi diğer işletimsel verileri. Statik veri varlığı tanımlayıcısı, saati ve koşul değeri sütunları içerir.|

### <a name="data-types"></a>Veri türleri
Yukarıdaki veri kaynaklarına göz önünde bulundurulduğunda, gözlemlenen PdM etki alanında iki ana veri türleri şunlardır:

- Zamana bağlı _veriler_: işlem telemetrisi, makine koşulları, iş sırası türleri, kayıt sırasında zaman damgalarına sahip olacak öncelik kodları. Hata, bakım/onarma ve Kullanım Geçmişi her olayla ilişkili zaman damgaları da gerekir.
- _Statik veriler_: makine özellikleri ve operatör özellikleri, makinelerin veya işleç özniteliklerinin teknik belirtimlerini tanımladıklarından, statiktir. Bu özellikler, zaman içinde değişebilir, ayrıca bunlarla ilişkili zaman damgaları sahip olmalıdır.

Tahmine ici ve hedef değişkenler, kullanılmakta olan algoritmaya bağlı olarak [, sayısal, kategorik ve diğer veri türlerine](https://www.statsdirect.com/help/basics/measurement_scales.htm) önceden işlenmeli/dönüştürülebilmelidir.

### <a name="data-preprocessing"></a>Veri ön işleme
_Özellik mühendisliğinin_bir önkoşulu olarak, çeşitli akışlardan verileri, kolayca yapı özelliklerinin oluşturulduğu bir şema oluşturmak için hazırlayın. Verilerin ilk kayıtların bulunduğu bir tablo görselleştirin. Tablodaki her satır bir eğitim örneğini temsil eder ve sütunlar tahmine _ici_ özelliklerini (bağımsız öznitelikler veya değişkenler olarak da adlandırılır) temsil eder. Son sütun (ler) _hedefin hedefi_ (bağımlı değişken) olduğu gibi verileri düzenleyin. Her eğitim örneği için, bu sütunun değeri olarak bir _etiket_ atayın.

Zamana bağlı veriler için sensör verilerinin süresi zaman birimler halinde böler. Her kayıt bir varlık için bir zaman birimine ait olmalıdır _ve ayrı bilgiler sunmalıdır_. Zaman birimi katları saniye, dakika, saat, gün, iş gereksinimlerinize göre tanımlanan ay, ve benzeri. Zaman birimi, _veri toplama sıklığıyla aynı olmak zorunda değildir_. Sıklığı yüksekse, herhangi bir birim önemli fark diğer veriler gösterilmeyebilir. Örneğin, ortam sıcaklığı 10 saniyede toplanan varsayalım. Eğitim verileri, aynı aralık kullanarak yalnızca örnek sayısını herhangi bir ek bilgi sağlamadan Şişir. Bu durumda, daha iyi bir stratejisi üzerinde İş Gerekçesi verilere tekrar 10 dakika ya da bir saat göre ortalama kullanmak olabilir.

Statik veriler için
- _Bakım kayıtları_: ham bakım verileri, belirli bir noktada gerçekleştirilmiş bakım etkinlikleri hakkında bilgi içeren bir varlık tanımlayıcısı ve zaman damgası içerir. Her kategori tanımlayıcısının belirli bir bakım eylemiyle benzersiz bir şekilde eşlendiği, bakım etkinliklerini _kategorik_ sütunlara dönüştürün. Bakım kayıt için şema varlık tanımlayıcısı, saati ve Bakım eylemi içerir.

- _Hata kayıtları_: belirli iş koşulları tarafından tanımlanan özel hata kodları veya hata olayları olarak hatalar veya hata nedenleri kaydedilebilir. Donanım birden çok hata kodları sahip olduğu durumlarda, etki alanında Uzman hedef değişkene ilgili olanları belirlemenize yardımcı olmalıdır. Bu hatalarla bağıntılı tahmine _ici_ özellikleri oluşturmak için kalan hata kodlarını veya koşullarını kullanın. Hata kayıt için şema varsa varlık tanımlayıcısı, saat, başarısız veya hata nedeni - verilebilir.

- _Makine ve işleç meta verileri_: bir varlığı, kendi öznitelikleriyle birlikte ilişkilendirmek için makine ve işleç verilerini tek bir şemada birleştirin. Makine koşulları için şema varlık tanımlayıcısı, varlık özellikleri, işleci tanımlayıcısı ve işleç özellikleri içerir.

Diğer veri ön işleme adımları _eksik değerleri işlemeyi_ ve öznitelik değerlerinin _normalleştirmesini_ içerir. Ayrıntılı bir tartışma bu kılavuzun kapsamı dışındadır - bazı yararlı başvuruları için sonraki bölüme bakın.

Yukarıdaki ile yerinde, özellik Mühendisliği varlık tanımlayıcısı ve zaman damgası göre yukarıdaki tabloların katılmak için önce son dönüşümü veri kaynakları önceden işlenmiş. Makine normal işlem olduğunda ortaya çıkan tabloda hata sütunu için null değerler gerekir. Bu null değerler, normal işlem için bir gösterge olarak imputed. Tahmine _dayalı model için Etiketler_oluşturmak üzere bu hata sütununu kullanın. Daha fazla bilgi için, tahmine dayalı [bakım için modelleme teknikleri](#modeling-techniques-for-predictive-maintenance)bölümüne bakın.

## <a name="feature-engineering"></a>Özellik mühendisliği
Özellik Mühendisliği, veri modelleme önce ilk adımdır. Veri bilimi işlemindeki rolü [burada açıklanmıştır](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/create-features). Bir _özellik_ , model için sıcaklık, basınç, titreşim vb. gibi tahmine dayalı bir özniteliktir. PdM için özellik Mühendisliği, hacimle süre içinde toplanan geçmiş veriler üzerinde bir makinenin sistem durumu özetleyen içerir. Bu anlamda den eşlerine Uzaktan izleme, anomali algılama ve hata algılama gibi farklı. 

### <a name="time-windows"></a>Zaman pencereleri
Uzaktan izleme, _zaman içinde noktadan_itibaren gerçekleşen olayları raporlamasına sahiptir. Anomali algılama modelleri (puan) gelen akışları veri noktaları itibarıyla bayrağı anomalileri zaman değerlendirin. Hata algılama belirli türlerdeki sürede noktaları ortaya çıktıkları olmasını hataları sınıflandırır. Buna karşılık PdM, _Geçmiş zaman dilimi_boyunca makine davranışını temsil eden özelliklere bağlı olarak _gelecekteki bir zaman dilimi_içinde oluşan tahmini sorunları içerir. PdM için özellik zaman tek tek noktalarından Tahmine dayalı olarak çok gürültülü verilerdir. Bu nedenle, her bir özelliğin verilerinin, zaman pencereleri üzerinde veri noktaları toplayarak _düzgün_ bir şekilde ele alınması gerekir.

### <a name="lag-features"></a>Lag özellikleri
Ne kadar geleceğe tahmin modeli olan iş gereksinimlerini tanımlayın. Sırayla bu süre, 'ne kadar model aramak yanınızdadır' tanımlamak yardımcı olur bu tahminler gerçekleştirin. Bu ' geri arama ' dönemine _gecikme_denir ve bu gecikme süresi boyunca uygulanan özellikler _gecikme özellikleri_olarak adlandırılır. Bu bölümde, veri kaynaklarından zaman damgaları ve statik veri kaynaklarından oluşturma özelliği ile oluşturulmuş lag özellikleri açıklanmaktadır. Gecikme özellikleri genellikle doğal olarak _sayısal_ olarak ayarlanır.

> [!IMPORTANT]
> Pencere boyutu deneme ile belirlenir ve bir etki alanında Uzman yardımı ile sonlandırılır. Seçim ve gecikme özelliklerini, bunların toplamalar ve windows türü tanımı için aynı uyarı tutar.

#### <a name="rolling-aggregates"></a>Sıralı toplamaları
Her bir varlık kaydı için sıralı bir pencere boyutu "W" toplamları hesaplamak için zaman birimlerinin sayısı olarak seçilir. Sonra gecikme özellikleri, bu kaydın _tarihinden önce_ W dönemleri kullanılarak hesaplanır. Şekil 1'de mavi satırlar her zaman birimi için bir varlık için kayıtlı algılayıcı değerlerini gösterir. Bunlar, W = 3 boyutunun bir pencere üzerinde çalışırken bir özellik değerlerinin ortalamasını gösterir. Hareketli ortalama, t<sub>1</sub> (turuncu) ile t<sub>2</sub> (yeşil) aralığındaki zaman damgalarına sahip tüm kayıtlar üzerinden hesaplanır. W genellikle dakika veya saat verilerin doğasına bağlı olarak değerdir. Ancak belirli bir varlığın tüm geçmiş kaydının kadar zaman sorunları, büyük bir W (örneğin, 12 aylık) çekme sağlayabilir.

![Şekil 1. Toplama özellikleri alınıyor](./media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

Şekil 1. Toplama özellikleri alınıyor

Toplamlar bir zaman penceresi üzerinde çalışırken, örnek sayısı, ortalama, CUMESUM (birikmeli toplamı) ölçüler, en düşük/en yüksek değerleri verilebilir. Ayrıca, farkı, standart sapma ve aykırı değerleri ötesinde N standart sapma sayısı sık sık kullanılır. Bu kılavuzdaki [kullanım örnekleri](#sample-pdm-use-cases) için uygulanabilecek toplamaların örnekleri aşağıda listelenmiştir. 
- _Uçuş gecikmesi_: son gün/hafta içindeki hata kodlarının sayısı.
- _Uçak motoru bölüm hatası_: geçen gün, standart sapma ve toplam gün, hafta vb. dahil olmak üzere toplama anlamına gelir. Bu ölçüm, iş etki alanı uzmanı ile birlikte belirtilmelidir.
- _ATM arızaları_: toplama, ortanca, Aralık, standart sapmalar, üç standart sapmanın ötesinde, üst ve alt CUMESUM 'un sayısını belirtir.
- _Alt yol tren kapı arızaları_: geçen gün, hafta, iki hafta içindeki olay sayısı.
- _Devre kesici hataları_: geçen hafta, yıl, üç yıl ve bu yana hata sayısı.

Başka bir kullanışlı PdM, eğilim değişiklikleri, ani ve verileri anormallikleri algoritmalarını kullanarak düzeyi değişiklikleri yakalamak için bir tekniktir.

#### <a name="tumbling-aggregates"></a>Atlayan toplamaları
Bir varlığın etiketlendiği her kayıt için, _w-<sub>k</sub>_  boyutundaki bir pencere tanımlanmıştır; burada _k_ , _w_boyutundaki pencerelerin sayısıdır. Daha sonra toplamalar, bir kaydın zaman damgasından önceki dönemler için _Windows_ _W-k, w-<sub>(k-1)</sub>,..., w-<sub>2</sub>, w-<sub>1</sub>_  ' n i _n üzerine yazılır_ . _k_ kısa vadeli etkileri yakalamak için küçük bir sayı ya da uzun süreli azalma düzenlerini yakalamak için büyük bir sayı olabilir. (bkz: Şekil 2).

![Şekil 2. Toplama özellikleri atlayan](./media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

Şekil 2. Toplama özellikleri atlayan

Rüzgar turbines kullanım örneği oluşturulabilir, H = 1 k ile özellikleri, öteleme = 3. Bunlar, her üst ve alt aykırı değerleri kullanarak son üç ay için gecikme aktarıldığını belirtir.

### <a name="static-features"></a>Statik özellikler

Tarih gibi donanım üretim, model numarası, konum, teknik belirtimler statik özellikler bazı örnekleri verilmiştir. Modelleme için _kategorik_ değişkenler olarak kabul edilir. Voltaj, geçerli, güç kapasitesi, dönüştürücü türü ve güç kaynağı, devre kesici kullanım örneği bazı örnek olarak verilebilir. Tekerlek hataları için örnek Lastik tekerlekleri (karışımı vs çelik) türüdür.

Şu ana kadar bahsedilen veri hazırlama çabalarını aşağıda gösterildiği gibi düzenlenmiş veri neden. Eğitim, sınama ve doğrulama veri (Bu örnekte zaman gün birimleri cinsinden gösterilmiştir) Bu mantıksal şemaya sahip olmalıdır.

| Varlık Kimliği | Zaman | \<Özellik sütunları > | Etiketle |
| ---- | ---- | --- | --- |
| A123 |1\. gün | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |
| A123 |2\. gün | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |
| ...  |...   | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |
| B234 |1\. gün | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |
| B234 |2\. gün | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |
| ...  |...   | arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. arasında yetersiz alanla karşılaştı. | arasında yetersiz alanla karşılaştı. |

Özellik mühendisliğinin son adımı hedef değişkenin **etiketleniyor** . Bu işlem, modelleme tekniği bağlıdır. Buna karşılık, modelleme teknik problemini ve kullanılabilir verilerin doğasına bağlıdır. Etiketleme, sonraki bölümde ele alınmıştır.

> [!IMPORTANT]
> Veri hazırlama ve özellik Mühendisliği başarılı PdM çözümleri gelmesi teknikleri modelleme olarak kadar önemlidir. Etki alanı uzmanı ve pratik doğru özellikleri ve veri modeli için gelen içinde önemli zaman ayırmanız. Özellik Mühendisliği birçok Kitaplar'dan küçük bir örnek aşağıda listelenmiştir:
> - Pyle, d veri hazırlığı için veri (Morgan Kaufmann dizide veri yönetim sistemleri), 1999 araştırma
> - Zheng, A., Casari Machine Learning için a özellik Mühendisliği: ilkeler ve veri uzmanları, O'Reilly, 2018'e yönelik teknikler.
> - Dong, G. Liu, H. (düzenleyiciler), Machine Learning ve veri analizi için özellik Mühendisliği (Bölümman & salonu/CRC veri madenciliği ve bilgi bulma serisi), CRC basma, 2018.

## <a name="modeling-techniques-for-predictive-maintenance"></a>Tahmine dayalı bakım için modelleme teknikleri

Bu bölüm, belirli bir etiket oluşturma yöntemleriyle birlikte PdM sorunlar için ana modelleme teknikleri açıklar. Farklı endüstriler arasında bir tek modelleme teknik kullanılabilir dikkat edin. Modelleme teknikleri, veri eldeki bağlamında yerine veri bilimi sorununu eşleştirilir.

> [!IMPORTANT]
> Hata koşulları ve etiketleme stratejisi için etiketleri seçimi  
> etki alanı Uzman danışmanlığı içinde belirlenmesi.

### <a name="binary-classification"></a>İkili sınıflandırma
İkili sınıflandırma, gelecekteki _bir zaman diliminde bir ekipman parçasının başarısız olma olasılığını tahmin_ etmek için kullanılır ve _gelecek ufuk dönemi X_olarak adlandırılır. X, iş sorunu ve taraftaki veriler tarafından etki alanı uzmanlığına göre belirlenir. Örnekler şunlardır:
- bileşenleri değiştirmek, bakım kaynaklarını dağıtmak, bu dönemde oluşması muhtemel bir sorundan kaçınmak için bakım gerçekleştirmek için gereken _En düşük sağlama süresi_ .
- bir sorun gerçekleşmeden önce gerçekleşebilmeniz gereken _en az olay sayısı_ .

Bu teknik eğitim örnekleri iki tür tanımlanır. _Bir hatayı belirten_, etiket = 1 olan pozitif bir örnek. Normal işlemler gösterir, negatif bir örnek, etiketi = 0. Hedef değişkeni ve bu nedenle etiket değerleri _kategorik_' tir. Modeli, büyük olasılıkla başarısız veya zaman birimi X önümüzdeki normal olarak çalışmak her yeni örneğin tanımlamanız gerekir.

#### <a name="label-construction-for-binary-classification"></a>İkili sınıflandırma için etiket oluşturma
Burada soru şudur: "varlık sonraki başarısız olduğunu belirten olasılığı nedir zaman birimi X?" Bu soru, bir varlığın "arıza" olarak arıza öncesinde etiketini X kayıtları yanıtlamak için (Etiket = 1), "normal" olarak tüm kayıtları etiketlemelerine (etiket = 0). (bkz: Şekil 3).

![Şekil 3. İkili sınıflandırma etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

Şekil 3. İkili sınıflandırma etiketleme

Bazı kullanım örnekleri için stratejisi etiketleme örnekleri aşağıda listelenmiştir.
- _Uçuş gecikmeleri_: X bir gün olarak seçilebilir ve bu da sonraki 24 saat içindeki gecikmeleri tahmin edebilir. Ardından hataları önce 24 saat içinde olan tüm uçuşlar 1 etiketlenmiştir.
- _ATM nakit örneği hataları_: bir amaç, bir işlemin sonraki bir saatteki hata olasılığını tespit etmek olabilir. Bu durumda, hatanın son bir saat içinde gerçekleşen tüm işlemleri 1 etiketlenmiştir. Sonraki N para birimi hata olasılığını tahmin dispensed, notları hata son N notlarına dispensed tüm notları 1 etiketlenmiştir.
- _Devre kesici hataları_: hedef, sonraki devre kesici komut hatasını tahmin etmek olabilir. Bu durumda, bir sonraki komuttan olmasını X seçilir.
- _Kapı başarısızlıklarını eğitme_: X iki gün olarak seçilebilir.
- _Rüzgar türbin hatalarıyla_: X iki ay olarak seçilebilir.

### <a name="regression-for-predictive-maintenance"></a>Tahmine dayalı bakım için regresyon
Regresyon modelleri, _bir varlığın kalan faydalı ömrünü (RUL) hesaplamak_için kullanılır. RUL sonraki hata gerçekleşmeden önce bir varlık çalışır durumda süre miktarı tanımlanır. Her eğitim örneği, bir varlık için _NY_ bir zaman birimine ait olan bir kayıttır; burada _n_ birden çok. Model her yeni örneğin RUL 'sini _sürekli sayı_olarak hesaplamalıdır. Bu hatadan önce kalan süreyi gösterir.

#### <a name="label-construction-for-regression"></a>Regresyon için etiket oluşturma
Burada soru şudur: "Ekipmanın kalan faydalı ömrü (RUL) nedir?" Arıza öncesinde her bir kayıt için sonraki hatasından önce kalan zaman birimlerinin sayısı için etiket hesaplayın. Bu yöntemde, etiketleri sürekli değişkenlerdir. (Bkz: Şekil 4)

![Şekil 4. Regresyon için etiketleme](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

Şekil 4. Regresyon için etiketleme

Regresyon için etiketleme başvuru içeren bir hata noktası gerçekleştirilir. Ne kadar varlık hatasından önce da sıçramıştır bilmeden özelliği hesaplamasına mümkün değildir. Bu nedenle buna ikili sınıflandırma için varlıklar veri herhangi bir hata olmadan modelleme için kullanılamaz. Bu sorun en iyi yöntem [Analizi](https://en.wikipedia.org/wiki/Survival_analysis)adlı başka bir istatistiksel teknik tarafından karşılanır. Ancak, zaman açısından değişkenlik gösteren verilerin sık aralıklarla ile ilgili PdM kullanım örnekleri için bu tekniği uygularken olası zorluklar ortaya çıkabilir. Acil durum analizi hakkında daha fazla bilgi için [Bu tek sayfalayıcı](https://www.cscu.cornell.edu/news/news.php/stnews78.pdf)bölümüne bakın.

### <a name="multi-class-classification-for-predictive-maintenance"></a>Tahmine dayalı bakım için çok sınıflı sınıflandırma
Çok sınıflı sınıflandırma teknikleri PdM çözümlerinde iki senaryo için kullanılabilir:
- _Sonraki iki sonucu_tahmin edin: ilk sonuç bir varlık için _başarısızlık zaman aralığıdır_ . Varlık, birden çok olası süreler birine atanır. İkinci sonuç, _birden çok ana nedenden biri_nedeniyle gelecekteki bir dönemde hata olma olasılığıdır. Bu tahmin belirtileri ve planı bakım zamanlamaları için izlemek bakım ekibi sağlar.
- Belirli bir hatanın _en olası temel nedenini_ tahmin edin. Bu sonucu doğru bir hatasını düzeltmek için bakım eylemleri kümesini önerir. Temel nedenler ve önerilen onarım kişilerinin sıralı bir listesi, bir hatadan sonra onarım eylemlerini öncelik teknisyenleri yardımcı olabilir.

#### <a name="label-construction-for-multi-class-classification"></a>Çok sınıflı sınıflandırma için etiket oluşturma
Buradaki soru şudur: "bir varlığın, bir sonraki _nZ_ biriminde başarısız olma olasılığı, _n_ dönem sayısıdır." Bu soruyu cevaplamak için demet süre (3Z 2Z, Z) kullanarak bir varlığın arıza öncesinde nZ kayıtları etiketleyin. Etiket diğer tüm kayıtları "normal" (etiket = 0). Bu yöntemde, hedef değişkeni _kategorik_ değerleri barındırır. (Bkz. Şekil 5).

![Şekil 5. Birden çok Lass sınıflandırması için hata zaman tahmini etiketleri](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

Şekil 5. Hata zaman tahmini için çok sınıflı sınıflandırma etiketleme

Buradaki soru şudur: "kök nedeni/sorunu _P<sub>i</sub>_ nedeniyle varlığın sonraki X biriminde başarısız olmasının olasılığı nedir?" olası ana _nedenlerin sayısıdır._ Bu soruyu yanıtlamak için, "bir varlık hatasından önce X kaydı" kök neden _p<sub>ı</sub>_ nedeniyle başarısız olmak üzere "(Label = _P<sub>ı</sub>_ ) olarak etiketleyin. "Normal" olarak tüm kayıtları etiket (etiket = 0). Bu yöntemde, etiketleri kategorik (bkz. Şekil 6) ayrıca.

![Şekil 6. Birden çok Lass sınıflandırması için kök neden tahmin etiketleri](./media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

Şekil 6. Kök nedeni tahmin için çok sınıflı sınıflandırma etiketleme

Model her _P<sub>i</sub>_  nedeniyle hata olasılığı ve hata olmaması olasılığını atar. Bu olasılıklar gelecekte ortaya en olası sorunları tahmin izin vermek için büyüklük sıralanabilir.

Burada soru şudur: "Bakım eylemleri bir hatadan sonra önerilir?" Bu soruyu yanıtlamak için etiketleme _gelecekteki bir ufuk gerektirmez_, çünkü model gelecekte hata tahmin etmez. _Hatanın zaten gerçekleşmesinin_ardından en olası temel nedeni tahmin etmek yeterlidir.

## <a name="training-validation-and-testing-methods-for-predictive-maintenance"></a>Eğitim, doğrulama ve Tahmine dayalı bakım için test yöntemleri
[Ekip veri bilimi işlemi](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) , model eğitimi-test-doğrulama döngüsünün tam kapsamını sağlar. Bu bölümde, PdM için benzersiz yönleri açıklanmaktadır.

### <a name="cross-validation"></a>Çapraz doğrulama
[Çapraz doğrulamanın](https://en.wikipedia.org/wiki/Cross-validation_(statistics)) amacı, eğitim aşamasında modeli "test" olarak belirleyen bir veri kümesi tanımlamaktır. Bu veri kümesine _doğrulama kümesi_denir. Bu teknik, _fazla ekleme_ gibi sorunları sınırlamaya yardımcı olur ve modelin bağımsız bir veri kümesini genelleştirmek için bir öngörü sağlar. Diğer bir deyişle, gerçek bir sorunu olabilir bir bilinmeyen bir veri kümesi. Eğitim ve test rutin PdM için daha iyi görünmeyen gelecekteki veri genelleştirmek için zaman çeşitli yönlerini dikkate almanız gerekir.

Birçok makine öğrenimi algoritmaları model performansını önemli ölçüde değiştirebilirsiniz hiperparametreleri sayısına bağlıdır. Bu hiperparametreleri en iyi değerlerini otomatik olarak modeli eğitimindeki hesaplanan değil. Veri bilimi insanı belirtilmelidir. Hiperparametreleri iyi değerlerini bulma birkaç yolu vardır.

En yaygın bir, örnekleri _k_ _katlara_ rastgele ayıran, Her hiper parametre değeri kümesi için, öğrenme algoritmasını _k_ kez çalıştırın. Her yinelemede, örneklerin geçerli kat doğrulama kümesi ve örneklerin geri kalanında bir eğitim kümesi olarak kullanın. Eğitim örnekler algoritmasını eğitmek ve performans ölçümlerini doğrulama örnekler işlem. Bu döngünün sonunda, _k_ performans ölçümlerinin ortalamasını hesaplayın. Her hiper parametre değerleri kümesi için en iyi ortalama performans sahip olanları seçin. Hiperparametreleri seçme genellikle doğası gereği Deneysel bir görevdir.

PdM sorunları, verileri çeşitli veri kaynaklarından gelen olayları zaman serisi olarak kaydedilir. Bu kayıtlar etiketleme zaman göre sıralı olabilir. Bu nedenle, veri kümesi _rastgele_ eğitim ve doğrulama kümesine bölündüğünde bazı _eğitim örnekleri bazı doğrulama örneklerinden daha sonra zaman içinde olabilir_. Hiper parametre değerlerinin gelecekteki performansı, modelin eğitilme _işleminden önce_ ulaşan bazı veriler temel alınarak tahmin edilir. Bu tahminler zaman serisi özellikle sabit değildir ve zaman içinde gelişse aşırı iyimser olabilir. Sonuç olarak, seçilen hiper parametre değerleri yetersiz olabilir.

Önerilen yöntem, tüm doğrulama örneklerinin daha sonra tüm eğitim örneklerinden daha sonra olduğu _zamana bağlı_ bir şekilde, örnekleri eğitim ve doğrulama kümesine bölmek için önerilir. Her hiper parametre değerleri kümesi için algoritma eğitim veri kümesi üzerinde eğitin. Modelin performans aynı doğrulama küme üzerinde ölçün. En iyi performans gösteren hiper parametre değerlerini seçin. Rastgele çapraz doğrulama tarafından seçmiş değerlerle daha iyi gelecekteki model performansını train/doğrulama bölünmüş sonucu olarak seçilen hiper parametre değerleri.

Son modelin tüm eğitim veriler üzerinde en iyi hiper parametre değerlerini kullanarak bir öğrenme algoritması eğitim tarafından oluşturulabilir.

### <a name="testing-for-model-performance"></a>Model performansını test etme
Yeni veri çubuğunda gelecekteki performansını tahmini bir model oluşturulduktan sonra gereklidir. İyi bir performans ölçümü, Hiper parametre değerlerini doğrulama küme üzerinde hesaplanan veya çapraz doğrulama bir ortalama performans ölçümü hesaplanan tahmin edin. Bu tahminler, çoğunlukla aşırı iyimser olur. İş, genellikle bunlar modeli test yönteminizi bazı ek yönergeler sahip olabilir.

PdM için önerilen yol, örnekleri eğitimlere, doğrulamaya ve test veri kümelerine _zamana bağlı_ bir şekilde bölemedir. Tüm test örnekleri tüm eğitim ve doğrulama örnekler sürede daha sonra olmalıdır. Bölünmüş sonra daha önce açıklandığı gibi performansını ölçmek ve modeli oluşturur.

Zaman serisi sabit ve kolayca tahmin edilebilir olduğunda, rastgele ve zamana bağımlı yaklaşımları gelecekteki performans benzer tahminler oluşturur. Ancak zaman serisi ileti örneği olmayan ve/veya tahmin etmek zor olduğunda, zamana bağlı yaklaşım gelecekteki performansını daha gerçekçi tahminler oluşturur.

### <a name="time-dependent-split"></a>Zamana bağlı ayırma
Bu bölümde, zamana bağlı ayırma uygulamak için en iyi uygulamalar açıklanmaktadır. Eğitim ve test kümelerine arasında iki yönlü zamana bağımlı bölme aşağıda açıklanmıştır.

Bir akış gibi çeşitli sensörlerden alınan ölçümleri zaman damgalı olay varsayılır. Birden çok olay içeren zaman çerçeveleri özellikleri ve eğitim ve test örnekler etiketlerini tanımlayın. Örneğin, ikili sınıflandırma için, geçmiş olaylara göre özellikler oluşturun ve gelecekte "X" birim içinde gelecek olaylara göre Etiketler oluşturun (bkz. [özellik Mühendisliği](#feature-engineering) ve modelleme teknikleri bölümlerine bakın). Bu nedenle, bir örnek etiketleme zaman diliminde özelliklerini zaman diliminde daha sonra gelir.

Zamana bağlı bölme için, bir modelin eğileceği, bir modelin eğitilmesi için bir _eğitim kesme süresi<sub></sub>_  <sub>seçin.</sub> Eğitim verilerine T<sub>c</sub> 'nin ötesinde gelecek etiketlerin sızmasını engellemek için, eğitim örneklerini t<sub>c</sub>'den önceki X birimi olacak şekilde etiketlemek için en son saati seçin. Şekil 7'de gösterilen örnekte, bir kayıt özellikleri ve etiketleri yukarıda açıklanan şekilde burada hesaplanır veri kümesindeki her kare temsil eder. Şekil X = 2, W = 3 test etme ve eğitim gideceğine kayıtları gösterir:

![Şekil 7. İkili sınıflandırma için zamana bağımlı Böl](./media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

Şekil 7. İkili sınıflandırma için zamana bağımlı Böl

Yeşil kareler eğitim için kullanılan zaman birimi ait kayıtları temsil eder. Her eğitim örneği, özellik oluşturma için son üç nokta ve T<sub>c</sub>'den önce etiketlemek için sonraki iki nokta dikkate alındığında oluşturulur. Sonraki iki dönemin herhangi bir bölümü T<sub>c</sub>'nin ötesinde, t<sub>c</sub>'nin ötesinde görünürlük olmadığı için bu örneği eğitim verileri kümesinden hariç tutun.

Siyah kareler kayıtlarını yukarıdaki kısıtlama verilen eğitim veri kümesinde kullanılmamalıdır son etiketli veri kümesini temsil eder. Bu kayıtlar T<sub>c</sub>'den önce olduklarından verileri test etmek için de kullanılmaz. Ayrıca, etiketleme zaman çerçevelerine kısmen eğitim zaman karesinde ideal değil bağlıdır. Verileri eğitim ve test çerçevelerini ayrı ayrı etiketleme zaman etiketi bilgi sızdırılmasını önlemek için sahip olması gerekir.

Şimdiye kadar tartışılan teknik, eğitim ve test, T<sub>c</sub>yakınında zaman damgalarına sahip örnekler arasında örtüşmeye izin verir. Daha büyük ayrımı elde etmek için bir çözüm, test kümesinden T<sub>c</sub> zaman birimleri içindeki örnekleri hariç tutmasıdır. Ancak, agresif bir bölünmüş bol miktarda veri kullanılabilirliğine bağlıdır.

RUL tahmin etmek için kullanılan regresyon modellerini daha ciddi bir şekilde sızıntı sorundan etkilenen. Rastgele split yöntemini kullanarak, aşırı fazla sığdırma yol açar. Gerileme sorunları için bölme, T<sub>c</sub> 'den önceki hataları olan varlıklara ait kayıtlar eğitim kümesine gitmelidir. Kesme sonra hataları olan varlıklar kayıtlarını test kümesine gidin.

Eğitim ve test için veri bölmek için başka bir en iyi uygulama varlığı kimliğe göre bölme kullanmaktır. Eğitim kümesinde kullanılan varlıkları hiçbiri modeli performans testinde kullanılan şekilde bölme olmalıdır. Bu yaklaşımı kullanarak bir model ile yeni varlıkları daha gerçekçi sonuçları sağlamak daha iyi belirtme şansı olur.

### <a name="handling-imbalanced-data"></a>İmbalanced verileri işleme
Sınıflandırma sorunları ' nda, bir sınıfa ait diğer diğerlerinden daha fazla örnek varsa, veri kümesi _imdengeli_olarak kabul edilir. İdeal olarak, her sınıfta eğitim verileri, yeterli temsilcileri, farklı sınıflar arasında ayrım etkinleştirmek için tercih edilen. Bir sınıf % 10'dan az veri varsa veri imbalanced olarak kabul edilir. Eksik temsil edilen sınıfa _minınlık sınıfı_denir.

PdM karşılaşılan birçok sorun diğer sınıf veya sınıflar için karşılaştırıldığında burada bir sınıf ciddi bir şekilde bulamamış gibi imbalanced veri kümeleri, yüz tanıma. Bazı durumlarda, yalnızca %0,001 toplam veri noktası azınlık sınıfı oluşturan. Sınıf dengesizliği PdM için benzersiz değil. Hataları ve anomalileri nadir oluşum olduğu diğer etki alanları, örnekler, sahtekarlık algılama ve ağa yetkisiz erişim için benzer bir sorun karşı karşıyadır. Bu hatalar azınlık sınıf örnekleri yapın.

Bunlar genel hata oranının en aza indirmeyi amaçlamanız beri verilerde sınıf dengesizliği ile birçok standart öğrenme algoritmalarını performansını, güvenliği aşıldı. %99 negatif ve pozitif %1 örnek bir veri kümesi için tüm örnekleri negatif olarak etiketleme tarafından % 99 doğruluğunu sağlamak için bir model gösterilebilir. Ancak modeli yanlış pozitif tüm örnekler sınıflandırır. doğruluğunun yüksek olsa bile, bu nedenle algoritma yararlı bir değildir. Sonuç olarak, _hata oranına ilişkin genel doğruluk_ gibi geleneksel değerlendirme ölçümleri, imdengeli öğrenme için yetersizdir. İmbalanced veri kümeleri ile karşı karşıya kalındığında, diğer ölçümleri modeli değerlendirme için kullanılır:
- Duyarlık
- Geri çekme
- F1 puanları
- Ayarlanmış maliyeti ROC (alıcı çalıştırma özellikleri)

Bu ölçümler hakkında daha fazla bilgi için bkz. [model değerlendirmesi](#model-evaluation).

Ancak, sınıf onarmak yardımcı bazı yöntemler vardır dengesizliği sorun. Bu iki önemli örnek, _Örnekleme tekniklerine_ ve _Maliyet duyarlı öğrenimine_sahiptir.

#### <a name="sampling-methods"></a>Örnekleme yöntemi
İmbalanced learning eğitim veri kümesi için dengeli bir veri kümesini değiştirmek için yöntemleri örnekleme kullanımını içerir. Örnekleme test kümesine değil uygulanacak yöntemlerdir. Birçok örnekleme tekniği olsa da, çoğu düz iletme _rastgele büyük örneklemedir_ ve _örnekleme aşamasındadır_.

_Rastgele aşırı örnekleme_ , minınma sınıfından rastgele bir örnek seçmeyi, bu örneklerin çoğaltılmasını ve bunları eğitim verileri kümesine eklemeyi içerir. Sonuç olarak, azınlık sınıf örnekleri sayısı artar ve sonunda farklı sınıfların örnekleri sayısını dengelemek. Oversampling bir dezavantajı, bazı örnekleri birden çok örneğini atlayarak sığdırma önde gelen çok belirli olacak sınıflandırıcı neden olabilir ' dir. Model eğitim yüksek doğruluk gösterebilir, ancak performansını görünmeyen test veri çubuğunda yetersiz olabilir.

Bunun tersine, _örnekleme altında rastgele_ bir çoğunluk sınıfından rastgele bir örnek seçilir ve bu örnekler eğitim veri kümesinden kaldırılır. Ancak, çoğu sınıf örnekleri kaldırma çoğu sınıf için ilgili önemli kavramları gözden kaçırabilirsiniz sınıflandırıcı neden olabilir. Minının sınıfının üzerine örneklendiği ve çoğunluk sınıfının aynı anda örneklendiği _karma örnekleme_ , başka bir önemli yaklaşım.

Birçok gelişmiş örnekleme teknikler vardır. Veri özellikleri ve veri Bilimcisi tarafından yinelemeli denemeleri sonuçlarını seçilen teknik bağlıdır.

#### <a name="cost-sensitive-learning"></a>Hassas öğrenme maliyeti
PdM içinde azınlık sınıfı oluşturan normal örnekler değerinden daha fazla ilgi hatalarıdır. Bu nedenle hatalarda algoritması'nın performansına çoğunlukla biridir. Hatalı pozitif bir sınıf negatif bir sınıf olarak tahmin etmeye yönelik birden çok tersi maliyeti olabilir. Bu durum genellikle farklı sınıfları için eşit kaybı veya asimetrik maliyetini yanlış sınıflandırma öğeler olarak adlandırılır. İdeal sınıflandırıcı yüksek tahmin doğruluğunu çoğunluğu sınıfı kesinliğini ödün vermeden azınlık sınıfı teslim.

Bu dengeyi elde etmek için birden çok yolu vardır. Sorun eşit kaybı riskini azaltmak için yüksek bir maliyet azınlık sınıfın hatalı sınıflandırmayla atayın ve ilişkin genel maliyeti en aza indirmek deneyin. _Svms gibi algoritmalar (vektör makinelerini destekler)_ bu yöntemi doğal olarak benimseyerek, olumlu ve olumsuz örneklerin maliyetini eğitim sırasında belirtmesine izin verir. Benzer şekilde, _Artırılmış karar ağaçları_ gibi yükseltme yöntemleri genellikle imledengeli verilerle iyi performans gösterir.

## <a name="model-evaluation"></a>Modeli değerlendirme
Hatalı Sınıflandırma, yanlış alarm işletmeye maliyeti yüksek olduğu PdM senaryolar için önemli bir sorundur. Örneğin, altyapısı hata yanlış bir öngörü üzerinde temel Uçağın yerde kararını zamanlamaları dengesini bozarsınız hem seyahat planlarını. Bir makineden çevrimdışı bir montaj hattı alma gelir kaybına yol açabilir. Bu nedenle yeni test verilerini karşı doğru performans ölçümleri ile model değerlendirme kritik öneme sahiptir.

PdM modelleri değerlendirmek için kullanılan tipik bir performans ölçümleri, aşağıda ele alınmıştır:

- [Doğruluk](https://en.wikipedia.org/wiki/Accuracy_and_precision) , sınıflandırıcının performansını açıklamak için kullanılan en popüler ölçümdür. Ancak doğruluk veri dağıtımları için duyarlıdır ve verimsiz bir ölçüdür imbalanced veri kümeleriyle senaryolar için. Diğer ölçümleri yerine kullanılır. [Karışıklık matrisi](https://en.wikipedia.org/wiki/Confusion_matrix) gibi araçlar, modelin doğruluğunu ve nedenini hesaplamak için kullanılır.
- PdM modellerinin [Duyarlığı](https://en.wikipedia.org/wiki/Precision_and_recall) , yanlış alarmlar oranı ile ilgilidir. Daha düşük precision modelinin genellikle atamasından hatalı karşılık gelir.
- [Geri çağırma](https://en.wikipedia.org/wiki/Precision_and_recall) oranı, test kümesindeki hatalardan kaç tanesi model tarafından doğru şekilde tanımlandığını gösterir. Daha yüksek bir geri çağırma oranları modelin gerçek kod hataları tanımlamak başarılı olduğu anlamına gelir.
- [F1 puanı](https://en.wikipedia.org/wiki/F1_score) duyarlık ve geri çekmenin harmonik ortasıdır ve değeri 0 (en kötü) ile 1 (en iyi) arasında değişir.

İkili sınıflandırma için
- [Alıcı işletim eğrileri (ROC)](https://en.wikipedia.org/wiki/Receiver_operating_characteristic) de popüler bir ölçümdür. Model performansını ROC Eğriler bir sabit işletim noktası üzerinde ROC göre yorumlanır.
- Ancak, PdM sorunları için _tablo_ ve kaldırma _grafikleri_ daha bilgilendirici. Bunlar yalnızca pozitif sınıfta (hata) odaklanın ve algoritması performans, daha karmaşık bir resmini ROC eğrileri daha sağlar.
  - _Decle tabloları_ , test örnekleri kullanılarak, hata olasılıkların azalan sırada oluşturulur. Sıralı örnekleri ardından deciles gruplandırılır (10 oranında olasılığı en yüksek olan bir örnek daha sonra % %20 30 ve benzeri). Her decile için oranı (gerçek pozitif sonuç oranı) /(random baseline) her decile algoritması performansı tahmin etmenize yardımcı olur. Rastgele taban 0.1, 0.2 vb. değerlerine göre alır.
  - [Grafikleri kaldırın](http://www2.cs.uregina.ca/~dbd/cs831/notes/lift_chart/lift_chart.html) tüm kaldırmalarda, kaldırma gerçek pozitif oranı ile rastgele gerçek pozitif oranlarla çizim yapın. En büyük kazanç gösterdikleri ilk deciles genellikle odağı sonucu olduğundan. İlk deciles ayrıca "riskli", için temsili olarak PdM için kullanıldığında görülebilir.

## <a name="model-operationalization-for-predictive-maintenance"></a>Tahmine dayalı bakım için modeli kullanıma hazır hale getirme

Veri bilimi alıştırmadır avantajı, yalnızca, eğitilen model işletimsel yapılan gerçekleşmiş. Diğer bir deyişle, modelin yeni, daha önce görünmeyen verilerine dayalı olarak tahminlerde bulunmak üzere iş sistemleri uygulamasına dağıtılması gerekir.  Yeni verilerin eğitim modelinin _model imzasına_ tam olarak iki şekilde uyması gerekir:
- Tüm özellikleri yeni veriler her mantıksal örneğinde (örneğin bir tablosunda bir satıra) bulunması gerekir.
- Yeni verilerin önceden işlenmesi gerekir ve her bir özelliğin, eğitim verileri tam olarak aynı şekilde tasarlanmıştır.

Yukarıdaki işlem, akademik ve sektör belgeleri Birçok bakımdan belirtilir. Ancak, aşağıdaki ifadeleri de aynı anlama gelir:
- Modeli kullanarak _yeni verileri puan_ edin
- _Modeli_ yeni verilere Uygula
- Modeli _Operationleştir_
- Modeli _dağıtma_
- _Modeli_ yeni verilere karşı Çalıştır

Daha önce belirtildiği gibi PdM için modeli kullanıma hazır hale getirme eşlerine farklıdır. Anomali algılama ve hata algılamayı kapsayan senaryolar genellikle _çevrimiçi Puanlama_ ( _gerçek zamanlı Puanlama_olarak da adlandırılır) uygular. Burada, model her bir gelen kaydı _puanlar_ ve bir tahmin döndürür. Anomali algılama için tahmini bir anomali ortaya çıktığını göstergesidir (örnek: bir sınıf SVM). Hata algılama için türü ya da hata sınıfı olacaktır.

Buna karşılık PdM, _Batch Puanlama_içerir. Model imza uymak için yeni verileri özelliklerinde eğitim verilerini aynı şekilde mühendislik gerekir. Yeni veriler için tipik olan büyük veri kümeleri için özelliklerini zaman pencereleri toplanır ve toplu işlemde puanlanması. Toplu Puanlama, genellikle [Spark](https://spark.apache.org/) veya [Azure Batch](https://docs.microsoft.com/azure/batch/batch-api-basics)gibi dağıtılmış sistemlerde yapılır. Birkaç alternatifleri - yetersiz hem de vardır:
- Akış veri altyapıları, bellek içinde windows üzerinde toplama destekler. Bu nedenle çevrimiçi Puanlama destekledikleri tartışılabilir. Ancak bu sistemler üzerinde daha geniş windows için saat veya seyrek öğe dar Windows yoğun veri uygundur. Bunlar için de yoğun veri geniş zaman pencereleri PdM senaryolarında görülen ölçeği değil.
- Toplu Puanlama kullanılabilir değilse, aynı anda küçük partiler halinde yeni verileri işlemek için çevrimiçi Puanlama uyum çözümüdür.

## <a name="solution-templates-for-predictive-maintenance"></a>Tahmine dayalı bakım çözüm şablonları

Bu kılavuz son bölümü PdM çözüm şablonları, öğreticiler ve Azure'da gerçekleştirilen denemeleri listesini sağlar. Bazı durumlarda dakikalar içinde bir Azure aboneliğine bu PdM uygulamaları dağıtılabilir. Kavram kanıtı tanıtımlar sanal alternatifleri veya Hızlandırıcılar gerçek üretim uygulamaları için deneme amaçlı olarak kullanılabilir. Bu şablonlar [Azure yapay zeka Galerisi](https://gallery.azure.ai) veya [Azure GitHub](https://github.com/Azure)' da bulunur. Bu farklı örnekleri, zaman içinde bu çözüm şablonu alınacaktır.

| # | Başlık | Açıklama |
|--:|:------|-------------|
| 2 | [Azure tahmine dayalı bakım çözüm şablonu](https://github.com/Azure/AI-PredictiveMaintenance) | Azure ML modelleme ve IoT uzaktan izleme bağlamında tahmine dayalı bakım senaryolarını destekleyebilen tam bir Azure altyapısını gösteren açık kaynaklı bir çözüm şablonu. |
| 3 | [Tahmine dayalı bakım için derin öğrenme](https://github.com/Azure/MachineLearningSamples-DeepLearningforPredictiveMaintenance) | Tahmine dayalı bakım için LSTM (uzun kısa süreli bellek) ağları (yinelenen bir sinir Networks sınıfı) kullanarak Azure Not defteri 'nin [Bu örnekteki bir blog gönderisine](https://azure.microsoft.com/blog/deep-learning-for-predictive-maintenance)sahip olması.|
| 4 | [R 'de tahmine dayalı bakım modelleme Kılavuzu](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) | R betiklerini ile PdM modelleme Kılavuzu|
| 5 | [Aerospace için Azure tahmine dayalı bakım](https://gallery.azure.ai/Solution/Predictive-Maintenance-for-Aerospace-1) | Azure ML v1.0 uçak bakım göre ilk PdM çözüm şablonlarından biri. Bu kılavuz, bu projeden geldiğini. |
| 6 | [IoT Edge için Azure AI araç seti](https://github.com/Azure/ai-toolkit-iot-edge) | TensorFlow kullanarak IoT Edge AI araç seti, Azure IoT Edge uyumlu Docker kapsayıcılarındaki derin öğrenme modellerini paketler ve bu modelleri REST API 'Ler olarak kullanıma sunar.
| 7 | [Azure IoT tahmine dayalı bakım](https://github.com/Azure/azure-iot-predictive-maintenance) | Bilgisayarlar - Azure IOT paketi önceden yapılandırılmış çözümü. IOT paketi ile uçak bakım PdM şablonu. Aynı projeyle ilgili [başka bir belge](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-overview) ve [izlenecek yol](https://docs.microsoft.com/azure/iot-suite/iot-suite-predictive-walkthrough) . |
| 8 | [SQL Server R Services kullanarak tahmine dayalı bakım şablonu](https://gallery.azure.ai/Tutorial/Predictive-Maintenance-Template-with-SQL-Server-R-Services-1) | R hizmetlerini temel alarak kalan faydalı ömrü senaryo Tanıtımı. |
| 9 | [Tahmine dayalı bakım modelleme Kılavuzu](https://gallery.azure.ai/Collection/Predictive-Maintenance-Modelling-Guide-1) | R ile [denemeleri](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Experiment-1) ve [veri kümeleri](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Modelling-Guide-Data-Sets-1) ile [Azure Not defteri](https://gallery.azure.ai/Notebook/Predictive-Maintenance-Modelling-Guide-R-Notebook-1) ve [denemeleri](https://gallery.azure.ai/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2) kullanılarak tasarlanan airustmaintenance veri kümesi özelliği, AzureML v 1.0|

## <a name="training-resources-for-predictive-maintenance"></a>Tahmine dayalı bakım için eğitim kaynakları

Microsoft Azure, PdM teknikleri, içerik ve yapay ZEKA kavramları ve uygulama Genel eğitim yanı sıra temel kavramları öğrenme yollarını sunar.

| Eğitim kaynağı  | Kullanılabilirlik |
|:-------------------|--------------|
| [Ağaçlar ve rastgele orman kullanarak PdM için öğrenme yolu](https://aischool.microsoft.com/learning-paths/1H5vH5wAYcAy88CoQWQcA8) | Genel | 
| [Derin öğrenme kullanarak PdM için öğrenme yolu](https://aischool.microsoft.com/learning-paths/FSIXxYkOGcauo0eUO8qAS) | Genel |
| [Azure 'da AI geliştiricisi](https://azure.microsoft.com/training/learning-paths/azure-ai-developer) | Genel |
| [Microsoft AI okul](https://aischool.microsoft.com/learning-paths) | Genel |
| [GitHub 'dan Azure AI öğrenimi](https://github.com/Azure/connectthedots/blob/master/readme.md) | Genel |
| [LinkedIn öğrenme](https://www.linkedin.com/learning) | Genel |
| [Microsoft AI YouTube web seminerleri](https://www.youtube.com/watch?v=NvrH7_KKzoM&t=4s) | Genel |
| [Microsoft AI göster](https://channel9.msdn.com/Shows/AI-Show) | Genel |
| [LearnAI@MS](https://learnanalytics.microsoft.com) | İş Ortakları |
| [Microsoft İş Ortağı Ağı](https://learningportal.microsoft.com) | İş Ortakları |

Ayrıca, yapay ZEKA MOOCS (açık çevrimiçi kurslara) ücretsiz Stanford ve MIT gibi akademik kurumları tarafından çevrimiçi sunulan ve diğer eğitim şirketlerdir.
