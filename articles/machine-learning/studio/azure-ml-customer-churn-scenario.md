---
title: Müşteri karmaşanı analiz edin
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) kullanarak müşteri karmaşalarını analiz etmek ve puanlamak için entegre bir model geliştirme örneği çalışması.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 12/18/2017
ms.openlocfilehash: 4cf918abae51ca330054ef86e57095d29a21a37a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204537"
---
# <a name="analyze-customer-churn-using-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio (klasik) kullanarak Müşteri Karmaşası'nı analiz edin

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure Machine Learning Studio (klasik) kullanılarak oluşturulmuş bir müşteri karmaşası analiz projesinin başvuru uygulaması sunulur. Bu makalede, endüstriyel müşteri karmaşası sorununu bütünsel olarak çözmek için ilişkili genel modelleri tartışıyoruz. Ayrıca Machine Learning kullanılarak üretilen modellerin doğruluğunu ölçer ve daha fazla geliştirme için yol tariflerini değerlendiririz.  

### <a name="acknowledgements"></a>Bildirimler
Bu deney, Microsoft'un Baş Veri BilimCisi Serge Berger ve microsoft Azure Machine Learning Studio'nun (klasik) eski Ürün Müdürü Roger Barga tarafından geliştirilmiş ve test edilmiştir. Azure dokümantasyon ekibi uzmanlıklarını minnetle kabul eder ve bu teknik incelemeyi paylaştıkları için teşekkür eder.

