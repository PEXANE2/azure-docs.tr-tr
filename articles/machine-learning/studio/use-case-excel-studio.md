---
title: Excel'den analiz geçirme
titleSuffix: ML Studio (classic) - Azure
description: Excel'de ve Azure Machine Learning Studio'da (klasik) doğrusal regresyon modellerinin karşılaştırılması
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/20/2017
ms.openlocfilehash: 85bae9bfc10460b51935c6eb1e14e3a3dd816a8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217805"
---
# <a name="migrate-analytics-from-excel-to-azure-machine-learning-studio-classic"></a>Analitiği Excel'den Azure Machine Learning Studio'ya geçirin (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

> *Kate Baroni* ve *Ben Boatman,* Microsoft'un Veri Öngörüleri Mükemmellik Merkezi'nde kurumsal çözüm mimarlarıdır. Bu makalede, mevcut bir regresyon çözümleme paketini Azure Machine Learning Studio (klasik) kullanarak bulut tabanlı bir çözüme geçirme deneyimlerini açıklarlar.

## <a name="goal"></a>Hedef

Projemiz iki hedef göz önünde bulundurularak başladı: 

1. Kuruluşumuzun aylık gelir tahminlerinin doğruluğunu artırmak için tahmine dayalı analizlerden yararlanın 
2. Sonuçlarımızı onaylamak, optimize etmek, artırmak ve ölçeklendirmek için Azure Machine Learning Studio'yı (klasik) kullanın. 

Birçok işletme gibi, kuruluşumuz da aylık gelir tahmin sürecinden geçerek devam eder. Küçük işletme analistleri ekibimiz, süreci desteklemek ve tahmin doğruluğunu iyileştirmek için Azure Machine Learning Studio'yu (klasik) kullanmakla görevlendirildi. Ekip, birden çok kaynaktan veri toplamak ve hizmet satış tahminiyle ilgili temel öznitelikleri tanımlayan istatistiksel analiz yoluyla veri özniteliklerini çalıştırmak için birkaç ay harcadı. Bir sonraki adım, Excel'deki veriler üzerinde istatistiksel regresyon modellerini prototiplemeye başlamaktı. Birkaç hafta içinde, mevcut alan ve finans tahmin süreçlerini geride eden bir Excel regresyon modelimiz vardı. Bu temel tahmin sonucu oldu. 

Daha sonra, Studio'nun (klasik) öngörülü performansı nasıl geliştirebileceğini öğrenmek için öngörülü analitiğimizi Studio'ya (klasik) taşımak için bir sonraki adımı attık.

## <a name="achieving-predictive-performance-parity"></a>Tahmine dayalı performans eşitliği elde etme
İlk önceliğimiz Studio (klasik) ve Excel regresyon modelleri arasında parite elde etmekti. Aynı veriler ve eğitim ve test verileri için aynı bölünme göz önüne alındığında, Excel ve Studio (klasik) arasında tahmine dayalı performans paritesi elde etmek istedik. Başlangıçta başarısız olduk. Excel modeli Studio (klasik) modelini geride yaptı. Başarısızlık Studio (klasik) temel araç ayarı anlayış eksikliği nedeniyle oldu. Studio (klasik) ürün ekibi yle senkronize olduktan sonra, veri kümelerimiz için gerekli olan temel ayarı daha iyi anladık ve iki model arasında parite elde ettik. 

### <a name="create-regression-model-in-excel"></a>Excel'de regresyon modeli oluşturma
Excel Regresyon'umuz Excel Çözümleme AracıPak'ta bulunan standart doğrusal regresyon modelini kullanılmıştır. 

Ortalama *% Hata'yı* hesapladık ve modelin performans ölçütü olarak kullandık. Excel'i kullanarak çalışan bir modele ulaşmak 3 ay sürdü. Öğrenmenin büyük bir kısmını Studio (klasik) deneye getirdik ve bu deney sonunda gereksinimleri anlamada yararlı oldu.

