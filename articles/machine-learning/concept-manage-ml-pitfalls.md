---
title: AutoML ile & dengesiz verileri aşırı yakıştmaktan kaçının
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'in otomatik makine öğrenme çözümleriyle ML modellerinin yaygın tuzaklarını belirleyin ve yönetin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 04/09/2020
ms.openlocfilehash: 76f920ad6aae68defb567a7a6623d1ffd488af5f
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80874866"
---
# <a name="prevent-overfitting-and-imbalanced-data-with-automated-machine-learning"></a>Otomatik makine öğrenimi ile aşırı montaj ve dengesiz verileri önleme

Makine öğrenme modelleri oluştururken aşırı montaj ve dengesiz veriler yaygın tuzaklar vardır. Varsayılan olarak, Azure Machine Learning'in otomatik makine öğrenimi, bu riskleri belirlemenize yardımcı olacak grafikler ve ölçümler sağlar ve bunları azaltmaya yardımcı olmak için en iyi uygulamaları uygular. 

## <a name="identify-over-fitting"></a>Aşırı montajı tanımlayın

Makine öğrenimine aşırı uyum, bir model eğitim verilerine çok iyi uyduğunda ortaya çıkar ve sonuç olarak görünmeyen test verileri üzerinde doğru bir şekilde tahmin edemez. Başka bir deyişle, model sadece eğitim verilerinde belirli desenleri ve gürültü ezberledi, ancak gerçek veriler üzerinde tahminler yapmak için yeterince esnek değildir.

Aşağıdaki eğitimli modelleri ve bunların ilgili tren ve test doğrularını göz önünde bulundurun.

| Model | Tren doğruluğu | Test doğruluğu |
|-------|----------------|---------------|
| A | %99,9 | 95% |
| B | 87% | 87% |
| C | %99,9 | %45 |

**Model A**göz önüne alındığında, görünmeyen verilerüzerinde test doğruluğu eğitim doğruluğu daha düşük ise, model aşırı takılı olduğu yaygın bir yanılgı vardır. Ancak, test doğruluğu her zaman eğitim doğruluğundan daha az olmalıdır ve aşırı uyum ve uygun uyum ayrımı *ne kadar* daha az doğru aşağı gelir. 

**A** ve **B**modellerini karşılaştırırken, a **modeli** daha iyi bir modeldir çünkü daha yüksek test doğruluğuna sahiptir ve test doğruluğu %95'te biraz daha düşük olmasına rağmen, aşırı uyumun mevcut olduğunu düşündüren önemli bir fark değildir. Tren ve test doğrulukları birbirine daha yakın olduğu için **B** modelini seçmezsiniz.

Model **C** aşırı montaj net bir durumda temsil eder; eğitim doğruluğu çok yüksektir ancak test doğruluğu bu kadar yüksek değildir. Bu ayrım özneldir, ancak sorun ve verilerinizin bilgisinden gelir ve hangi hata büyüklükleri kabul edilebilir.

## <a name="prevent-over-fitting"></a>Aşırı montajı önleyin

En korkunç durumlarda, aşırı takılan bir model, eğitim sırasında görülen özellik değeri kombinasyonlarının her zaman hedef için aynı çıktıyı ortaya çıkaracağını varsayar.

Aşırı montajı önlemenin en iyi yolu, aşağıdakiler de dahil olmak üzere ML'nin en iyi uygulamalarını takip etmektir:

* Daha fazla eğitim verisi kullanma ve istatistiksel önyargıyı ortadan kaldırma
* Hedef sızıntısını önleme
* Daha az özellik kullanma
* **Düzenlileştirme ve hiperparametre optimizasyonu**
* **Model karmaşıklığı sınırlamaları**
* **Çapraz doğrulama**

Otomatik ML bağlamında, yukarıdaki ilk üç öğe **uyguladığınız en iyi uygulamalardır.** Son üç cesur öğeleri aşırı uydurma karşı korumak için varsayılan olarak **en iyi uygulamalar otomatik ML uygular.** Otomatik ML dışındaki ortamlarda, en iyi altı uygulamanın tümü, aşırı montaj lı modelleri önlemek için aşağıdaki leri izlemeye değerdir.

### <a name="best-practices-you-implement"></a>Uyguladığınız en iyi uygulamalar