> [!NOTE]
> Bu deneme için kullanılan veriler herkese açık değildir. Karmaşa analizi için bir makine öğrenme modeli oluşturma nın bir örneği için bkz: [Azure AI Galerisi'nde](https://gallery.azure.ai/) [perakende karmaşalı model şablonu](https://gallery.azure.ai/Collection/Retail-Customer-Churn-Prediction-Template-1)
> 
> 



## <a name="the-problem-of-customer-churn"></a>Müşteri karmaşası sorunu
Tüketici pazarında ve tüm kurumsal sektörlerdeki işletmeler churn ile uğraşmak zorunda. Bazen çalkalama aşırı ve politika kararlarını etkiler. Geleneksel çözüm, yüksek yoğunluklu churners tahmin etmek ve bir konsiyerj hizmeti, pazarlama kampanyaları, ya da özel dağıtımlar uygulayarak ihtiyaçlarını gidermektir. Bu yaklaşımlar endüstriden sanayiye farklılık gösterebilir. Hatta belirli bir tüketici kümesinden bir endüstride (örneğin, telekomünikasyon) diğerine değişebilirler.

Ortak faktör işletmelerin bu özel müşteri tutma çabalarını en aza indirmek gerekir. Böylece, doğal bir metodoloji çalkalama olasılığı ile her müşteri puan ve üst N olanları adres olacaktır. En iyi müşteriler en karlı olanlar olabilir. Örneğin, daha karmaşık senaryolarda, özel dağıtım için adayların seçimi sırasında bir kar işlevi kullanılır. Ancak, bu hususlar churn ile başa çıkmak için tam stratejinin sadece bir parçasıdır. İşletmeler ayrıca riski (ve ilişkili risk toleransını), müdahalenin düzeyini ve maliyetini ve makul müşteri segmentasyonunu da dikkate almak zorundadır.  

## <a name="industry-outlook-and-approaches"></a>Sektörgörünümü ve yaklaşımları
Churn sofistike işleme olgun bir endüstrinin bir işaretidir. Klasik örnek, abonelerin sık sık bir sağlayıcıdan diğerine geçiş yaptığı bilinen telekomünikasyon sektörüdür. Bu gönüllü churn önemli bir konudur. Ayrıca, sağlayıcılar *churn sürücüleri*hakkında önemli bilgiler birikmiş , hangi geçiş müşterileri sürücü faktörlerdir.

Örneğin, cep telefonu veya cihaz seçimi cep telefonu iş churn iyi bilinen bir sürücüdür. Sonuç olarak, popüler bir politika yeni aboneler için bir cep dili fiyatını sübvanse etmek ve bir yükseltme için mevcut müşterilere tam bir fiyat şarj etmektir. Tarihsel olarak, bu ilke müşterilerin yeni bir indirim almak için bir sağlayıcıdan diğerine atlamalı yol açmıştır. Bu da sağlayıcıların stratejilerini geliştirmelerine yol açtı.

Yakışıklı teklifleri yüksek volatilite hızla mevcut yakışıklı modellerine dayalı churn modelleri geçersiz bir faktördür. Ayrıca, cep telefonları sadece telekomünikasyon cihazları değil, onlar da moda deyimleri (iPhone düşünün). Bu sosyal öngörüler düzenli telekomünikasyon veri kümelerinin kapsamı dışındadır.

Modelleme için net sonuç sadece churn için bilinen nedenleri ortadan kaldırarak bir ses politikası tasarlayamazsınız. Aslında, kategorik değişkenleri (karar ağaçları gibi) ölçen klasik modeller de dahil olmak üzere sürekli bir modelleme stratejisi **zorunludur.**

Kuruluşlar, müşterilerine büyük veri kümelerini kullanarak, soruna etkili bir yaklaşım olarak büyük veri analitiği (özellikle büyük verilere dayalı karmaşa algılama) gerçekleştirmektedir. ETL ile ilgili öneriler bölümünde churn sorununa yönelik büyük veri yaklaşımı hakkında daha fazla bilgi bulabilirsiniz.  

## <a name="methodology-to-model-customer-churn"></a>Müşteri karmaşasını modelleme metodolojisi
Müşteri karmaşasını çözmek için ortak bir problem çözme süreci Şekil 1-3'te gösterilmiştir:  

1. Risk modeli, eylemlerin olasılık ve riski nasıl etkilediğini düşünmenizi sağlar.
2. Bir müdahale modeli, müdahale düzeyinin çalkalama olasılığını ve müşteri yaşam boyu değer (CLV) miktarını nasıl etkileyebileceğini düşünmenize olanak tanır.
3. Bu analiz, en iyi teklifi sunmak için müşteri segmentlerini hedefleyen proaktif bir pazarlama kampanyasına yükseltilen nitel bir analize bağlıdır.  

![Risk toleransı artı karar modellerinin nasıl eyleme geçirilebilir öngörüler verdiğini gösteren diyagram](./media/azure-ml-customer-churn-scenario/churn-1.png)

Bu ileriye dönük yaklaşım churn tedavi etmek için en iyi yoldur, ama karmaşıklığı ile birlikte gelir: biz çok modelli bir arketip geliştirmek ve modeller arasında iz bağımlılıkları geliştirmek zorunda. Modeller arasındaki etkileşim aşağıdaki diyagramda gösterildiği gibi kapsüllenebilir:  

![Çalkalama modeli etkileşim diyagramı](./media/azure-ml-customer-churn-scenario/churn-2.png)

*Şekil 4: Birleşik çok modelli arketip*  

Müşteri tutma için bütünsel bir yaklaşım sunmak istiyorsak, modeller arasındaki etkileşim çok önemlidir. Her model mutlaka zaman içinde bozulur; bu nedenle, mimari örtük bir döngüdür (CRISP-DM veri madenciliği standardı tarafından belirlenen arketipe benzer, [***3***]).  

Risk-karar-pazarlama segmentasyonu/ayrıştırma genel döngüsü hala birçok iş sorunları için geçerli olan genelleştirilmiş bir yapıdır. Basitleştirilmiş bir tahmine dayalı çözüme izin vermeyen karmaşık bir iş sorununun tüm özelliklerini sergilediği için churn analizi bu sorun grubunun güçlü bir temsilcisidir. Modern yaklaşımdan çalkalama yaklaşımının sosyal yönleri yaklaşımda özellikle vurgulanmaz, ancak sosyal yönleri herhangi bir modelde olduğu gibi modelleme arketipinde de özetlenmiştir.  

Burada ilginç bir ek büyük veri analitiği olduğunu. Günümüzün telekomünikasyon ve perakende işletmeleri müşterileri hakkında kapsamlı veriler toplar ve Nesnelerin İnterneti ve her yerde bulunan eğilimler göz önüne alındığında, çok modelli bağlantı ihtiyacının ortak bir eğilim haline geldiğini kolayca öngörebiliriz. iş birden fazla katmanda akıllı çözümler istihdam etmek için izin veren cihazlar.  

 

## <a name="implementing-the-modeling-archetype-in-machine-learning-studio-classic"></a>Machine Learning Studio'da modelleme arketipinin uygulanması (klasik)
Açıklanan sorun göz önüne alındığında, entegre bir modelleme ve puanlama yaklaşımı uygulamak için en iyi yolu nedir? Bu bölümde, Azure Machine Learning Studio'u (klasik) kullanarak bunu nasıl başardığımızı göstereceğiz.  

Churn için küresel bir arketip tasarlarken çok modelli yaklaşım bir zorunluluktur. Yaklaşımın puanlama (tahmine dayalı) kısmı bile çok modelli olmalıdır.  

Aşağıdaki diyagram, machine learning studio (klasik) churn tahmin etmek için dört puanlama algoritmaları kullanan oluşturulan prototip gösterir. Çok modelli bir yaklaşımıkullanmanın nedeni, yalnızca doğruluğu artırmak için bir topluluk sınıflandırıcı oluşturmak değil, aynı zamanda aşırı montaja karşı korumak ve önceden yazılmış özellik seçimini geliştirmektir.  

![Birbirine bağlı birçok modüle sahip karmaşık bir Studio (klasik) çalışma alanını gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-3.png)

*Şekil 5: Çalkalama modelleme yaklaşımının prototipi*  

Aşağıdaki bölümlerde Machine Learning Studio (klasik) kullanılarak uyguladığımız prototip puanlama modeli hakkında daha fazla bilgi verilmiştir.  

### <a name="data-selection-and-preparation"></a>Veri seçimi ve hazırlanması
Modelleri oluşturmak ve müşterileri puanlamak için kullanılan veriler CRM dikey çözümden elde edildi ve veriler müşteri gizliliğini korumak için gizlendi. Veriler ABD'de 8.000 abonelik hakkında bilgi içerir ve üç kaynağı birleştirir: veri sağlama (abonelik meta verileri), etkinlik verileri (sistemin kullanımı) ve müşteri destek verileri. Veriler, müşteriler hakkında işle ilgili herhangi bir bilgi içermez; örneğin, sadakat meta verileri veya kredi puanları içermez.  

Basitlik için, ETL ve veri temizleme işlemleri kapsam dışıdır, çünkü veri hazırlamanın başka bir yerde yapıldığını varsayıyoruz.

Modelleme için özellik seçimi, rasgele orman modüllerini kullanan işlemde yer alan tahminör kümesinin ön önem puanına dayanır. Machine Learning Studio'daki (klasik) uygulama için, temsili özellikler için ortalama, ortanca ve aralıkları hesapladık. Örneğin, kullanıcı etkinliği için minimum ve maksimum değerler gibi nitel veriler için toplamlar ekledik.

Ayrıca son altı ayın zamansal bilgilerini de yakaladık. Verileri bir yıl boyunca analiz ettik ve istatistiksel olarak anlamlı eğilimler olsa bile, çalkaln üzerindeki etkisinin altı ay sonra büyük ölçüde azaldığını belirledik.  

En önemli nokta, ETL, özellik seçimi ve modelleme dahil olmak üzere tüm sürecin Microsoft Azure'daki veri kaynakları kullanılarak Machine Learning Studio'da (klasik) uygulanmasıdır.   

Aşağıdaki diyagramlar kullanılan verileri göstermektedir.  

![Ham değerlerle kullanılan verilerin bir örneğini gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-4.png)

*Şekil 6: Veri kaynağından alıntı (obfuscated)*  

![Veri kaynağından çıkarılan istatistiksel özellikleri gösteren ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-5.png)

*Şekil 7: Veri kaynağından çıkarılan özellikler*
 

> Bu verilerin özel olduğunu ve bu nedenle model ve verilerin paylaşılamayacağını unutmayın.
> Ancak, herkese açık verileri kullanan benzer bir model [için, Azure AI Galerisi'ndeki](https://gallery.azure.ai/)bu örnek denemeye bakın: [Telco Customer Churn](https://gallery.azure.ai/Experiment/31c19425ee874f628c847f7e2d93e383).
> 
> Cortana Intelligence Suite'i kullanarak bir churn analiz modelini nasıl uygulayabileceğiniz hakkında daha fazla bilgi edinmek için, Kıdemli Program Yöneticisi Wee Hyong Tok'un [bu videosunu](https://info.microsoft.com/Webinar-Harness-Predictive-Customer-Churn-Model.html) da öneririz. 
> 
> 

### <a name="algorithms-used-in-the-prototype"></a>Prototipte kullanılan algoritmalar
Prototipi oluşturmak için aşağıdaki dört makine öğrenme algoritmasını kullandık (özelleştirme yok):  

1. Lojistik regresyon (LR)
2. Artırılan karar ağacı (BT)
3. Ortalama perceptron (AP)
4. Destek vektör makinesi (SVM)  

Aşağıdaki diyagram, deneme tasarım yüzeyinin modellerin oluşturulduğu sırayı gösteren bir bölümünü gösterir:  

![Stüdyo deneme tuvalküçük bir bölümünün ekran görüntüsü](./media/azure-ml-customer-churn-scenario/churn-6.png)  

*Şekil 8: Machine Learning Studio'da model oluşturma (klasik)*  

### <a name="scoring-methods"></a>Puanlama yöntemleri
Biz etiketli bir eğitim veri seti kullanarak dört model attı.  

Ayrıca sas Enterprise Miner 12 masaüstü sürümü kullanılarak inşa karşılaştırılabilir bir model puanlama dataset gönderdi. SAS modelinin ve dört Machine Learning Studio (klasik) modelinin doğruluğunu ölçtük.  

## <a name="results"></a>Sonuçlar
Bu bölümde, puanlama veri kümesine dayalı olarak modellerin doğruluğu ile ilgili bulgularımızı sıyoruz.  

### <a name="accuracy-and-precision-of-scoring"></a>Puanlamanın doğruluğu ve hassasiyeti
Genellikle, Azure Machine Learning Studio (klasik) uygulaması yaklaşık% 10-15 (Area Under Curve veya AUC) doğruluk SAS arkasındadır.  

Ancak, churn en önemli metrik yanlış sınıflandırma oranı: yani, üst N churners olarak sınıflandırıcı tarafından tahmin, bunlardan hangisi aslında çalkalama **vermedi** ve henüz özel tedavi aldı? Aşağıdaki diyagram, tüm modeller için bu yanlış sınıflandırma oranını karşılaştırır:  

![4 algoritmanın performansını karşılaştıran eğri grafiğin altındaki alan](./media/azure-ml-customer-churn-scenario/churn-7.png)

*Şekil 9: Eğri altında Passau prototip alanı*

### <a name="using-auc-to-compare-results"></a>Sonuçları karşılaştırmak için AUC kullanma
Eğri nin Altındaki Alan (AUC), pozitif ve negatif popülasyonlar için puanların dağılımları arasında küresel bir *ayrıştırılabilirlik* ölçüsünü temsil eden bir metriktir. Geleneksel Alıcı Operatör Özelliği (ROC) grafiğine benzer, ancak önemli bir fark, AUC ölçümünün bir eşik değeri seçmenizi gerektirmemesidir. Bunun yerine, **tüm** olası seçimler üzerinde sonuçları özetler. Buna karşılık, geleneksel ROC grafiği dikey eksendeki pozitif oranı ve yatay eksendeki yanlış pozitif oranı gösterir ve sınıflandırma eşiği değişir.   

AUC, modellerin AUC değerleri ile karşılaştırılmasına izin verdiği için farklı algoritmalar (veya farklı sistemler) için bir değer ölçüsü olarak kullanılır. Bu meteoroloji ve biyobilim gibi sektörlerde popüler bir yaklaşımdır. Bu nedenle, AUC sınıflandırıcı performansını değerlendirmek için popüler bir aracı temsil eder.  

### <a name="comparing-misclassification-rates"></a>Yanlış sınıflandırma oranlarının karşılaştırılması
Yaklaşık 8.000 aboneliğin CRM verilerini kullanarak söz konusu veri kümesindeki yanlış sınıflandırma oranlarını karşılaştırdık.  

* SAS yanlış sınıflandırma oranı %10-15 idi.
* Machine Learning Studio (klasik) yanlış sınıflandırma oranı ilk 200-300 churners için% 15-20 idi.  

Telekomünikasyon sektöründe, sadece onlara bir konsiyerj hizmeti veya diğer özel muamele sunarak çalkalama riski en yüksek olan müşterilere hitap etmek önemlidir. Bu bağlamda, Machine Learning Studio (klasik) uygulaması SAS modeli ile eşit sonuçlar elde eder.  

Aynı şekilde, doğruluk hassasiyetten daha önemlidir, çünkü çoğunlukla potansiyel çalkalama ları doğru bir şekilde sınıflandırmakla ilgileniyoruz.  

Vikipedi'deki aşağıdaki diyagram, canlı ve anlaşılması kolay bir grafikte ilişkiyi betimlener:  

![İki hedef. Bir hedef gevşek gruplanmış ama boğa gözü yakınında "Düşük doğruluk: iyi doğruluk, kötü hassasiyet işaretli isabet işaretleri gösterir. Başka bir hedef sıkıca gruplanmış ama uzak boğa-göz işaretlenmiş "Düşük doğruluk: kötü doğruluk, iyi hassasiyet"](./media/azure-ml-customer-churn-scenario/churn-8.png)

*Şekil 10: Doğruluk ve hassasiyet arasındaki denge*

### <a name="accuracy-and-precision-results-for-boosted-decision-tree-model"></a>Artırılmış karar ağacı modeli için doğruluk ve hassas sonuçlar
Aşağıdaki grafik, artırılmış karar ağacı modeli için Machine Learning prototipini kullanarak puanlamanın ham sonuçlarını görüntüler, bu dört model arasında en doğru olanı olur:  

![Dört algoritma için Doğruluk, Hassasiyet, Geri Çağırma, F-Skor, AUC, Ortalama Günlük Kaybı ve Eğitim Günlüğü Kaybını gösteren tablo snippet](./media/azure-ml-customer-churn-scenario/churn-9.png)

*Şekil 11: Artırılmış karar ağacı modeli özellikleri*

## <a name="performance-comparison"></a>Performans karşılaştırması
Machine Learning Studio (klasik) modelleri ve SAS Enterprise Miner 12.1 masaüstü sürümü kullanılarak oluşturulan karşılaştırılabilir bir model kullanılarak verilerin hangi hızda puanlandığını karşılaştırdık.  

Aşağıdaki tablo algoritmaların performansını özetler:  

*Tablo 1. Algoritmaların genel performansı (doğruluğu)*

| LR | BT | AP | Svm |
| --- | --- | --- | --- |
| Ortalama Model |En İyi Model |Düşük performans |Ortalama Model |

Machine Learning Studio'da (klasik) barındırılan modeller, yürütme hızı açısından SAS'ı %15-25 oranında geride tuttu, ancak doğruluk büyük ölçüde eşitti.  

## <a name="discussion-and-recommendations"></a>Tartışma ve öneriler
Telekomünikasyon sektöründe, çeşitli uygulamalar da dahil olmak üzere churn analiz ortaya çıkmıştır:  

* Dört temel kategori için ölçümler türetin:
  * **Varlık (örneğin, bir abonelik)**. Çalkalama konusu olan abonelik ve/veya müşteri hakkında temel bilgileri sağlama.
  * **Etkinlik**. Varlıkla ilgili tüm olası kullanım bilgilerini (örneğin, oturum açma sayısı) edinin.
  * **Müşteri desteği**. Aboneliğin sorunları veya müşteri desteğiyle etkileşimleri olup olmadığını belirtmek için müşteri destek günlüklerinden bilgi toplayıp.
  * **Rekabetçi ve iş verileri**. Müşteri hakkında mümkün olan tüm bilgileri edinin (örneğin, kullanılamayan veya izlenmesi zor olabilir).
* Özellik seçimini yönlendirmek için önemi kullanın. Bu, artırılan karar ağacı modelinin her zaman umut verici bir yaklaşım olduğu anlamına gelir.  

Bu dört kategorinin kullanımı, kategori başına makul etkenler üzerinde oluşturulan dizinlere dayalı basit bir *deterministik* yaklaşımın, çalkalama riski altındaki müşterileri belirlemek için yeterli olması gerektiği yanılsamasını yaratır. Ne yazık ki, bu kavram akla yatkın görünse de, yanlış bir anlayıştır. Bunun nedeni churn bir zamansal etkisi ve çalkalama katkıda bulunan faktörler genellikle geçici durumlarda olmasıdır. Bir müşterinin bugün ayrılmayı düşünmesine neden olan şey yarın farklı olabilir ve bundan altı ay sonra da kesinlikle farklı olacak. Bu nedenle, *olasılıksal* bir model bir zorunluluktur.  

Bu önemli gözlem genellikle iş, genellikle analitik bir iş zekası odaklı bir yaklaşım tercih göz ardı edilir, çoğunlukla çünkü daha kolay satmak ve basit otomasyon kabul.  

Ancak, Machine Learning Studio (klasik) kullanarak self-servis analitik vaadi, bilgi dört kategori, bölüm veya bölüm tarafından dereceli, churn hakkında makine öğrenme için değerli bir kaynak haline olmasıdır.  

Azure Machine Learning Studio'da (klasik) gelen bir diğer heyecan verici özellik de, önceden tanımlanmış modüllerin deposuna zaten mevcut olan özel bir modül ekleme yeteneğidir. Bu özellik, temelde, kitaplıkları seçmek ve dikey pazarlar için şablonlar oluşturmak için bir fırsat yaratır. Azure Machine Learning Studio'nun (klasik) pazaryerinde önemli bir ayırıcısıdır.  

Bu konuyu gelecekte de devam etmeyi umuyoruz, özellikle büyük veri analitiği ile ilgili.
  

## <a name="conclusion"></a>Sonuç
Bu makalede, genel bir çerçeve kullanarak müşteri karmaşası ortak sorunu mücadele için mantıklı bir yaklaşım açıklanmaktadır. Biz puanlama modelleri için bir prototip olarak kabul ve Azure Machine Learning Studio (klasik) kullanarak uygulamaya. Son olarak, Prototip çözümünün doğruluğunu ve performansını SAS'taki karşılaştırılabilir algoritmalarla değerlendirdik.  

 

## <a name="references"></a>Başvurular
[1] Tahmine Dayalı Analitik: Tahminlerin Ötesinde, W. McKnight, Bilgi Yönetimi, Temmuz/Ağustos 2011, s.18-20.  

[2] Vikipedi makalesi: [Doğruluk ve hassasiyet](https://en.wikipedia.org/wiki/Accuracy_and_precision)

[3] [CRISP-DM 1.0: Adım Adım Veri Madenciliği Kılavuzu](https://www.the-modeling-agency.com/crisp-dm.pdf)   

[4] [Büyük Veri Pazarlaması: Müşterilerinizi Daha Etkili Bir Şekilde Meşgul Edin ve Değeri Artırın](https://www.amazon.com/Big-Data-Marketing-Customers-Effectively/dp/1118733894/ref=sr_1_12?ie=UTF8&qid=1387541531&sr=8-12&keywords=customer+churn)

[5] [Azure AI Galerisi'nde](https://gallery.azure.ai/) [Telco churn modeli şablonu](https://gallery.azure.ai/Experiment/Telco-Customer-Churn-5) 
 

## <a name="appendix"></a>Ek
![Churn prototip bir sunum Anlık görüntü](./media/azure-ml-customer-churn-scenario/churn-10.png)

*Şekil 12: Churn prototipbir sunum Anlık görüntü*