### <a name="create-comparable-experiment-in-studio-classic"></a>Studio'da karşılaştırılabilir deney oluşturun (klasik)
Studio'daki deneyimimizi oluşturmak için bu adımları izledik (klasik): 

1. Studio (klasik) (çok küçük dosya) bir csv dosyası olarak veri seti yüklendi
2. Yeni bir deneme oluşturdu ve Excel'de kullanılan aynı veri özelliklerini seçmek için [Veri Kümesi modülündeki Sütunları Seç'i][select-columns] kullandı 
3. Verileri Excel'de olduğu gibi aynı eğitim veri kümelerine bölmek için [Split Data][split] modüllerini *(Göreli İfade* modu ile) kullandı 
4. [Doğrusal Regresyon][linear-regression] modülü (yalnızca varsayılan seçenekler) ile denendi, belgelendi ve sonuçları Excel regresyon modelimizile karşılaştırdı

### <a name="review-initial-results"></a>İlk sonuçları gözden geçirme
İlk başta, Excel modeli açıkça Studio (klasik) modeli geride: 

|  | Excel | Studio (klasik) |
| --- |:---:|:---:|
| Performans | | |
| <ul style="list-style-type: none;"><li>Ayarlanmış R Karesi</li></ul> |0.96 |Yok |
| <ul style="list-style-type: none;"><li>Katsayısı <br />Belirlenmesi</li></ul> |Yok |0,78<br />(düşük doğruluk) |
| Ortalama Mutlak Hata |9.5 Milyon Dolar |19,4 Milyon Dolar |
| Ortalama Mutlak Hata (%) |6.03% |12.2% |

Machine Learning ekibindeki geliştiriciler ve veri bilimciler tarafından süreç ve sonuçlarımızı çalıştırdığımızda, hızlı bir şekilde bazı yararlı ipuçları sağladılar. 

* Studio'da (klasik) [Lineer Regresyon][linear-regression] modüllerini kullandığınızda iki yöntem sağlanır:
  * Online Gradyan İniş: Daha büyük ölçekli sorunlar için daha uygun olabilir
  * Sıradan En Küçük Kareler: Bu yöntem çoğu insan doğrusal regresyon duyunca düşünüyorum. Küçük veri kümeleri için Sıradan En Küçük Kareler daha uygun bir seçim olabilir.
* Performansı artırmak için L2 Düzenlileştirme Ağırlık parametresini ayarlamayı düşünün. Varsayılan olarak 0,001 olarak ayarlanır, ancak küçük veri setimiz için performansı artırmak için 0,005 olarak ayarlıyoruz. 

### <a name="mystery-solved"></a>Gizem çözüldü!
Önerileri uyguladığımız zaman, Studio'da (klasik) Excel ile aynı temel performansı elde ettik: 

|  | Excel | Stüdyo (klasik) (İlk) | Stüdyo (klasik) w / En Az Kareler |
| --- |:---:|:---:|:---:|
| Etiketli değer |Fiili (sayısal) |Aynı |Aynı |
| Öğrenci |Excel -> Veri Analizi -> Regresyon |Doğrusal Regresyon. |Çizgisel Regresyon |
| Öğrenci seçenekleri |Yok |Varsayılanları |sıradan en az kareler<br />L2 = 0,005 |
| Veri Kümesi |26 satır, 3 özellik, 1 etiket. Hepsi sayısal. |Aynı |Aynı |
| Split: Tren |Excel ilk 18 satırda eğitildi, son 8 satırda test edildi. |Aynı |Aynı |
| Split: Test |Son 8 satıra uygulanan Excel regresyon formülü |Aynı |Aynı |
| **Performans** | | | |
| Ayarlanmış R Karesi |0.96 |Yok | |
| Tespit Katsayısı |Yok |0,78 |0.952049 |
| Ortalama Mutlak Hata |9.5 Milyon Dolar |19,4 Milyon Dolar |9.5 Milyon Dolar |
| Ortalama Mutlak Hata (%) |<span style="background-color: 00FF00;">6.03%</span> |12.2% |<span style="background-color: 00FF00;">6.03%</span> |

