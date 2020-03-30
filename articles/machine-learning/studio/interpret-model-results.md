---
title: Model sonuçlarını yorumlama
titleSuffix: ML Studio (classic) - Azure
description: Puan modeli çıktılarını kullanarak ve görselleştiren bir algoritma için en uygun parametre kümesini nasıl seçilir?
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 11/29/2017
ms.openlocfilehash: 9a0b855f48085138b28e02e0a5d01c5dd0f666be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218060"
---
# <a name="interpret-model-results-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da model sonuçlarını yorumlama (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]
Bu konu, Azure Machine Learning Studio'da (klasik) tahmin sonuçlarını nasıl görselleştirip yorumlanacağıaçıklanmaktadır. Bir modeli eğittikten ve bunun üzerine tahminlerde bulunduktan sonra ("modeli attı"), tahmin sonucunu anlamanız ve yorumlamanız gerekir.



Azure Machine Learning Studio'da dört ana tür makine öğrenme modeli vardır (klasik):

* Sınıflandırma
* Kümeleme
* Regresyon
* Tavsiye sistemleri

Bu modellerin üstünde tahmin için kullanılan modüller şunlardır:

* [Sınıflandırma][score-model] ve regresyon için Puan Modeli modülü
* [Kümeleme için Kümeler modülüne atama][assign-to-clusters]
* Tavsiye sistemleri için [Score Matchbox Tavsiye Cihazı][score-matchbox-recommender]

