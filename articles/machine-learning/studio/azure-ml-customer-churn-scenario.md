---
title: Müşteri karmaşıklığını çözümle
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) kullanarak müşteri karmaşıklığı çözümlemek ve Puanlama yapmak için tümleşik bir model geliştirmeye yönelik örnek olay incelemesi.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 12/18/2017
ms.openlocfilehash: 79a7b157d5daf8940aba454808c65ed433cfe29c
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169006"
---
# <a name="analyze-customer-churn-using-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio kullanarak müşteri karmaşıklığını çözümleme (klasik)
## <a name="overview"></a>Genel Bakış
Bu makale, Azure Machine Learning Studio (klasik) kullanılarak oluşturulan bir müşteri karmaşıklığı analiz projesinin başvuru uygulamasını gösterir. Bu makalede, ilişkili genel modellerini bütünlüklü olarak endüstriyel müşteri karmaşıklığı sorununu çözmek için ele alır. Biz de Machine Learning kullanılarak oluşturulan model doğruluğunu ölçmek ve daha fazla geliştirme yönergeleri değerlendirin.  

### <a name="acknowledgements"></a>Bildirimler
Bu deneme, daha önce Microsoft Azure Machine Learning Studio (klasik) için üretim yöneticisi olan Serge Berger, Microsoft 'ta sorumlu veri Bilimcu ve Roger Barga tarafından geliştirilmiştir ve test edilmiştir. Azure belgeleri takımının minnettar uzmanlıklarını bildirir ve bu teknik incelemeyi paylaşmak için teşekkürler.