**Daha fazla veri** kullanmak, aşırı uyumu önlemenin en basit ve en iyi yoludur ve ek bir bonus genellikle doğruluğu artırır. Daha fazla veri kullandığınızda, modelin tam desenleri ezberleması zorlaşır ve daha fazla koşulu karşılamak için daha esnek çözümlere ulaşmak zorunda kalır. Ayrıca, eğitim verilerinizin canlı tahmin verilerinde bulunmayan yalıtılmış desenler içermediğinden emin olmak için **istatistiksel önyargıyı**tanımak da önemlidir. Bu senaryoyu çözmek zor olabilir, çünkü tren ve test kümeleriniz arasında aşırı uyum olmayabilir, ancak canlı test verileriyle karşılaştırıldığında aşırı uyum sağlayan bir durum olabilir.

Hedef sızıntısı, tren/test kümeleri arasında aşırı uyum göremeyebilirsiniz, ancak daha ziyade tahmin-zaman görünür benzer bir konudur. Hedef sızıntısı, modelinizin normalde tahmin zamanında olmaması gereken verilere erişerek eğitim sırasında "hile yaptığında" oluşur. Örneğin, sorununuz Pazartesi günü bir emtia fiyatının Cuma günü ne olacağını tahmin etmekse, ancak özelliklerinizden biri yanlışlıkla Perşembe günlerinden gelen verileri içeriyorsa, bu modelin gelecekte göremeyeceği için tahmin zamanında sahip olamayacağı veriler olacaktır. Hedef sızıntısı kaçırmak kolay bir hatadır, ancak genellikle sorun için anormal yüksek doğruluk ile karakterizedir. Hisse senedi fiyatını tahmin etmeye çalışıyorsanız ve bir modeli %95 doğrulukla eğitiyorsanız, özelliklerinizde bir yerlerde büyük olasılıkla hedef sızıntısı olabilir.

Özelliklerikaldırmak, modelin belirli desenleri ezberlemek için kullanılacak çok fazla alana sahip olmasını engelleyerek aşırı uyuma yardımcı olabilir ve böylece daha esnek olmasına neden olabilir. Nicel olarak ölçmek zor olabilir, ancak özellikleri kaldırabilir ve aynı doğruluğu koruyabilirseniz, modeli daha esnek hale getirmiştir ve aşırı uyum riskini azaltmış olur.

### <a name="best-practices-automated-ml-implements"></a>En iyi uygulamalar otomatik ML uygular

Düzenlileştirme, karmaşık ve aşırı takılı modelleri cezalandırmak için bir maliyet işlevini en aza indirme işlemidir. Farklı türde düzenlileştirme işlevleri vardır, ancak genel olarak hepsi model katsayısı boyutunu, varyansını ve karmaşıklığı cezalandırır. Otomatik ML, aşırı uyumu kontrol eden farklı model hiperparametre ayarlarına sahip farklı kombinasyonlarda L1 (Kement), L2 (Sırt) ve ElasticNet (L1 ve L2) kullanır. Basit bir ifadeyle, otomatik ML bir model düzenlenir ne kadar değişir ve en iyi sonucu seçin.

Otomatik ML ayrıca aşırı montajı önlemek için açık model karmaşıklığı sınırlamaları da uygular. Çoğu durumda bu uygulama, tek tek ağaç maksimum derinliğinin sınırlı olduğu ve orman veya topluluk tekniklerinde kullanılan toplam ağaç sayısının sınırlı olduğu karar ağacı veya orman algoritmaları için özeldir.

Çapraz doğrulama (CV), tam eğitim verilerinizin birçok alt kümesini alma ve her alt kümede bir model yetiştirme işlemidir. Fikir bir model "şanslı" alabilir ve bir alt kümesi ile büyük doğruluk var, ama birçok alt setleri kullanarak model bu yüksek doğruluk her zaman elde olmaz. CV yaparken, bir doğrulama tutma dataset sağlar, CV kıvrımları (alt küme sayısı) belirtin ve otomatik ML modelinizi eğitecek ve doğrulama setinizdeki hatayı en aza indirmek için hiperparametreleri ayarlarsınız. Bir CV kıvrımı aşırı formda olabilir, ancak birçoğunu kullanarak son modelinizin aşırı formda olma olasılığını azaltır. Bunun karşılığında CV daha uzun eğitim süreleri ve dolayısıyla daha yüksek maliyetle sonuçlanır, çünkü bir modeli bir kez eğitmek yerine, her *n* CV alt kümeleri için bir kez eğitebilirsiniz. 