Bu belge, bu modüllerin her biri için tahmin sonuçlarının nasıl yorumlanacağı açıklanmaktadır. Bu modüllere genel bir bakış için Azure [Machine Learning Studio'da (klasik) algoritmalarınızı optimize etmek için parametreleri nasıl seçeceğinize](algorithm-parameters-optimize.md)bakın.

Bu konu, tahmin yorumunu ele almaktadır, ancak model değerlendirmesini ele almayız. Modelinizi nasıl değerlendirecekleriniz hakkında daha fazla bilgi için [Azure Machine Learning Studio'da (klasik) model performansını nasıl değerlendirebilirsiniz'](evaluate-model-performance.md)e bakın.

Azure Machine Learning Studio'da (klasik) yeniyseniz ve başlamak için basit bir deneme oluşturmak için yardıma ihtiyacınız varsa, [bkz.](create-experiment.md)

## <a name="classification"></a>Sınıflandırma
Sınıflandırma sorunlarının iki alt kategorisi vardır:

* Yalnızca iki sınıfla ilgili sorunlar (iki sınıflı veya ikili sınıflandırma)
* İkiden fazla sınıfla ilgili sorunlar (çok sınıflı sınıflandırma)

Azure Machine Learning Studio (klasik) bu tür sınıflandırmalarla başa çıkmak için farklı modüllere sahiptir, ancak tahmin sonuçlarını yorumlama yöntemleri benzerdir.

### <a name="two-class-classification"></a>İki sınıflı sınıflandırma
**Örnek deneme**

İki sınıflı sınıflandırma problemine örnek olarak iris çiçeklerin sınıflandırılması örnek tir. Görev iris çiçekleri özelliklerine göre sınıflandırmaktır. Azure Machine Learning Studio'da (klasik) sağlanan Iris veri seti, yalnızca iki çiçek türünün (sınıf 0 ve 1) örneklerini içeren popüler [Iris veri kümesinin](https://en.wikipedia.org/wiki/Iris_flower_data_set) bir alt kümesidir. Her çiçek için dört özellik (sepal uzunluğu, sepal genişliği, yaprak uzunluğu ve taç yaprağı genişliği) vardır.

![Iris deneyinin ekran görüntüsü](./media/interpret-model-results/1.png)

Şekil 1. Iris iki sınıflı sınıflandırma problemi deneyi

Şekil 1'de gösterildiği gibi, bu sorunu çözmek için bir deneme yapılmıştır. İki sınıf artırılmış karar ağacı modeli eğitildi ve puanlandı. Artık [Puan][score-model] Modeli modülünün çıkış portunu tıklayıp Visualize'ı tıklatarak [Puan Modeli][score-model] modülünden tahmin sonuçlarını **görselleştirebilirsiniz.**

![Puan modeli modülü](./media/interpret-model-results/1_1.png)

Bu, Şekil 2'de gösterildiği gibi puanlama sonuçlarını gündeme getirir.

![Iris iki sınıflı sınıflandırma deneyi sonuçları](./media/interpret-model-results/2.png)

Şekil 2. İki sınıflı sınıflandırmada bir puan modeli sonucunu görselleştirin

**Sonuç yorumu**

Sonuç tablosunda altı sütun vardır. Sol dört sütun dört özelliktir. Sağ iki sütun, Puanlı Etiketler ve Puanlı Olasılıklar, tahmin sonuçlarıdır. Puanlı Olasılıklar sütunu, bir çiçeğin pozitif sınıfa (Sınıf 1) ait olma olasılığını gösterir. Örneğin, sütundaki ilk sayı (0,028571) ilk çiçeğin Sınıf 1'e ait olma olasılığı 0,028571 olduğu anlamına gelir. Puanlı Etiketler sütunu, her çiçek için öngörülen sınıfı gösterir. Bu, Puanlı Olasılıklar sütununa dayanır. Bir çiçeğin puanlandırılmama olasılığı 0,5'ten büyükse, Sınıf 1 olarak tahmin edilir. Aksi takdirde, Sınıf 0 olarak tahmin edilir.

**Web hizmeti yayını**

Tahmin sonuçları anlaşıldıktan ve ses değerlendirildikten sonra, deney bir web hizmeti olarak yayınlanabilir, böylece çeşitli uygulamalarda dağıtabilir ve yeni iris çiçeği hakkında sınıf tahminleri elde etmek için arayabilirsiniz. Bir eğitim deneyinin bir puanlama deneyine nasıl dönüştürüleceğini öğrenmek ve bunu bir web hizmeti olarak yayımlamak için [Bkz.](tutorial-part3-credit-risk-deploy.md) Bu yordam, Şekil 3'te gösterildiği gibi bir puanlama deneyi sağlar.

![Puanlama deneyinin ekran görüntüsü](./media/interpret-model-results/3.png)

Şekil 3. Iris iki sınıflı sınıflandırma problemi deneyi puanlama

Şimdi web hizmeti için giriş ve çıktı ayarlamanız gerekir. Giriş, Iris çiçek özellikleri girişi olan [Score Model'in][score-model]doğru giriş noktasıdır. Çıktıseçimi, tahmin edilen sınıfla (puanlı etiket), puanlanan olasılık la mı yoksa her ikisiyle mi ilgilendiğinize bağlıdır. Bu örnekte, her ikisiyle de ilgilendiğiniz varsayılır. İstenilen çıkış sütunlarını seçmek için [Veri kümesi modülünde Sütunları Seç'i][select-columns] kullanın. [Veri kümesinde Sütun Ları Seç'i][select-columns]tıklatın, Başlat sütun **seçiciyi**tıklatın ve **Puanlı Etiketler** ve **Puanlı Olasılıkları**seçin. [Veri kümesinde Sütunseç'in][select-columns] çıkış bağlantı noktasını ayarladıktan ve yeniden çalıştırdıktan sonra, **WEB HİzMETİ YAYINLA'yı**tıklatarak puanlama denemesini bir web hizmeti olarak yayımlamaya hazır olmalısınız. Son deney Şekil 4'e benziyor.

![İris iki sınıflı sınıflandırma deneyi](./media/interpret-model-results/4.png)

Şekil 4. Iris iki sınıflı sınıflandırma probleminin son skorlama deneyi

Web hizmetini çalıştırdıktan ve bir test örneğinin bazı özellik değerlerini girdikten sonra, sonuç iki sayı döndürür. İlk sayı puanlı etiket, ikincisi ise puanlı olasılıktır. Bu çiçek 0.9655 olasılık ile Sınıf 1 olarak tahmin edilir.

![Test yorumlama puan modeli](./media/interpret-model-results/4_1.png)

![Puanlama test sonuçları](./media/interpret-model-results/5.png)

Şekil 5. Iris iki sınıf sınıflandırma web hizmeti sonucu

### <a name="multi-class-classification"></a>Çok sınıflı sınıflandırma
**Örnek deneme**

Bu denemede, çok sınıflı sınıflandırmaya örnek olarak harf tanıma görevi gerçekleştirirsiniz. Sınıflandırıcı, elle yazılmış resimlerden çıkarılan bazı el yazısı öznitelik değerlerini temel alan belirli bir harfi (sınıf) tahmin etmeye çalışır.

![Mektup tanıma örneği](./media/interpret-model-results/5_1.png)

Eğitim verilerinde, elle yazılmış mektup görüntülerinden çıkarılan 16 özellik vardır. 26 harf bizim 26 sınıfları oluşturur. Şekil 6, harf tanıma için çok sınıflı bir sınıflandırma modelini eğitecek ve bir test veri kümesinde ayarlanan aynı özellik üzerinde tahmin de bulunacak bir deneyi gösterir.

![Harf tanıma çok sınıflı sınıflandırma deneyi](./media/interpret-model-results/6.png)

Şekil 6. Harf tanıma çok sınıflı sınıflandırma problemi deneyi

[Score Model][score-model] modülünün çıkış portunu tıklatıp **Visualize'ı**tıklatarak [Skor Modeli][score-model] modülünden elde edilen sonuçları görselleştirerek Şekil 7'de gösterildiği gibi içeriği görmeniz gerekir.

![Puan modeli sonuçları](./media/interpret-model-results/7.png)

Şekil 7. Çok sınıflı bir sınıflandırmada puan model sonuçlarını görselleştirin

**Sonuç yorumu**

Sol daki 16 sütun, test kümesinin özellik değerlerini temsil ediyor. "XX" sınıfı için Puanlı Olasılıkları gibi adlara sahip sütunlar, iki sınıflı durumdaki Puanlı Olasılıklar sütununa benzer. Bunlar, karşılık gelen girişin belirli bir sınıfa düşme olasılığını gösterir. Örneğin, ilk giriş için 0,003571 olasılık bir "A", 0,000451 olasılık bir "B" ve benzeri olmasıdır. Son sütun (Puanlı Etiketler) iki sınıflı durumda Puanlı Etiketler ile aynıdır. İlgili girişin öngörülen sınıfı olarak en büyük puan olasılığına sahip sınıfı seçer. Örneğin, ilk giriş için, "F" olma olasılığı en yüksek olduğu için puanlı etiket "F" (0,916995) olur.

**Web hizmeti yayını**

Ayrıca, her giriş için puanlı etiketi ve puanlı etiket in olasılığını da alabilirsiniz. Temel mantık, tüm puanlama olasılıkları arasında en büyük olasılığı bulmaktır. Bunu yapmak için [Execute R Script][execute-r-script] modüllerini kullanmanız gerekir. R kodu Şekil 8'de, denemenin sonucu şekil 9'da gösterilmiştir.

![R kodu örneği](./media/interpret-model-results/8.png)

Şekil 8. Puanlı Etiketleri ve etiketlerin ilişkili olasılıklarını ayıklamak için R kodu

![Deney sonucu](./media/interpret-model-results/9.png)

Şekil 9. Harf tanıma çok sınıflı sınıflandırma probleminin son skorlama denemesi

Web hizmetini yayımlayıp çalıştırdıktan ve bazı giriş özelliği değerlerini girdikten sonra, döndürülen sonuç Şekil 10'a benzer. Bu el yazısıyla yazılmış 16 özelliği yle 0,9715 olasılıklı bir "T" olacağı tahmin edilir.

![Test yorumlama puan modülü](./media/interpret-model-results/9_1.png)

![Test sonucu](./media/interpret-model-results/10.png)

Şekil 10. Çok sınıflı sınıflandırmanın web hizmeti sonucu

## <a name="regression"></a>Regresyon
Regresyon problemleri sınıflandırma problemlerinden farklıdır. Bir sınıflandırma probleminde, iris çiçeğinin hangi sınıfa ait olduğu gibi ayrı kılıklı sınıfları tahmin etmeye çalışırsınız. Ancak aşağıdaki regresyon problemi örneğinde de görebileceğiniz gibi, bir arabanın fiyatı gibi sürekli bir değişkeni tahmin etmeye çalışıyorsunuz.

**Örnek deneme**

Gerileme için örnek olarak otomobil fiyat tahminini kullanın. Bir otomobilin fiyatını, make, fuel type, body type ve drive wheel gibi özelliklerine göre tahmin etmeye çalışıyorsunuz. Deney Şekil 11'de gösterilmiştir.

![Otomobil fiyat regresyon deneyi](./media/interpret-model-results/11.png)

Şekil 11. Otomobil fiyat regresyon sorunu deneyi

[Puan Modeli][score-model] modülünü görselleştirerek, sonuç Şekil 12'ye benzer.

![Otomobil fiyat tahmini sorunu için puanlama sonuçları](./media/interpret-model-results/12.png)

Şekil 12. Otomobil fiyat tahmini sorunu için puanlama sonucu

**Sonuç yorumu**

Puanlı Etiketler, bu puanlama sonucunun sonuç sütunudur. Sayılar her araç için öngörülen fiyat.

**Web hizmeti yayını**

Regresyon denemesini bir web hizmetinde yayımlayabilir ve otomobil fiyat tahmini için iki sınıflı sınıflandırma kullanım örneğinde olduğu gibi çağırabilirsiniz.

![Otomobil fiyat regresyon sorunu için puanlama deneyi](./media/interpret-model-results/13.png)

Şekil 13. Bir otomobil fiyat gerileme sorunu puanlama deneyi

Web hizmetini çalıştıran döndürülen sonuç Şekil 14'e benzer. Bu araba için öngörülen fiyat $15,085.52.

![Test yorumlama puanlama modülü](./media/interpret-model-results/13_1.png)

![Puanlama modülü sonuçları](./media/interpret-model-results/14.png)

Şekil 14. Bir otomobil fiyat gerileme sorunu Web hizmeti sonucu

## <a name="clustering"></a>Kümeleme
**Örnek deneme**

Kümeleme deneyi oluşturmak için Iris veri kümesini yeniden kullanalım. Burada, yalnızca özelliklere sahip olacak ve kümeleme için kullanılabilecek şekilde veri kümesindeki sınıf etiketlerini filtreleyebilirsiniz. Bu iris kullanım örneğinde, eğitim işlemi sırasında iki olacak küme sayısını belirtin, bu da çiçekleri iki sınıfa kümelediğiniz anlamına gelir. Deney Şekil 15'te gösterilmiştir.

![Iris kümeleme problemi deneyi](./media/interpret-model-results/15.png)

Şekil 15. Iris kümeleme problemi deneyi

Kümeleme, eğitim veri kümesinin tek başına temel doğruluk etiketleri olmaması açısından sınıflandırmadan farklıdır. Gruplandırma, eğitim veri kümesi örneklerini farklı kümeler halinde gruplandırma. Eğitim sürecinde, model özellikleri arasındaki farkları öğrenerek girişleri etiketler. Bundan sonra, eğitilen model gelecekteki girişleri daha da sınıflandırmak için kullanılabilir. Bir kümeleme problemi içinde ilgilendiğimiz sonucun iki bölümü vardır. İlk bölüm eğitim veri kümesini etiketlemek, ikinci bölüm ise yeni bir veri kümesini eğitilmiş modelle sınıflandırmaktır.

Sonucun ilk [bölümü, Tren Kümeleme Modeli'nin][train-clustering-model] sol çıkış bağlantı noktasını tıklatıp Visualize'ı tıklatarak **görselleştirilebilir.** Görselleştirme Şekil 16'da gösterilmiştir.

![Kümeleme sonucu](./media/interpret-model-results/16.png)

Şekil 16. Eğitim veri kümesi için kümeleme sonucunu görselleştirin

İkinci bölümün sonucu, yeni girişleri eğitilmiş kümeleme modeliyle kümeleme, Şekil 17'de gösterilmiştir.

![Kümeleme sonucunu görselleştir](./media/interpret-model-results/17.png)

Şekil 17. Kümeleme sonucunu yeni bir veri kümesinde görselleştirin

**Sonuç yorumu**

Her ne kadar iki parçanın sonuçları farklı deney aşamalarından kaynaklansa da, aynı görünür ve aynı şekilde yorumlanır. İlk dört sütun özellikleri vardır. Son sütun, Atamalar, tahmin sonucudur. Aynı sayı atanan girişlerin aynı kümede olduğu tahmin edilir, diğer bir deyişle benzerlikleri bir şekilde paylaşırlar (bu deneme varsayılan Öklid mesafe metriklerini kullanır). Küme sayısını 2 olarak belirttiğiniz için, Atamalar'daki girişler 0 veya 1 olarak etiketlenir.

**Web hizmeti yayını**

Kümeleme denemesini bir web hizmetinde yayımlayabilir ve tahminleri kümeleme için iki sınıflı sınıflandırma kullanım örneğinde olduğu gibi çağırabilirsiniz.

![Iris kümeleme sorunu için puanlama deneyi](./media/interpret-model-results/18.png)

Şekil 18. Iris kümeleme probleminin puanlama denemesi

Web hizmetini çalıştırdıktan sonra, döndürülen sonuç Şekil 19'a benzer. Bu çiçeğin küme 0'da olduğu tahmin edilir.

![Puanlama modüllerini test edin](./media/interpret-model-results/18_1.png)

![Puanlama modülü sonucu](./media/interpret-model-results/19.png)

Şekil 19. Iris iki sınıf sınıflandırma web hizmeti sonucu

## <a name="recommender-system"></a>Tavsiye sistemi
**Örnek deneme**

Tavsiye sistemleri için restoran tavsiyesi sorununu örnek olarak kullanabilirsiniz: müşteriler için derecelendirme geçmişlerine göre restoranlar önerebilirsiniz. Giriş verileri üç bölümden oluşur:

* Müşterilerden restoran derecelendirmeleri
* Müşteri özellik verileri
* Restoran özelliği verileri

Azure Machine Learning Studio'daki [Train Matchbox Recommender][train-matchbox-recommender] modülüyle yapabileceğimiz birkaç şey vardır (klasik):

* Belirli bir kullanıcı ve öğe için derecelendirmeleri tahmin etme
* Öğeleri belirli bir kullanıcıya önerme
* Belirli bir kullanıcıyla ilgili kullanıcıları bulma
* Belirli bir öğeyle ilgili öğeleri bulma

**Tavsiye tahmin türü** menüsündeki dört seçenekarasından seçim yaparak ne yapmak istediğinizi seçebilirsiniz. Burada dört senaryonun arasında da yürüyebilirsiniz.

![Kibrit kutusu tavsiye cihazı](./media/interpret-model-results/19_1.png)

Tavsiye sistemi için tipik bir Azure Machine Learning Studio (klasik) denemesi Şekil 20'ye benzer. Bu tavsiye sistemi modüllerinin nasıl kullanılacağı hakkında bilgi için [Train kibrit kutusu tavsiye cihazı][train-matchbox-recommender] ve Puan [kibrit kutusu tavsiye cihazı][score-matchbox-recommender]na bakın.

![Tavsiye sistemi deneyi](./media/interpret-model-results/20.png)

Şekil 20. Tavsiye sistemi deneyi

**Sonuç yorumu**

**Belirli bir kullanıcı ve öğe için derecelendirmeleri tahmin etme**

**Recommender tahmin türü**altında **Derecelendirme Tahmini'ni** seçerek, tavsiye eden sistemden belirli bir kullanıcı ve öğenin derecelendirmesini tahmin etmesini istersiniz. [Score Matchbox Recommender][score-matchbox-recommender] çıktısının görselleştirilmesi Şekil 21'e benzer.

![Tavsiye sisteminin puan sonucu -- derecelendirme tahmini](./media/interpret-model-results/21.png)

Şekil 21. Tavsiye sistemi-derecelendirme tahmini nin puan sonucunu görselleştirin

İlk iki sütun, giriş verileri tarafından sağlanan kullanıcı öğesi çiftleridir. Üçüncü sütun, belirli bir öğe için bir kullanıcının öngörülen derecelendirmesidir. Örneğin, ilk satırda, müşteri U1048'in restoran 135026'yı 2 olarak derecelendirecek şekilde tahmin edilir.

**Öğeleri belirli bir kullanıcıya önerme**

**Tavsiye tahmin türü**altında Öğe **Önerisi'ni** seçerek, tavsiye eden sistemden öğeleri belirli bir kullanıcıya tavsiye etmesini istersiniz. Bu senaryoda seçilecek son parametre *Önerilen madde seçimidir.* Nominal **Öğelerden (model değerlendirmesi için)** seçeneği öncelikle eğitim süreci sırasında model değerlendirmesi içindir. Bu tahmin aşaması için **Tüm Öğelerden'i**seçiyoruz. [Score Matchbox Recommender][score-matchbox-recommender] çıktısının görselleştirilmesi Şekil 22'ye benzer.

![Tavsiye sisteminin skor sonucu -- madde önerisi](./media/interpret-model-results/22.png)

Şekil 22. Tavsiye sisteminin puan sonucunu görselleştirin-- öğe önerisi

Altı sütundan ilki, giriş verilerinin sağladığı gibi, öğeleri öneren kullanıcı adlarını temsil eder. Diğer beş sütun, kullanıcıya önerilen öğeleri azalan alaka düzeyi sırasına göre temsil ediyor. Örneğin, ilk satırda, müşteri U1048 için en çok tavsiye edilen restoran 134986, 135018, 134975, 135021 ve 132862 izledi.

**Belirli bir kullanıcıyla ilgili kullanıcıları bulma**

**İlgili Kullanıcıları** **Recommender tahmin türü**altında seçerek, tavsiye eden sistemden belirli bir kullanıcıyla ilgili kullanıcıları bulmasını istersiniz. İlgili kullanıcılar benzer tercihleri olan kullanıcılardır. Bu senaryoda seçilecek son parametre *İlgili kullanıcı seçimidir.* Öğeleri **Derecelendiren Kullanıcılardan (model değerlendirmesi için)** seçeneği öncelikle eğitim süreci sırasında model değerlendirmesi içindir. Bu tahmin aşaması için **Tüm Kullanıcılardan** Birini Seçin. [Score Matchbox Recommender][score-matchbox-recommender] çıktısının görselleştirilmesi Şekil 23'e benzer.

![Tavsiye sisteminin skor sonucu --ilgili kullanıcılar](./media/interpret-model-results/23.png)

Şekil 23. Tavsiye sistemi ile ilgili kullanıcıların puan sonuçlarını görselleştirin

Altı sütunun ilki, giriş verilerinden sağlandığı üzere ilgili kullanıcıları bulmak için gereken kullanıcı adlarını gösterir. Diğer beş sütun, kullanıcının tahmin edilen ilgili kullanıcılarını azalan alaka düzeyi sırasına göre saklar. Örneğin, ilk satırda, müşteri U1048 için en alakalı müşteri U1051, u1066, U1044, U1017 ve U1072 izledi.

**Belirli bir öğeyle ilgili öğeleri bulma**

**Recommender tahmin türü**altında **İlgili Öğeleri** seçerek, tavsiye sistemi belirli bir öğeile ilgili öğeleri bulmak için soruyorsunuz. İlgili öğeler, aynı kullanıcı tarafından en çok beğenilen öğelerdir. Bu senaryoda seçilecek son parametre *İlgili madde seçimidir.* Nominal **Öğelerden (model değerlendirmesi için)** seçeneği öncelikle eğitim süreci sırasında model değerlendirmesi içindir. Bu tahmin aşaması için **Tüm Öğelerden Birini** seçiyoruz. [Score Matchbox Recommender][score-matchbox-recommender] çıktısının görselleştirilmesi Şekil 24'e benzer.

![Tavsiye sisteminin puan sonucu --ilgili öğeler](./media/interpret-model-results/24.png)

Şekil 24. Tavsiye sistemi ile ilgili öğelerin puan sonuçlarını görselleştirin

Altı sütundan ilki, giriş verileri tarafından sağlanan ilgili öğeleri bulmak için gereken öğe adlarını temsil eder. Diğer beş sütun, öğenin öngörülen ilgili öğelerini alaka düzeyi açısından azalan sırada depolar. Örneğin, ilk satırda, madde 135026 için en alakalı öğe 135074, 135035, 132875, 135055 ve 134992 izledi.

**Web hizmeti yayını**

Öngörüler almak için web hizmetleri olarak bu denemeleri yayımlama işlemi dört senaryonun her biri için benzerdir. Burada örnek olarak ikinci senaryoyu (öğeleri belirli bir kullanıcıya öner) alıyoruz. Diğer üçüyle aynı prosedürü uygulayabilirsiniz.

Eğitimli tavsiye sistemini eğitilmiş bir model olarak kaydetmek ve giriş verilerini istendiği gibi tek bir kullanıcı kimliği sütununa filtrelemek, denemeyi Şekil 25'te olduğu gibi bağlayabilir ve bir web hizmeti olarak yayımlayabilirsiniz.

![Restoran tavsiye sorunu puanlama deneyi](./media/interpret-model-results/25.png)

Şekil 25. Restoran tavsiye sorunu puanlama deneyi

Web hizmetini çalıştıran döndürülen sonuç Şekil 26'ya benzer. Kullanıcı U1048 için önerilen beş restoran 134986, 135018, 134975, 135021 ve 132862'dir.

![Tavsiye sistemi hizmeti örneği](./media/interpret-model-results/25_1.png)

![Örnek deney sonuçları](./media/interpret-model-results/26.png)

Şekil 26. Restoran tavsiyesi sorunu Web hizmeti sonucu

<!-- Module References -->
[assign-to-clusters]: https://msdn.microsoft.com/library/azure/eed3ee76-e8aa-46e6-907c-9ca767f5c114/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-matchbox-recommender]: https://msdn.microsoft.com/library/azure/55544522-9a10-44bd-884f-9a91a9cec2cd/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[train-clustering-model]: https://msdn.microsoft.com/library/azure/bb43c744-f7fa-41d0-ae67-74ae75da3ffd/
[train-matchbox-recommender]: https://msdn.microsoft.com/library/azure/fa4aa69d-2f1c-4ba4-ad5f-90ea3a515b4c/
