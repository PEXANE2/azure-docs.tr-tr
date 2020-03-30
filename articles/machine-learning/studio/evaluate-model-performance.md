---
title: Model performansını değerlendirme
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da (klasik) model performansını nasıl değerlendireceklerini ve bu görev için kullanılabilen ölçümlerhakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18, previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 3c041834b9ad191817cdf1380b0a75efc7639bd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218150"
---
# <a name="how-to-evaluate-model-performance-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da model performansı nasıl değerlendirilir (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Bu makalede, Azure Machine Learning Studio'da (klasik) bir modelin performansının nasıl değerlendirilene kadar değerlendirilebildiğini göstermekte ve bu görev için kullanılabilen ölçümlerin kısa bir açıklamasını sunmuştur. Üç yaygın denetimli öğrenme senaryosu sunulmuştur: 

* Regresyon
* ikili sınıflandırma 
* çok sınıflı sınıflandırma



Bir modelin performansını değerlendirmek, veri bilimi sürecinin temel aşamalarından biridir. Bir veri kümesinin puanlama (tahminleri) eğitimli bir model tarafından ne kadar başarılı olduğunu gösterir. 

Azure Machine Learning Studio (klasik), model değerlendirmesini iki ana makine öğrenme modülü aracılığıyla destekler: [Modeli Değerlendirin][evaluate-model] ve [Modeli Çapraz Doğrulayın.][cross-validate-model] Bu modüller, makine öğrenimi ve istatistikte yaygın olarak kullanılan bir dizi ölçüm açısından modelinizin nasıl performans gösterdiğini görmenizi sağlar.

## <a name="evaluation-vs-cross-validation"></a>Değerlendirme ve Çapraz Doğrulama
Değerlendirme ve çapraz doğrulama, modelinizin performansını ölçmenin standart yollarıdır. Her ikisi de inceleyebilir veya diğer modellerin bu karşılaştırıldığında değerlendirme ölçümleri oluşturun.

[Değerlendirme Modeli,][evaluate-model] puanlanmış bir veri kümesini giriş olarak bekler (veya iki farklı modelin performansını karşılaştırmak isterseniz iki). Bu nedenle, sonuçları değerlendirmek için önce [Modelinizi Tren Modeli][train-model] modüllerini kullanarak eğitmeniz ve [Puan Modeli][score-model] modüllerini kullanarak bazı veri setinde öngörülerde bulunmanız gerekir. Değerlendirme, puanlanan etiketlere/olasılıklara ve gerçek etiketlere dayanır ve bunların hepsi [Puan Modeli][score-model] modülü tarafından üretilir.

Alternatif olarak, giriş verilerinin farklı alt kümelerinde otomatik olarak bir dizi tren puanı değerlendirme işlemi (10 kat) gerçekleştirmek için çapraz doğrulamayı kullanabilirsiniz. Giriş verileri, biri test için ayrılmış, diğeri eğitim için 9 bölüme ayrılmıştır. Bu işlem 10 kez tekrarlanır ve değerlendirme ölçümleri ortalamalanır. Bu, bir modelin yeni veri kümelerine ne kadar iyi genelleştirileceğini belirlemede yardımcı olur. [Çapraz Doğrulama Modeli][cross-validate-model] modülü eğitimsiz bir model alır ve bazı etiketli veri seti ve ortalama sonuçlara ek olarak, 10 kıvrımher değerlendirme sonuçlarını çıkar.