> [!NOTE]
> Çapraz doğrulama varsayılan olarak etkinleştirilir; otomatik ML ayarlarında yapılandırılmalıdır. Ancak, çapraz doğrulama yapılandırıldıktan ve bir doğrulama veri kümesi sağlandıktan sonra, işlem sizin için otomatikleştirilir. Bkz. 

<a name="imbalance"></a>

## <a name="identify-models-with-imbalanced-data"></a>Dengesiz verilerle modelleri belirleme

Dengesiz veriler genellikle makine öğrenimi sınıflandırma senaryoları için verilerde bulunur ve her sınıftaki gözlemlerin orantısız bir oranını içeren verileri ifade eder. Bu dengesizlik bir modelin doğruluğunun yanlış algılanan olumlu bir etkisine yol açabilir, çünkü giriş verileri bir sınıfa karşı önyargıya sahiptir, bu da eğitimli modelin bu önyargıyı taklit etmesiyle sonuçlanır. 

Sınıflandırma algoritmaları genellikle doğruluk la değerlendirildiğinden, bir modelin doğruluk puanını kontrol etmek, dengesiz verilerden etkilenip etkilenmediğini belirlemenin iyi bir yoludur. Gerçekten yüksek doğruluk ya da bazı sınıflar için gerçekten düşük doğruluk var mıydı?

Buna ek olarak, otomatik ML çalıştırmaları, modelinizin sınıflandırmalarının doğruluğunu anlamanıza ve dengesiz verilerden etkilenecek modelleri belirlemenize yardımcı olabilecek aşağıdaki grafikleri otomatik olarak oluşturur.

Grafik| Açıklama
---|---
[Karışıklık Matrisi](how-to-understand-automated-ml.md#confusion-matrix)| Doğru sınıflandırılan etiketleri verilerin gerçek etiketlerine göre değerlendirir. 
[Hassas geri çağırma](how-to-understand-automated-ml.md#precision-recall-chart)| Doğru etiketlerin verilerin bulunan etiket örneklerinin oranına oranını değerlendirir 
[ROC Eğrileri](how-to-understand-automated-ml.md#roc)| Doğru etiketlerin yanlış pozitif etiketlerin oranına oranını değerlendirir.

## <a name="handle-imbalanced-data"></a>Dengesiz verileri işleme 

Makine öğrenimi iş akışını basitleştirme amacının bir parçası olarak, otomatik ML, 

- **Ağırlık sütunu:** otomatik ML, ağırlıklı bir sütunu giriş olarak destekler ve verilerdeki satırların yukarı veya aşağı ağırlıklandırılmasına neden olur ve bu da sınıfı daha fazla veya daha az "önemli" hale getirebilir.

- Otomatik ML tarafından kullanılan algoritmalar düzgün 20:1,, en yaygın sınıf en az ortak sınıf daha veri 20 kat daha fazla satır olabilir anlamına dengesizlik işleyebilir.

Aşağıdaki teknikler, dengesiz verileri otomatik ML dışında işlemek için ek seçeneklerdir. 

- Küçük sınıfları yukarı ya da daha büyük sınıfları aşağı örnekleme yaparak sınıf dengesizliğine bile yeniden örnekleme. Bu yöntemler işlemek ve analiz etmek için uzmanlık gerektirir.

- Dengesiz verilerle daha iyi ilgilenen bir performans ölçümü kullanın. Örneğin, F1 puanı ağırlıklı bir kesinlik ve hatırlama ortalamasıdır. Hassas bir sınıflandırıcının kesinliğini ölçer, düşük hassasiyet yüksek sayıda yanlış pozitifi gösterir--, geri çağırma ise bir sınıfın tamlığını ölçer, düşük geri çağırma da yüksek sayıda yanlış negatifi gösterir. 

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik makine öğrenimini kullanarak nasıl model oluşturup nasıl oluşturup nasıl oluşturup öğrenebilirsiniz:

+ [Öğreticiyi izleyin: Azure Machine Learning ile bir regresyon modelini otomatik olarak eğitin](tutorial-auto-train-models.md)

+ Otomatik eğitim denemesi için ayarları yapılandırın:
  + Azure Machine Learning stüdyosunda [şu adımları kullanın.](how-to-use-automated-ml-for-ml-models.md)
  + Python SDK ile [şu adımları kullanın.](how-to-configure-auto-train.md)