Buna ek olarak, Excel katsayıları Azure eğitimli modeldeki özellik ağırlıkları ile iyi karşılaştırıldı:

|  | Excel Katsayıları | Azure Özellik Ağırlıkları |
| --- |:---:|:---:|
| Durdurma / Önyargı |19470209.88 |19328500 |
| Özellik A |0.832653063 |0.834156 |
| Özellik B |11071967.08 |11007300 |
| Özellik C |25383318.09 |25140800 |

## <a name="next-steps"></a>Sonraki Adımlar
Excel içinde Machine Learning web hizmetini tüketmek istedik. İş analistlerimiz Excel'e güveniyor ve Machine Learning web hizmetini bir dizi Excel verisiyle aramanın ve öngörülen değeri Excel'e döndürmenin bir yolunu bulabilmek için bir yola ihtiyacımız vardı. 

Studio'da (klasik) mevcut olan seçenekleri ve algoritmaları kullanarak modelimizi de optimize etmek istedik.

### <a name="integration-with-excel"></a>Excel ile tümleştirme
Çözümümüz, eğitimli modelden bir web hizmeti oluşturarak Machine Learning regresyon modelimizi operasyonel hale getirmekti. Birkaç dakika içinde, web hizmeti oluşturuldu ve tahmin edilen bir gelir değerini döndürmek için doğrudan Excel'den çağırabiliriz. 

*Web Hizmetleri Panosu* bölümünde indirilebilir bir Excel çalışma kitabı içerir. Çalışma kitabı web hizmeti API ve şema bilgileri gömülü önceden biçimlendirilmiş gelir. *Excel Çalışma Kitabını İndir'i*tıklattığınızda, çalışma kitabı açılır ve yerel bilgisayarınıza kaydedebilirsiniz. 

![Web Hizmetleri Panosundan Excel çalışma kitabını indirin](./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-1.png)

Çalışma kitabı açıkken, önceden tanımlanmış parametrelerinizi aşağıda gösterildiği gibi mavi Parametre bölümüne kopyalayın. Parametreler girildikten sonra, Excel Machine Learning web hizmetine seslenir ve öngörülen puanlı etiketler yeşil Tahmin Edilen Değerler bölümünde görüntülenir. Çalışma kitabı, Parametreler altında girilen tüm satır öğeleri için eğitilmiş modelinizi temel alan parametreler için öngörüler oluşturmaya devam eder. Bu özelliğin nasıl kullanılacağı hakkında daha fazla bilgi için [Excel'den Azure Machine Learning Web Hizmeti Tüketme'ye](consuming-from-excel.md)bakın. 

![Dağıtılan web hizmetine bağlanan Şablon Excel çalışma kitabı](./media/linear-regression-in-azure/machine-learning-linear-regression-in-azure-2.png)

### <a name="optimization-and-further-experiments"></a>Optimizasyon ve diğer deneyler
Artık Excel modelimizle bir temele sahip olduğumuza göre, Machine Learning Lineer Regresyon Modelimizi optimize etmek için ileriye taşındık. İlk veri öğelerini seçmemizi geliştirmek için [Filtre Tabanlı Özellik Seçimi][filter-based-feature-selection] modülünü kullandık ve bu da %4,6 Ortalama Mutlak Hata performansı geliştirmemize yardımcı oldu. Gelecekteki projeler için, modelleme için kullanılacak doğru özellik kümesini bulmak için veri öznitelikleri aracılığıyla bizi haftalarca kaydedebilecek bu özelliği kullanacağız. 

Daha sonra performansı karşılaştırmak için denememize [Bayesian][bayesian-linear-regression] veya [Boosted Decision Trees][boosted-decision-tree-regression] gibi ek algoritmalar eklemeyi planlıyoruz. 

