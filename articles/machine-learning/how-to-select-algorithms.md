---
title: Makine öğrenimi algoritması nasıl seçilir?
titleSuffix: Azure Machine Learning
description: Kümeleme, sınıflandırma veya regresyon denemelerinde denetimli ve denetimsiz öğrenme için Azure Machine Learning algoritmaları nasıl seçilir?
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: FrancescaLazzeri
ms.author: lazzeri
ms.reviewer: cgronlun
ms.date: 03/05/2020
ms.openlocfilehash: e0482bac9569a834adf3e1cdef2b3f702980eac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78328672"
---
# <a name="how-to-select-algorithms-for-azure-machine-learning"></a>Azure Machine Learning için algoritmalar nasıl seçilir?

Yaygın bir soru "Hangi makine öğrenimi algoritması kullanmalıyım?" Seçtiğiniz algoritma öncelikle veri bilimi senaryonuzun iki farklı yönüne bağlıdır:

 - **Verilerinizle ne yapmak istiyorsunuz?** Özellikle, geçmiş verilerinizden öğrenerek yanıtlamak istediğiniz iş sorusu nedir?

 - **Veri bilimi senaryonuzun gereksinimleri nelerdir?** Özellikle, çözümünüzün desteklediği özelliklerin doğruluğu, eğitim süresi, doğrusallığı, parametre sayısı ve sayısı nedir?

 ![Algoritma seçiminde dikkat edilmesi gerekenler: Ne bilmek istiyorsunuz? Senaryo gereksinimleri nelerdir?](./media/how-to-select-algorithms/how-to-select-algorithms.png)

## <a name="business-scenarios-and-the-machine-learning-algorithm-cheat-sheet"></a>İş senaryoları ve Machine Learning Algoritma hile sayfası