Aşağıdaki bölümlerde, basit regresyon ve sınıflandırma modelleri oluşturacak ve hem [Model'i değerlendirin][evaluate-model] hem de [Çapraz Doğrulama Modeli][cross-validate-model] modüllerini kullanarak performanslarını değerlendireceğiz.

## <a name="evaluating-a-regression-model"></a>Regresyon Modelinin Değerlendirilmesi
Boyutlar, beygir gücü, motor özellikleri ve benzeri özellikleri kullanarak bir otomobilin fiyatını tahmin etmek istediğimizi varsayalım. Bu, hedef değişkenin *(fiyatın)* sürekli sayısal bir değer olduğu tipik bir regresyon sorunudur. Belirli bir arabanın özellik değerleri göz önüne alındığında, o arabanın fiyatını tahmin edebilirsiniz doğrusal bir regresyon modeli sığdırabilirsiniz. Bu regresyon modeli, eğittiğimiz veri kümesini elde etmek için kullanılabilir. Tahmin edilen otomobil fiyatlarını gördükten sonra, tahminlerin ortalama gerçek fiyatlardan ne kadar saptadığını bakarak model performansını değerlendirebiliriz. Bunu göstermek için, Machine Learning *Studio'daki (klasik)* Kayıtlı Veri **Setleri** bölümünde bulunan Otomobil fiyat verilerini (Raw) veri kümesini kullanırız.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Azure Machine Learning Studio'da (klasik) çalışma alanınıza aşağıdaki modülleri ekleyin:

* Otomobil fiyat verileri (Raw)
* [Çizgisel Regresyon][linear-regression]
* [Model Eğitme][train-model]
* [Modeli Puanlama][score-model]
* [Modeli Değerlendirme][evaluate-model]

Aşağıda Şekil 1'de gösterildiği gibi bağlantı noktalarını bağlayın ve [Tren Modeli][train-model] modülünün Etiket sütununa *fiyat*ayarlayın.

![Regresyon Modelinin Değerlendirilmesi](./media/evaluate-model-performance/1.png)

Şekil 1. Regresyon Modelinin Değerlendirilmesi.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme Sonuçlarının İncelenmesi
Denemeyi çalıştırdıktan sonra, [Model'i Değerlendir][evaluate-model] modülünün çıkış bağlantı noktasını tıklatabilir ve değerlendirme sonuçlarını görmek için *Visualize'ı* seçebilirsiniz. Regresyon modelleri için kullanılabilir değerlendirme ölçümleri şunlardır: *Ortalama Mutlak Hata*, Kök Ortalama Mutlak *Hata*, *Göreli Mutlak Hata*, *Göreli Kareli Hata*, ve Kararlılık *Katsayısı*.

Buradaki "hata" terimi, öngörülen değer le gerçek değer arasındaki farkı temsil eder. Bu farkın mutlak değeri veya karesi genellikle tüm örneklerde hatanın toplam büyüklüğünü yakalamak için hesaplanır, çünkü tahmin edilen ve gerçek değer arasındaki fark bazı durumlarda negatif olabilir. Hata ölçümleri, bir regresyon modelinin öngörülerinin gerçek değerlerden ortalama sapması açısından tahmin performansını ölçer. Daha düşük hata değerleri, modelin öngörülerde daha doğru olduğu anlamına gelir. Genel hata ölçümü sıfır, modelin verilere mükemmel uyduğu anlamına gelir.

R karesi olarak da bilinen belirleme katsayısı, modelin verilere ne kadar uyduğunu ölçmenin standart bir yoludur. Bu model tarafından açıklanan varyasyon oranı olarak yorumlanabilir. Daha yüksek bir oran 1 mükemmel bir uyum gösterir bu durumda, daha iyidir.

![Lineer Regresyon Değerlendirme Ölçümleri](./media/evaluate-model-performance/2.png)

Şekil 2. Lineer Regresyon Değerlendirme Ölçümleri.

### <a name="using-cross-validation"></a>Çapraz Doğrulama kullanma
Daha önce de belirtildiği gibi, [Çapraz-Doğrula Modeli][cross-validate-model] modüllerini kullanarak tekrarlanan eğitim, puanlama ve değerlendirmeleri otomatik olarak gerçekleştirebilirsiniz. Bu durumda ihtiyacınız olan tek şey bir veri kümesi, eğitimsiz bir model ve [Çapraz Doğrulama Modeli][cross-validate-model] modülüdür (aşağıdaki şemaya bakın). Etiket sütununa [Çapraz Doğrula Modeli][cross-validate-model] modülünün özelliklerinde *fiyat* ayarlamalısınız.

![Bir Regresyon Modelinin Çapraz Doğrulama](./media/evaluate-model-performance/3.png)

Şekil 3. Bir Regresyon Modeli çapraz doğrulama.

Denemeyi çalıştırdıktan sonra, [Çapraz-Onula Modeli][cross-validate-model] modülünün sağ çıkış bağlantı noktasına tıklayarak değerlendirme sonuçlarını inceleyebilirsiniz. Bu, her yineleme (kat) için ölçümlerin ve ölçümlerin her birinin ortalama sonuçlarının ayrıntılı bir görünümünü sağlar (Şekil 4).

![Bir Regresyon Modelinin Çapraz Doğrulama Sonuçları](./media/evaluate-model-performance/4.png)

Şekil 4. Bir Regresyon Modelinin Çapraz Doğrulama Sonuçları.

## <a name="evaluating-a-binary-classification-model"></a>İkili Sınıflandırma Modelinin Değerlendirilmesi
İkili sınıflandırma senaryosunda, hedef değişkenin yalnızca iki olası sonucu vardır: {0, 1} veya {false, true}, {negative, positive}. Size bazı demografik ve istihdam değişkenleri olan yetişkin çalışanların bir veri kümesi verildiğini ve gelir düzeyini tahmin etmenizin istendiğini varsayalım, {"<=50 K", ">50 K" değerlerine sahip bir ikili değişken. Başka bir deyişle, negatif sınıf yılda 50 K'den az veya eşit kazanan çalışanları, pozitif sınıf ise diğer tüm çalışanları temsil eder. Gerileme senaryosunda olduğu gibi, bir model eğitecek, bazı veriler elde eder ve sonuçları değerlendiririz. Buradaki temel fark, Azure Machine Learning Studio (klasik) hesaplama ve çıktılarının seçimidir. Gelir düzeyi tahmin senaryosunu göstermek için, Bir Studio (klasik) deney oluşturmak ve yaygın olarak kullanılan ikili sınıflandırıcı olan iki sınıflı lojistik regresyon modelinin performansını değerlendirmek için [Yetişkin](https://archive.ics.uci.edu/ml/datasets/Adult) veri kümesini kullanacağız.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Azure Machine Learning Studio'da (klasik) çalışma alanınıza aşağıdaki modülleri ekleyin:

* Yetişkin Nüfus Sayımı Gelir İkili Sınıflandırması veri seti
* [Çift Sınıflı Lojistik Regresyon][two-class-logistic-regression]
* [Model Eğitme][train-model]
* [Modeli Puanlama][score-model]
* [Modeli Değerlendirme][evaluate-model]

Şekil 5'te gösterildiği gibi bağlantı noktalarını bağlayın ve [Tren Modeli][train-model] modülünün Etiket sütununa *gelir*ayarlayın.

![İkili Sınıflandırma Modelinin Değerlendirilmesi](./media/evaluate-model-performance/5.png)

Şekil 5. İkili Sınıflandırma Modelinin Değerlendirilmesi.

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme Sonuçlarının İncelenmesi
Denemeyi çalıştırdıktan [sonra, Model'i Değerlendir][evaluate-model] modülünün çıkış noktasına tıklayabilir ve değerlendirme sonuçlarını görmek için *Visualize'ı* seçebilirsiniz (Şekil 7). İkili sınıflandırma modelleri için mevcut değerlendirme ölçümleri şunlardır: *Doğruluk*, *Hassaslık*, *Geri Çağırma*, *F1 Puanı*, ve *AUC*. Buna ek olarak, modül gerçek pozitif, yanlış negatifler, yanlış pozitifler ve gerçek negatiflerin yanı sıra *ROC,* *Precision/Recall*ve *Lift* eğrilerinin sayısını gösteren bir karışıklık matrisi çıkar.

Doğruluk sadece doğru sınıflanmış örneklerin oranıdır. Genellikle bir sınıflandırıcı değerlendirirken baktığınız ilk metriktir. Ancak, test verileri dengesizolduğunda (örneklerin çoğunun sınıflardan birine ait olduğu durumlarda) veya sınıflardan birinin performansıyla daha çok ilgileniyorsanız, doğruluk bir sınıflandırıcının etkinliğini gerçekten yakalamaz. Gelir düzeyi sınıflandırma senaryosunda, örneklerin %99'unun yılda 50.000'den az veya eşit kazanan kişileri temsil ettiği bazı verileri test ettiğinizi varsayalım. Tüm örnekler için "<=50K" sınıfını tahmin ederek 0,99 doğruluk elde etmek mümkündür. Bu durumda sınıflandırıcı genel olarak iyi bir iş yapıyor gibi görünüyor, ama gerçekte, yüksek gelirli bireylerin herhangi bir sınıflandırmak için başarısız (%1) Doğru.

Bu nedenle, değerlendirmenin daha belirli yönlerini yakalayan ek ölçümleri hesaplamak yararlıdır. Bu tür ölçümlerin ayrıntılarına girmeden önce, ikili sınıflandırma değerlendirmesinin kafa karışıklığı matrisini anlamak önemlidir. Eğitim kümesindeki sınıf etiketleri, genellikle pozitif veya negatif olarak adlandırdığımız yalnızca iki olası değer alabilir. Bir sınıflandırıcının doğru tahmin ettiği olumlu ve olumsuz örneklersırasıyla true positives (TP) ve true negatives (TN) olarak adlandırılır. Benzer şekilde, yanlış sınıflandırılan örneklere yanlış pozitif (FP) ve yanlış negatifler (FN) denir. Karışıklık matrisi yalnızca bu dört kategorinin her birine düşen örnek sayısını gösteren bir tablodur. Azure Machine Learning Studio (klasik), veri kümesindeki iki sınıftan hangisinin pozitif sınıf olduğuna otomatik olarak karar verir. Sınıf etiketleri Boolean veya tamsayılars ise, 'true' veya '1' etiketli örnekler pozitif sınıf atanır. Etiketler gelir veri kümesi gibi dizeleri ise, etiketler alfabetik olarak sıralanır ve ikinci düzey pozitif sınıf ise ilk düzey negatif sınıf olarak seçilir.

![İkili Sınıflandırma Karışıklık Matrisi](./media/evaluate-model-performance/6a.png)

Şekil 6. İkili Sınıflandırma Karışıklık Matrisi.

Gelir sınıflandırma sıcanına geri dönersek, kullanılan sınıflandırıcının performansını anlamamıza yardımcı olacak çeşitli değerlendirme soruları sormak isteriz. Doğal bir soru şudur: 'Modelin 50 K (TP+FP) >kazandığı tahmin edilen kişilerden kaç tanesi doğru olarak sınıflandırıldı (TP)?' Bu soruya doğru sınıflandırılan **Precision** pozitiflerin oranı olan modelin Hassasiyeti'ne bakarak cevaplanabilir: TP/(TP+FP). Başka bir yaygın soru "50k (TP + FN) >geliri olan tüm yüksek kazançlı çalışanların dışında, kaç sınıflandırıcı doğru sınıflandırıcı (TP) sınıflandırmak yaptı". Bu aslında **Recall**, ya da gerçek pozitif oranı: TP / (TP + FN) sınıflandırıcı. Kesinlik ve hatırlama arasında bariz bir denge olduğunu fark edebilirsiniz. Örneğin, göreceli olarak dengeli bir veri kümesi göz önüne alındığında, çoğunlukla olumlu örnekleri tahmin eden bir sınıflandırıcı, yüksek bir geri çağırma olurdu, ancak olumsuz örneklerin çoğu yanlış sınıflandırılatır ve çok sayıda yanlış pozitif sonuçla sonuçlanır. Bu iki ölçümün nasıl değiştiğine ilişkin bir çizim görmek için, değerlendirme sonucu çıktı sayfasındaki **PRECISION/RECALL** eğrisine tıklayabilirsiniz (Şekil 7'nin sol üst kısmı).

![İkili Sınıflandırma Değerlendirme Sonuçları](./media/evaluate-model-performance/7.png)

Şekil 7. İkili Sınıflandırma Değerlendirme Sonuçları.

Sık sık kullanılan bir diğer ilgili metrik **F1 Puanı**, dikkate hem hassasiyet ve hatırlama alır. Bu iki ölçümün harmonik ortalamasıdır ve şu şekilde hesaplanır: F1 = 2 (hassas x hatırlama) / (hassas + geri çağırma). F1 puanı değerlendirmeyi tek bir sayıda özetlemek için iyi bir yoldur, ancak bir sınıflandırıcının nasıl bir şekilde nasıl hissettiğini daha iyi anlamak için hem hassasiyete bakmak hem de birlikte hatırlamak her zaman iyi bir uygulamadır.

Buna ek olarak, **alıcı çalışma karakteristik (ROC)** eğrisi ve **Eğri (AUC)** değeri altında ilgili Alan yanlış pozitif oranı karşı gerçek pozitif oranı inceleyebilirsiniz. Bu eğri sol üst köşeye ne kadar yakınsa, sınıflandırıcının performansı o kadar iyi olur (yanlış pozitif oranı en aza indirirken gerçek pozitif oranı en üst düzeye çıkarmaktır). Çizimin diyagonaline yakın olan eğriler, rasgele tahmine yakın tahminler yapma eğiliminde olan sınıflandırıcılardan kaynaklanır.

### <a name="using-cross-validation"></a>Çapraz Doğrulama kullanma
Regresyon örneğinde olduğu gibi, verilerin farklı alt kümelerini otomatik olarak sürekli eğitmek, puanlamak ve değerlendirmek için çapraz doğrulama gerçekleştirebiliriz. Benzer şekilde, Çapraz [Doğrulama Modeli][cross-validate-model] modüllerini, eğitimsiz bir lojistik regresyon modelini ve bir veri kümesini kullanabiliriz. Etiket sütunu, [Çapraz Doğrula Modeli][cross-validate-model] modülünün özelliklerinde *gelir* elde etmek için ayarlanmalıdır. Denemeyi çalıştırdıktan ve [Çapraz-Doğrulama Modeli][cross-validate-model] modülünün doğru çıkış noktasına tıkladıktan sonra, her bir inin ortalama ve standart sapmasına ek olarak her bir kat için ikili sınıflandırma metrik değerlerini görebiliriz. 

![İkili Sınıflandırma Modelinin Çapraz Doğrulanması](./media/evaluate-model-performance/8.png)

Şekil 8. İkili Sınıflandırma Modeli çapraz doğrulama.

![İkili Sınıflandırıcının Çapraz Doğrulama Sonuçları](./media/evaluate-model-performance/9.png)

Şekil 9. İkili Sınıflandırıcının Çapraz Doğrulama Sonuçları.

## <a name="evaluating-a-multiclass-classification-model"></a>Çok Sınıflı Sınıflandırma Modelinin Değerlendirilmesi
Bu deneyde, iris tesisinin üç farklı türü (sınıfı) örneklerini içeren popüler [Iris](https://archive.ics.uci.edu/ml/datasets/Iris "ıris") veri kümesini kullanacağız. Her örnek için dört özellik değeri (sepal uzunluk/genişlik ve yaprak uzunluğu/genişliği) vardır. Önceki deneylerde, modelleri aynı veri kümelerini kullanarak eğittik ve test ettik. Burada, verilerin iki alt kümesini oluşturmak, ilkinde eğitim almak ve ikincisinde puan almak ve değerlendirmek için [Bölünmüş Veri][split] modüllerini kullanacağız. Iris veri kümesi UCI Machine [Learning Deposu'nda](https://archive.ics.uci.edu/ml/index.html)herkese açıktır ve Bir Alma [Verisi][import-data] modülü kullanılarak indirilebilir.

### <a name="creating-the-experiment"></a>Deneme oluşturma
Azure Machine Learning Studio'da (klasik) çalışma alanınıza aşağıdaki modülleri ekleyin:

* [Veri İthalat][import-data]
* [Çok Sınıflı Karar Ormanı][multiclass-decision-forest]
* [Verileri Bölme][split]
* [Model Eğitme][train-model]
* [Modeli Puanlama][score-model]
* [Modeli Değerlendirme][evaluate-model]

Aşağıda Şekil 10'da gösterildiği gibi bağlantı noktalarını bağlayın.

[Tren Modeli][train-model] modülünün Etiket sütun dizini 5 olarak ayarlayın. Veri kümesinin üstbilgi satırı yoktur, ancak sınıf etiketlerinin beşinci sütunda olduğunu biliyoruz.

[Veri Alma][import-data] modülüne tıklayın ve *Veri kaynağı* özelliğini HTTP üzerinden Web *URL'sine*ve *URL'ye* http://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.dataayarlayın.

[Bölünmüş Veri][split] modülünde (örneğin 0,7) eğitim için kullanılacak örneklerin kesirini ayarlayın.

![Çok Sınıflı Sınıflandırıcıyı Değerlendirme](./media/evaluate-model-performance/10.png)

Şekil 10. Çok Sınıflı Sınıflandırıcıyı Değerlendirme

### <a name="inspecting-the-evaluation-results"></a>Değerlendirme Sonuçlarının İncelenmesi
Denemeyi çalıştırın ve [Model'i Değerlendir'in][evaluate-model]çıkış bağlantı noktasına tıklayın. Değerlendirme sonuçları bu durumda bir karışıklık matrisi şeklinde sunulur. Matris, her üç sınıf için de gerçek ve öngörülen örnekleri gösterir.

![Çok Sınıflı Sınıflandırma Değerlendirme Sonuçları](./media/evaluate-model-performance/11.png)

Şekil 11. Çok Sınıflı Sınıflandırma Değerlendirme Sonuçları.

### <a name="using-cross-validation"></a>Çapraz Doğrulama kullanma
Daha önce de belirtildiği gibi, [Çapraz-Doğrula Modeli][cross-validate-model] modüllerini kullanarak tekrarlanan eğitim, puanlama ve değerlendirmeleri otomatik olarak gerçekleştirebilirsiniz. Bir veri kümesi, eğitimsiz bir model ve [Çapraz Doğrulama Modeli][cross-validate-model] modülü gerekir (aşağıdaki şemaya bakın). Yine [Çapraz Doğrula Modeli][cross-validate-model] modülünün etiket sütununa (bu durumda sütun dizini 5) ayarlamanız gerekir. Denemeyi çalıştırdıktan ve [Çapraz Doğrulama Modeli'nin][cross-validate-model]doğru çıkış bağlantı noktasını tıklattıktan sonra, her kıvrım için metrik değerlerin yanı sıra ortalama ve standart sapmayı da inceleyebilirsiniz. Burada görüntülenen ölçümler ikili sınıflandırma durumunda tartışılanlara benzer. Ancak, çok sınıflı sınıflandırmada, genel pozitif veya negatif sınıf olmadığı için, doğru pozitif/negatifve yanlış pozitif/negatiflerin hesaplanması sınıf başına bazda sayılarak yapılır. Örneğin, 'Iris-setosa' sınıfının kesinliği veya geri çağırması hesaplanırken, bunun pozitif sınıf ve diğer tüm diğerlerinin negatif olduğu varsayılır.

![Çok Sınıflı Sınıflandırma Modelinin Çapraz Doğrulanması](./media/evaluate-model-performance/12.png)

Şekil 12. Çok Sınıflı Sınıflandırma Modelinin Çapraz Doğrulanması.

![Çok Sınıflı Sınıflandırma Modelinin Çapraz Doğrulama Sonuçları](./media/evaluate-model-performance/13.png)

Şekil 13. Çok Sınıflı Sınıflandırma Modelinin Çapraz Doğrulama Sonuçları.

<!-- Module References -->
[cross-validate-model]: https://msdn.microsoft.com/library/azure/75fb875d-6b86-4d46-8bcc-74261ade5826/
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[multiclass-decision-forest]: https://msdn.microsoft.com/library/azure/5e70108d-2e44-45d9-86e8-94f37c68fe86/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-logistic-regression]: https://msdn.microsoft.com/library/azure/b0fd7660-eeed-43c5-9487-20d9cc79ed5d/