Regresyon ile deneme yapmak istiyorsanız, denemek için iyi bir veri kümesi, sayısal öznitelikleri çok olan Enerji Verimliliği Regresyon örnek veri kümesivardır. Veri kümesi Studio'daki örnek veri kümelerinin bir parçası olarak sağlanır (klasik). Isıtma Yükü veya Soğutma Yükü tahmin etmek için çeşitli öğrenme modülleri kullanabilirsiniz. Aşağıdaki grafik, hedef değişken Soğutma Yükü için tahmin Enerji Verimliliği veri seti karşı öğrenir farklı regresyon bir performans karşılaştırması: 

| Model | Ortalama Mutlak Hata | Kök Ortalama Kareli Hata | Göreli Mutlak Hata | Göreli Kareli Hata | Tespit Katsayısı |
| --- | --- | --- | --- | --- | --- |
| Artırılmış Karar Ağacı |0.930113 |1.4239 |0.106647 |0.021662 |0.978338 |
| Lineer Regresyon (Gradyan İniş) |2.035693 |2.98006 |0.233414 |0.094881 |0.905119 |
| Sinir Ağı Regresyonu |1.548195 |2.114617 |0.177517 |0.047774 |0.952226 |
| Doğrusal Regresyon (Sıradan En Az Kareler) |1.428273 |1.984461 |0.163767 |0.042074 |0.957926 |

## <a name="key-takeaways"></a>Anahtar Paketler
Excel regresyon ve Studio (klasik) deneylerini paralel olarak çalıştırarak çok şey öğrendik. Excel'de temel modeli oluşturmak ve Machine Learning [Lineer Regresyon][linear-regression] kullanarak modellerile karşılaştırmak Studio'yu (klasik) öğrenmemize yardımcı oldu ve veri seçimi ve model performansını artırmak için fırsatlar keşfettik. 

Ayrıca, gelecekteki tahmin projelerini hızlandırmak için [Filtre Tabanlı Özellik Seçimi'ni][filter-based-feature-selection] kullanmanın tavsiye olduğunu da bulduk. Verilerinize özellik seçimi uygulayarak, Studio'da (klasik) daha iyi genel performansla geliştirilmiş bir model oluşturabilirsiniz. 

Öngörülü analitik tahminleri Studio'dan (klasik) Sistematik olarak Excel'e aktarabilme özelliği, geniş bir iş kullanıcı kitlesine başarılı bir şekilde sonuç sağlama olanağında önemli bir artışsağlar. 

## <a name="resources"></a>Kaynaklar
Regresyon ile çalışmanıza yardımcı olacak bazı kaynaklar şunlardır: 

* Excel'de gerileme. Excel'de hiç gerilemeden hiç denemediyseniz, bu öğretici bunu kolaylaştırır:[https://www.excel-easy.com/examples/regression.html](https://www.excel-easy.com/examples/regression.html)
* Regresyon vs tahmin. Tyler Chessman doğrusal regresyon iyi bir acemi açıklaması içeren Excel, zaman serisi tahmin yapmak açıklayan bir blog makalesi yazdı. [https://www.itprotoday.com/sql-server/understanding-time-series-forecasting-concepts](https://www.itprotoday.com/sql-server/understanding-time-series-forecasting-concepts) 
* Sıradan En Az Kareler Lineer Regresyon: Kusurları, Sorunları ve Tuzaklar. Regresyon'un tanıtımı ve tartışması için: [ https://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/](https://www.clockbackward.com/2009/06/18/ordinary-least-squares-linear-regression-flaws-problems-and-pitfalls/)

<!-- Module References -->
[bayesian-linear-regression]: https://msdn.microsoft.com/library/azure/ee12de50-2b34-4145-aec0-23e0485da308/
[boosted-decision-tree-regression]: https://msdn.microsoft.com/library/azure/0207d252-6c41-4c77-84c3-73bdf1ac5960/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