> [!NOTE]
> Bu deneme için kullanılan verileri genel olarak kullanılabilir değil. Dalgalanma analizi için makine öğrenimi modelinin nasıl oluşturulacağı hakkında bir örnek için, bkz. [Azure yapay zeka Galerisi](https://gallery.azure.ai/) [Perakende değişim modeli şablonu](https://gallery.azure.ai/Collection/Retail-Customer-Churn-Prediction-Template-1)
> 
> 



## <a name="the-problem-of-customer-churn"></a>Müşteri karmaşıklığı sorununu
İşletmeler tüketici pazarında ve tüm kurumsal kesimde, değişim sıklığı ile uğraşmak zorunda. Bazen değişim sıklığı aşırı ve ilke kararlarını etkiler. Geleneksel bir çözümün yüksek eğilimini churners tahmin edin ve kendi ihtiyaçlarına özel dispensations uygulayarak veya pazarlama kampanyaları, bir concierge hizmetini aracılığıyla adres sağlamaktır. Bu yaklaşım, sektör sektör farklılık gösterebilir. Bunlar bile belirli tüketicinin kümeden bir endüstri (örneğin, telekomünikasyon) içinde başka bir farklılık gösterebilir.

İşletmeler, bu özel müşteri elde tutma çalışmaları en aza indirmek gereken ortak faktördür. Bu nedenle, her müşteri dalgalanması olasılığını ile puan ve üst N olanları adres için doğal bir Metodoloji olacaktır. En iyi müşteriler En Karlı olanları olabilir. Örneğin, daha karmaşık senaryolarda, kar işlevi özel dispensation için adayları seçim sırasında kullanılır. Ancak, bu tam stratejisi karmaşası başa çıkmak için yalnızca bir kısmını faktörlerdir. İşletmelerin de hesap risk (ve ilişkili risk toleransınıza) düzeyini ve müdahale ve yatkýn müşteri Segmentasyonu maliyetini uygulamanız gerekir.  

## <a name="industry-outlook-and-approaches"></a>Sektör outlook ve yaklaşımları
Gelişmiş işleme değişim olgun bir sektör bir işarettir. Klasik örnek, sıklıkla bir sağlayıcısından diğerine geçmek için aboneleri burada bilinen telekomünikasyon endüstri standardıdır. Gönüllü Bu karmaşıklığı prime bir konudur. Üstelik, sağlayıcılar, müşterilerin geçiş yapmak için gereken faktörler olan *karmaşıklık sürücüleri*hakkında önemli bilgi edindi.

Cep telefonu iş değişim ahize veya cihaz seçim örneği için bilinen bir sürücüdür. Sonuç olarak, bir popüler ahize fiyatı subsidize yeni abonelere yönelik ve mevcut müşteriler bir yükseltme için tam bir fiyat uyguladığınız ilkesidir. Tarihsel olarak, bu ilke, yeni indirim almak için bir sağlayıcısından diğerine atlamalı müşterilere açmıştır. Bu, istenir, kendi stratejileri iyileştirmek için sağlayıcıları.

Yüksek dalgalanma ahize tekliflere değişim geçerli ahize modellerde dayalı modelleri hızla çıkarır bir faktördür. Ayrıca, cep telefonları yalnızca telekomünikasyon cihazları değil; bunlar da biçimde deyimleri (iPhone göz önünde bulundurun). Bu sosyal adaylarının normal telekomünikasyon veri kümesi kapsamı dışında olan.

Net modelleme için bilinen nedenlerle karmaşıklığı ortadan kaldırarak bir ses ilke insanlara olamaz sonucudur. Aslında, kategorik değişkenlerin (karar ağaçları gibi) miktarını karşılayan klasik modeller dahil sürekli bir modelleme stratejisi **zorunludur**.

Müşterilerinin büyük veri kümelerini kullanarak, kuruluşların büyük veri analizi (özellikle, büyük verilere dayalı değişim saptama içinde) sorun yönelik etkili bir yaklaşım olarak çalışıp çalışmadığını denetleyin. ETL bölümüne önerileri karmaşıklığı sorununu büyük veri yaklaşımı hakkında daha fazla bilgi bulabilirsiniz.  

## <a name="methodology-to-model-customer-churn"></a>Model müşteri kaybı için yöntemi
Şekil 1-3'te Müşteri dalgalanması çözmek için yaygın bir sorun çözme işlemi gösterilmiştir:  

1. Bir risk modeli olasılığını ve risk eylemleri nasıl etkileyeceğini göz önünde bulundurun sağlar.
2. Bir araya modeli, değişim sıklığı ve müşteri miktarını olasılığını müdahale düzeyini nasıl etkileyebilecek göz önünde bulundurun (CLV) ömrü değeri sağlar.
3. Bu analiz, kendisini en iyi öneri sunmak için müşteri segmentlerini hedefleyen proaktif bir pazarlama kampanyası için ilerletilmiş bir quantitative analiz için uygundur.  

![Risk toleransı ve karar modellerinin eyleme dönüştürülebilir Öngörüler nasıl oluşturduğunu gösteren diyagram](./media/azure-ml-customer-churn-scenario/churn-1.png)

İleri görünümlü bu yaklaşım karmaşası değerlendirmek için en iyi yoludur, ancak karmaşıklığı ile gelir: çok modelli archetype ve modelleri arasındaki bağımlılıkları izleme geliştirmek sunuyoruz. Aşağıdaki diyagramda gösterildiği gibi modelleri arasındaki etkileşimi kapsüllenmiş:  

![Dalgalanma modeli etkileşim diyagramı](./media/azure-ml-customer-churn-scenario/churn-2.png)

*Şekil 4: Birleşik çok modelli, arşiv Etype*  

Müşteri bekletme için bütünsel bir yaklaşım sunmak için ise modelleri arasındaki etkileşimi anahtardır. Her bir modelin zaman içinde düşmesi gerekli; Bu nedenle, mimari kapalı bir döngüdür (net/DM veri araştırma standardı tarafından ayarlanan, [***3***]).  

Risk karar pazarlama kesimleme/ayrıştırma genel döngüsü hala birçok iş sorunlarını için geçerli olan bir genelleştirilmiş, yapısıdır. Basitleştirilmiş Tahmine dayalı bir çözüm izin vermeyen bir karmaşık iş sorunun tüm nitelikler sergilediğinden karmaşıklığı analizi yalnızca bir güçlü sorunları bu grubun temsilcisidir. Değişim sıklığı için modern yaklaşımı sosyal yönlerini özellikle yaklaşımda vurgulanmış değil, ancak bunlar herhangi bir modelde olduğu gibi sosyal özelliklerini modelleme archetype içinde kapsüllenir.  

Burada ilginç bir ayrıca büyük veri analizi ' dir. Günümüzün telekomünikasyon ve perakende işletmeler, müşterilerine hakkında ayrıntılı verileri toplamak ve biz kolayca çok modelli bağlantısı için gereken belirli eğilimler nesnelerin interneti gibi Gelişmekte olan ve her yerde bulunan bir genel eğilim olacak öngörüyor birden çok katman akıllı çözümler kullanmak istemiyorsunuz iş izin cihazlar.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio-classic"></a>Machine Learning Studio (klasik) üzerinde modelleyen modelleme 'yi uygulama
Açıklanan sorun verildiğinde, tümleşik modelleme ve Puanlama yaklaşımı uygulamak için en iyi yol nedir? Bu bölümde, Azure Machine Learning Studio (klasik) kullanarak bunu nasıl gerçekleştirebileceğinizi gösteririz.  

Çok modelli bir yaklaşım genel bir archetype değişim sıklığı için tasarlarken zorunluluktur. Çok modelli bir yaklaşım bile Puanlama (Tahmine dayalı) parçası olması gerekir.  

Aşağıdaki diyagramda, karmaşıklığın tahmin edilmesi için Machine Learning Studio (klasik) üzerinde dört Puanlama algoritması kullanan oluşturduğumuz prototipi gösterilmektedir. Çok modelli bir yaklaşım kullanarak nedenini değil yalnızca bir topluluğu Sınıflandırıcısı, doğruluğunu artırmak için ancak de aşırı sığdırma karşı korumak ve öngörücü özellik seçimi artırmak için oluşturmaktır.  

![Birçok bağlantılı modülle karmaşık bir Studio (klasik) çalışma alanını gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-3.png)

*Şekil 5: bir dalgalanma modelleme yaklaşımını prototip*  

Aşağıdaki bölümler Machine Learning Studio (klasik) kullanarak uyguladığımız prototip Puanlama modeliyle ilgili daha fazla ayrıntı sağlar.  

### <a name="data-selection-and-preparation"></a>Veri seçimi ve hazırlama
Veri modelleri oluşturmak için kullanılan ve puanı müşterilerin müşteri gizliliğini korumak için farklı verilerle CRM dikey çözümden elde. Veri ABD'deki 8000 abonelikler hakkında bilgi içerir ve üç kaynağı birleştirir: veri (abonelik meta veriler), etkinlik verileri (kullanım sistemin) ve müşteri destek verileri sağlama. Veriler müşterilerle ilgili iş bilgilerini içermez; Örneğin, bağlılık programı meta verilerini veya kredi puanlarını içermez.  

Veri hazırlama zaten sahip olduğunu varsaydığından kolaylık olması için ETL ve verileri temizleme işlemleri kapsam dışına başka bir yerde yapılır.

Modelleme için özellik seçimi adaylarının, rastgele orman modülü kullanan işlemine dahil kümesinin başlangıç anlam Puanlama temel alır. Machine Learning Studio (klasik) uygulamasında uygulama için, temsilci özellikleri için Ortalama, ortanca ve aralıkları hesapladık. Örneğin, kullanıcı etkinliği için minimum ve maksimum değerleri gibi nitel veri toplamaları ekledik.

Ayrıca en son altı ay boyunca zamana bağlı bilgileri yakaladığımız. Verileri bir yıl boyunca analiz ettik ve olmasa bile istatistiksel olarak önemli eğilimleri, değişim sıklığı üzerindeki etkisini önemli ölçüde altı ay sonra düşer kuruldu.  

En önemli nokta, ETL, özellik seçimi ve modelleme dahil olmak üzere tüm işlemin, Microsoft Azure veri kaynakları kullanılarak Machine Learning Studio (klasik) olarak uygulandığına göre belirlenir.   

Aşağıdaki diyagramlarda kullanılan verileri gösterilmektedir.  

![Ham değerlerle kullanılan verilerin bir örneğini gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-4.png)

*Şekil 6: veri kaynağı alıntısı (karıştırılmış)*  

![Veri kaynağından ayıklanan istatistiksel özellikleri gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-5.png)

*Şekil 7: veri kaynağından ayıklanan Özellikler*
 

> Bu veriler özeldir ve bu nedenle modeli ve veri paylaşılamaz unutmayın.
> Ancak, genel olarak kullanılabilir verileri kullanan benzer bir model için [Azure yapay zeka Galerisi](https://gallery.azure.ai/): [Telco Müşteri karmaşıklığı](https://gallery.azure.ai/Experiment/31c19425ee874f628c847f7e2d93e383)' nda bu örnek deneyime bakın.
> 
> Cortana Intelligence Suite kullanarak bir dalgalanma Analizi modeli uygulama hakkında daha fazla bilgi edinmek için [Bu videoyu](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) , üst düzey Program Yöneticisi Wee Hyong tok tarafından da öneririz. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Prototip kullanılan algoritmalar
Prototip (özelleştirme yok) oluşturmak için aşağıdaki dört makine öğrenimi algoritmaları kullanılır:  

1. Lojistik regresyon (LR)
2. Artırmalı karar ağacı (BT)
3. Ortalama perceptron (AP)
4. Destekli vektör makinesi (SVM)  

Aşağıdaki diyagram, modelleri oluşturulduğu dizisini gösterir deneme tasarım yüzeyine bir bölümünü gösterir:  

![Studio deneme tuvalinin küçük bir bölümünün ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-6.png)  

*Şekil 8: Machine Learning Studio modeller oluşturma (klasik)*  

### <a name="scoring-methods"></a>Puanlama yöntemleri
Biz olan dört model oluşturduğunuz bir etiketli bir eğitim veri kümesi kullanarak puanlanmış.  

Biz de SAS Kurumsal Miner 12 Masaüstü sürümü kullanılarak oluşturulan bir karşılaştırılabilir modeli Puanlama veri kümesine gönderilen. SAS modelinin ve dört Machine Learning Studio (klasik) modellerin doğruluğunu ölçüyoruz.  

## <a name="results"></a>Sonuçlar
Bu bölümde, bizim bulguları Puanlama veri kümesini temel alan bir model doğruluğunu hakkında size sunar.  

### <a name="accuracy-and-precision-of-scoring"></a>Doğruluk ve puanlamasını duyarlık
Genellikle, Azure Machine Learning Studio (klasik) uygulamasındaki uygulama,% 10-15 (eğri veya AUC altındaki alan) ile aynı şekilde SAS 'nin arkasında olur.  

Bununla birlikte, karmaşıklık bakımından en önemli ölçüm, yanlış sınıflandırma oranıdır: Yani sınıflandırıcının tahmin edilebileceği, gerçekten ne kadar **karmaşıklamadığı ve** henüz özel bir işleme aldıkları, en önemli N çüleri. Aşağıdaki diyagram bu modelleri misclassification ücretine karşılaştırılır:  

![4 algoritmaların performansını karşılaştıran eğri grafiğinin alanı](./media/azure-ml-customer-churn-scenario/churn-7.png)

*Şekil 9: eğri altındaki Passau prototip alanı*

### <a name="using-auc-to-compare-results"></a>Sonuçları karşılaştırmak için AUC kullanma
Eğri altındaki alan (AUC), pozitif ve negatif popülasyonlar için puanlar dağıtımları *arasındaki genel* bir ölçü ölçüsünü temsil eden bir ölçümdür. Geleneksel alıcı işleci özellikleri (ROC) grafiğe benzer, ancak AUC ölçüm eşiği değeri seçmenizi gerektirmeyeceğini bir önemli fark vardır. Bunun yerine, **Tüm** olası seçimler üzerinde sonuçları özetler. Buna karşılık, dikey eksen ve hatalı pozitif sonuç oranı yatay eksende pozitif sonuç oranı geleneksel ROC grafik gösterir ve sınıflandırma eşiği değişir.   

AUC, modellerin AUC değerleri aracılığıyla karşılaştırılmasını sağladığından farklı algoritmalar (veya farklı sistemler) için değer ölçüsü olarak kullanılır. Bu, sektörde meteorology ve biosciences gibi popüler bir yaklaşımdır. Bu nedenle, AUC sınıflandırıcı performansını değerlendirmek için popüler bir aracı temsil eder.  

### <a name="comparing-misclassification-rates"></a>Karşılaştırma misclassification oranları
Biz misclassification ücretler söz konusu veri kümesinde yaklaşık 8000 aboneliklerinin CRM verilerinizi kullanarak karşılaştırılır.  

* 10-%15 SAS misclassification gönderebilme hızıydı.
* Machine Learning Studio (klasik) yanlış sınıflandırma oranı, en üstteki 200-300 churanlar için% 15-20 ' di.  

Telekomünikasyon sektörün concierge hizmetini veya diğer özel olarak değerlendirilmesi sunarak olasılığı en yüksek riskli sahip müşteriler ele almak önemlidir. Bu şekilde Machine Learning Studio (klasik) uygulama, SAS modeliyle birlikte sonuçlara erişir.  

Biz genellikle olası churners doğru sınıflandırma ilgilendiğiniz aynı şekilde, doğruluk duyarlığından daha fazla önemlidir.  

Aşağıdaki Wikipedia diyagramdan canlı, anlaşılması kolay bir grafik ilişkiyi göstermektedir:  

![İki hedef. Bir hedef isabet işaretlerini gevşek gruplanmış, ancak "düşük doğruluk: iyi, zayıf hassasiyet" olarak işaretlenen Bulls-Eye yakın şekilde gösterir. Başka bir hedef sıkı bir şekilde gruplandırılır, ancak "düşük doğruluk: zayıf gerçeği, iyi duyarlık" olarak işaretlendi.](./media/azure-ml-customer-churn-scenario/churn-8.png)

*Şekil 10: doğruluk ve duyarlık arasında zorunluluğunu getirir*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Artırmalı karar ağacı modeli doğruluğu ve duyarlık sonuçları
Aşağıdaki grafikte en doğru olan dört model arasında özelleştirmede artırmalı karar ağacı modeli için Machine Learning'i prototype kullanarak Puanlama ham sonuçları görüntüler:  

![Dört algoritma için doğruluk, duyarlık, geri çekme, F puanı, AUC, ortalama günlük kaybı ve eğitim günlüğü kaybını gösteren tablo kod parçacığı](./media/azure-ml-customer-churn-scenario/churn-9.png)

*Şekil 11: Artırılmış karar ağacı model özellikleri*

## <a name="performance-comparison"></a>Performans karşılaştırma
Machine Learning Studio (klasik) modellerini ve SAS kurumsal Miner 12,1 Masaüstü sürümü kullanılarak oluşturulan karşılaştırılabilir bir modeli kullanarak verilerin puanlanması hızını karşılaştırdık.  

Algoritmaların performansını aşağıdaki tabloda özetlenmiştir:  

*Tablo 1. Algoritmaların genel performansı (doğruluğu)*

| LR | BT | AP | SVM |
| --- | --- | --- | --- |
| Ortalama modeli |En iyi modeli |Yeterli performansa sahip olmayan |Ortalama modeli |

Machine Learning Studio (klasik) içinde barındırılan modeller yürütme hızı için% 15-25 oranında gerçekleştirilmiş, ancak doğruluk büyük ölçüde ağırlıklıydı.  

## <a name="discussion-and-recommendations"></a>Tartışma ve öneriler
Telekomünikasyon sektörün değişim sıklığı, analiz etmek için çeşitli yöntemler çıkmıştır dahil olmak üzere:  

* Dört temel kategorileri için ölçümleri türetilir:
  * **Varlık (örneğin, bir abonelik)** . Abonelik ve/veya değişim konusu müşteri hakkındaki temel bilgileri sağlayın.
  * **Etkinlik**. Örneğin, oturum açma sayısı varlıkla ilgili tüm olası kullanım bilgilerini edinin.
  * **Müşteri desteği**. Aboneliğin sorunları veya müşteri desteği ile etkileşim var olup olmadığını belirtmek için müşteri destek günlüklerinden bilgi toplar.
  * **Rekabet ve iş verileri**. Tüm olası müşteri bilgilerini elde (örneğin, kullanılamıyor veya izlemek zor olabilir).
* Önem derecesi için sürücü özellik seçimi kullanın. Bu, artırmalı karar ağacı modeli her zaman taahhüdü bir yaklaşım olduğunu gösterir.  

Bu dört kategorinin kullanılması, kategori başına makul faktörlere göre biçimlendirilmiş olan dizinlere dayalı basit bir *belirleyici* yaklaşımın belirlenmesi, müşterileri karmaşıklık açısından riske almak için yeterli olmalıdır. Ne yazık ki atayabiliyoruz yatkýn görünüyor olsa da, bu yanlış bir anlayış olur. , Değişim sıklığı geçici etkisidir ve karmaşıklığı süresini etkileyen faktörleri genellikle geçici bir durumda olmadığından nedenidir. Bugün bırakarak dikkate alınması gereken bir müşterinin ne müşteri adayları yarın farklı olabilir ve bu kesinlikle altı ay bundan farklı olacaktır. Bu nedenle, bir *dayalı* modeli bir zorunludur.  

Bu önemli gözlem genellikle iş zekası odaklı bir yaklaşım analizi için genellikle tercih ettiği iş kaçan, çoğunlukla, olduğundan daha kolay bir satış ve basit bir Otomasyon admits.  

Ancak, Machine Learning Studio (klasik) kullanarak Self Servis analizinin taahhüdünü, bölüm veya departmana göre oluşan dört bilgi kategorisinin, karmaşıklık hakkında makine öğrenimi için değerli bir kaynak haline gelmesinin nedeni.  

Azure Machine Learning Studio (klasik) ile ilgili başka bir heyecan verici yetenek, zaten kullanılabilir olan önceden tanımlanmış modüller deposuna özel bir modül ekleyebilme özelliğidir. Bu özellik, temelde, kitaplığı seçin ve dikey pazarları için şablonlar oluşturma fırsatı oluşturur. Pazar yerinde Azure Machine Learning Studio (klasik) 'nin önemli bir farklıdır.  

Bu konuda daha sonra devam etmek özellikle büyük veri analizi ile ilgili umuyoruz.
  

## <a name="conclusion"></a>Sonuç
Bu yazıda, genel framework kullanarak genel müşteri karmaşıklığı sorununu giderme mantıklı bir yaklaşım açıklanmaktadır. Puanlama modelleri için bir prototip kabul ettik ve Azure Machine Learning Studio (klasik) kullanarak uyguladık. Son olarak, doğruluk ve performans SAS karşılaştırılabilir algoritmalar onaylamaz prototip çözümün değerlendirdik.  

 

## <a name="references"></a>Başvurular
[1] tahmine dayalı analiz: tahminlerin ötesinde, W. Mckgece, bilgi yönetimi, Temmuz/Ağustos 2011, p. 18-20.  

[2] Vikipedi makalesi: [doğruluk ve duyarlık](https://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [net-DM 1,0: adım adım veri araştırma Kılavuzu](https://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [büyük veri Pazarlaması: müşterilerinize daha etkin ve sürücü değeri de katılın](https://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Azure yapay zeka Galerisi](https://gallery.azure.ai/) Içindeki [Telco karmaşıklığı modeli şablonu](https://gallery.azure.ai/Experiment/Telco-Customer-Churn-5) 
 

## <a name="appendix"></a>Ek
![Dalgalanma prototipi üzerinde bir sununun anlık görüntüsü](./media/azure-ml-customer-churn-scenario/churn-10.png)

*Şekil 12: bir sununun dalgalanma prototipi üzerinde anlık görüntüsü*