[Azure Machine Learning Algoritması Hile Sayfası](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri) ilk göz önünde bulundurularak size yardımcı olur: **Verilerinizle ne yapmak istiyorsunuz?** Machine Learning Algoritmahile Sayfasında, yapmak istediğiniz görevi arayın ve ardından tahmine dayalı analitik çözümü için bir [Azure Machine Learning tasarımcı](https://docs.microsoft.com/azure/machine-learning/concept-designer?WT.mc_id=docs-article-lazzeri) algoritması bulun. 

Makine Öğrenme tasarımcısı [multiclass Karar Orman,](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-decision-forest?WT.mc_id=docs-article-lazzeri) [Tavsiye sistemleri,](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-recommender?WT.mc_id=docs-article-lazzeri) [Nöral Ağ Regresyon,](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/neural-network-regression?WT.mc_id=docs-article-lazzeri) [Multiclass Nöral Ağ](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-neural-network?WT.mc_id=docs-article-lazzeri)ve [K-Means Kümeleme](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/k-means-clustering?WT.mc_id=docs-article-lazzeri)gibi algoritmalar, kapsamlı bir portföy sağlar. Her algoritma makine öğrenme sorunu farklı bir tür ele almak için tasarlanmıştır. Her algoritmanın nasıl çalıştığı ve algoritmayı optimize etmek için parametrelerin nasıl ayarlanış ları ile ilgili belgelerle birlikte tam bir liste için [Makine Öğrenimi tasarımcısı algoritması](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri) ve modül başvurusuna bakın.

> [!NOTE]
> Makine öğrenme algoritması hile sayfasını indirmek için [Azure Machine öğrenme algoritması hile sayfasına](https://docs.microsoft.com/azure/machine-learning/algorithm-cheat-sheet?WT.mc_id=docs-article-lazzeri)gidin.
> 
> 

Azure Machine Learning Algoritmahile Sayfasındaki rehberliğin yanı sıra, çözümünüz için bir makine öğrenme algoritması seçerken diğer gereksinimleri de aklınızda bulundurun. Doğruluk, eğitim süresi, doğrusallık, parametre sayısı ve özellik sayısı gibi göz önünde bulundurulması gereken ek faktörler şunlardır.

## <a name="additional-requirements-for-a-data-science-scenario"></a>Veri bilimi senaryosu için ek gereksinimler

Verilerinizle ne yapmak istediğinizi anladıktan sonra, çözümünüz için ek gereksinimler belirlemeniz gerekir. 

Aşağıdaki gereksinimler için seçimler yapın ve muhtemelen değiş tokuş yapın:

- Doğru -luk
- Eğitim süresi
- Doğrusallık
- Parametre sayısı
- Özellik sayısı

## <a name="accuracy"></a>Doğru -luk

Makine öğreniminde doğruluk, bir modelin etkinliğini, doğru sonuçların toplam olgulara oranı olarak ölçer. Machine Learning tasarımcısında, [Model'i Değerlendir modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri) endüstri standardı değerlendirme ölçümleri kümesini hesaplar. Bu modülü, eğitimli bir modelin doğruluğunu ölçmek için kullanabilirsiniz.

Mümkün olan en doğru yanıtı almak her zaman gerekli değildir. Bazen bir yaklaşım, ne için kullanmak istediğinize bağlı olarak yeterlidir. Bu durumda, daha yaklaşık yöntemlerle yapıştırarak işlem sürenizi önemli ölçüde azaltabilirsiniz. Yaklaşık yöntemler de doğal olarak aşırı yakışmaktan kaçınma eğilimindedir.

Modeli Değerlendir modülünü kullanmanın üç yolu vardır:

- Modeli değerlendirmek için eğitim verileriniz üzerinden puanlar oluşturun
- Modelde puanlar oluşturun, ancak bu puanları ayrılmış bir test kümesindeki puanlarla karşılaştırın
- Aynı veri kümesini kullanarak iki farklı ama ilgili modelin puanlarını karşılaştırın

Makine öğrenimi modellerinin doğruluğunu değerlendirmek için kullanabileceğiniz ölçümlerin ve yaklaşımların tam listesi için [bkz.](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/evaluate-model?WT.mc_id=docs-article-lazzeri)

## <a name="training-time"></a>Eğitim süresi

Denetimli öğrenmede eğitim, hataları en aza indiren bir makine öğrenimi modeli oluşturmak için geçmiş verileri kullanmak anlamına gelir. Bir modeli eğitmek için gereken dakika veya saat sayısı algoritmalar arasında büyük bir değişiklik gösterir. Eğitim süresi genellikle doğrulukla yakından bağlıdır; biri genellikle diğerine eşlik eder. 

Buna ek olarak, bazı algoritmalar veri noktalarının sayısına diğerlerinden daha duyarlıdır. Belirli bir algoritmayı seçebilirsiniz, çünkü bir zaman sınırlamanız varsa, özellikle de veri kümesi büyükse.

Machine Learning tasarımcısında, makine öğrenimi modeli oluşturmak ve kullanmak genellikle üç aşamalı bir süreçtir:

1.  Belirli bir algoritma türünü seçerek ve parametrelerini veya hiperparametrelerini tanımlayarak bir modeli yapılandırın. 

2.  Etiketli ve algoritmaile uyumlu veri kümesi sağlayın. Hem verileri hem de modeli [Train Model modülüne](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/train-model?WT.mc_id=docs-article-lazzeri)bağlayın.

3.  Eğitim tamamlandıktan sonra, yeni veriler üzerinde tahminlerde bulunmak için [puanlama modüllerinden](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/score-model?WT.mc_id=docs-article-lazzeri) biriyle birlikte eğitilmiş modeli kullanın.

## <a name="linearity"></a>Doğrusallık

İstatistiklerde ve makine öğreniminde doğrusallık, veri setinizde bir değişken ile sabit arasında doğrusal bir ilişki olduğu anlamına gelir. Örneğin, doğrusal sınıflandırma algoritmaları sınıfların düz bir çizgi (veya daha yüksek boyutlu analog) ile ayrılabileceğini varsayar.

Birçok makine öğrenme algoritması doğrusallılılılılılılıktan yararlanıyor. Azure Machine Learning tasarımcısında şunları içerir: 

- [Çok sınıflı lojistik regresyon](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/multiclass-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [İki sınıflı lojistik regresyon](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-logistic-regression?WT.mc_id=docs-article-lazzeri)
- [Destek vektör makineleri](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri)  

Doğrusal regresyon algoritmaları, veri eğilimlerinin düz bir çizgi izlediğini varsayar. Bu varsayım bazı sorunlar için kötü değildir, ancak diğerleri için doğruluğu azaltır. Sakıncalarına rağmen, doğrusal algoritmalar ilk strateji olarak popülerdir. Onlar algoritmik basit ve hızlı tren eğilimindedir.

![Doğrusal olmayan sınıf sınırı](./media/how-to-select-algorithms/nonlinear-class-boundary.png)

***Doğrusal olmayan sınıf sınırı***: *Doğrusal sınıflandırma algoritması güvenmek düşük doğrulukla sonuçlanır.*

![Doğrusal olmayan bir eğilime sahip veriler](./media/how-to-select-algorithms/nonlinear-trend.png)

***Doğrusal olmayan bir eğilime sahip veriler***: *Doğrusal regresyon yöntemi kullanmak gerekenden çok daha büyük hatalar oluşturur.*

## <a name="number-of-parameters"></a>Parametre sayısı

Parametreler, bir veri bilimcinin algoritma kurarken çevirebilmek için aldığı düğümlerdir. Bunlar, hata toleransı veya yineleme sayısı gibi algoritmanın davranışını etkileyen sayılar veya algoritmanın nasıl davranacağının varyantları arasındaki seçeneklerdir. Algoritmanın eğitim süresi ve doğruluğu bazen doğru ayarları elde etmeye karşı hassas olabilir. Genellikle, çok sayıda parametreye sahip algoritmalar iyi bir kombinasyon bulmak için en çok deneme yanılma gerektirir.

Alternatif olarak, Machine Learning tasarımcısında [Tune Model Hiperparametre modülü](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/tune-model-hyperparameters?WT.mc_id=docs-article-lazzeri) vardır: Bu modülün amacı, bir makine öğrenme modeli için optimum hiperparametreleri belirlemektir. Modül, farklı ayar kombinasyonlarını kullanarak birden çok modeli oluşturur ve sınar. Ayarların birleşimlerini elde etmek için tüm modellerdeki ölçümleri karşılaştırır. 

Bu, parametre alanını kapsadığından emin olmak için harika bir yol olsa da, bir modeli eğitmek için gereken süre parametre sayısıyla katlanarak artar. İyi tarafı, birçok parametreye sahip olmak genellikle bir algoritmanın daha fazla esnekliğe sahip olduğunu gösterir. Parametre ayarlarının doğru kombinasyonunu bulmanız koşuluyla, genellikle çok iyi bir doğruluk elde edebilirsiniz.

## <a name="number-of-features"></a>Özellik sayısı

Makine öğreniminde, bir özellik analiz etmeye çalıştığınız fenomenin ölçülebilir bir değişkenidir. Belirli veri türleri için, özellik sayısı veri noktalarının sayısına göre çok büyük olabilir. Bu genellikle genetik veya metinsel veriler ile durumdur. 

Çok sayıda özellik, bazı öğrenme algoritmalarını zorlayarak eğitim süresini imkansız hale getirebilir. [Destek vektör makineleri](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/two-class-support-vector-machine?WT.mc_id=docs-article-lazzeri) özellikle çok sayıda özelliğe sahip senaryolar için uygundur. Bu nedenle, bilgi alımından metin ve görüntü sınıflandırmasına kadar birçok uygulamada kullanılmıştır. Destek vektör makineleri hem sınıflandırma hem de regresyon görevleri için kullanılabilir.

Özellik seçimi, belirli bir çıktı verilen girdilere istatistiksel testler uygulama işlemini ifade eder. Amaç, hangi sütunların çıktıyı daha tahmin edici olduğunu belirlemektir. Machine Learning tasarımcısındaki [Filtre Tabanlı Özellik Seçimi modülü,](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/filter-based-feature-selection?WT.mc_id=docs-article-lazzeri) aralarından seçim yapabileceğiniz birden çok özellik seçimi algoritması sağlar. Modül, Pearson korelasyon ve ki-kare değerleri gibi korelasyon yöntemlerini içerir.

[Permütasyon Özelliği Önem modüllerini,](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/permutation-feature-importance?WT.mc_id=docs-article-lazzeri) veri setiniz için bir dizi özellik önem puanını hesaplamak için de kullanabilirsiniz. Daha sonra, bir modelde kullanılacak en iyi özellikleri belirlemenize yardımcı olmak için bu puanlardan yararlanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

 - [Azure Machine Learning tasarımcısı hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/machine-learning/service/concept-designer?WT.mc_id=docs-article-lazzeri)
 - Azure Machine Learning tasarımcısında bulunan tüm makine öğrenimi algoritmalarının açıklamaları için [Bkz.](https://docs.microsoft.com/azure/machine-learning/algorithm-module-reference/module-reference?WT.mc_id=docs-article-lazzeri)
 - Derin öğrenme, makine öğrenimi ve Yapay Algı arasındaki ilişkiyi keşfetmek için [Bkz. Derin Öğrenme ve Makine Öğrenimi](https://docs.microsoft.com/azure/machine-learning/service/concept-deep-learning-vs-machine-learning?WT.mc_id=docs-article-lazzeri)
