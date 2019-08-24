---
title: Derin öğrenme ve makine öğrenimi karşılaştırması
titleSuffix: Azure Machine Learning service
description: Derin öğrenme ve makine öğrenimi arasındaki ilişki ve yapay zeka kategorisine nasıl uyum sağladığını öğrenin. Bu makalede sahtekarlık algılama, ses ve yüz tanıma, yaklaşım Analizi ve zaman serisi tahmini gibi derin öğrenme senaryoları nasıl çözeceği açıklanmaktadır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 08/07/2019
ms.openlocfilehash: 5301be493ef22fc2d74cc337d88b04caf391fdaa
ms.sourcegitcommit: 6d2a147a7e729f05d65ea4735b880c005f62530f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69982802"
---
# <a name="deep-learning-vs-machine-learning"></a>Derin öğrenme ve makine öğrenimi karşılaştırması

Bu makale, derin öğrenme ile makine öğrenimi arasındaki ilişkiyi anlamanıza yardımcı olur. İki kavramın nasıl karşılaştırılacağını ve yapay zeka 'nın daha geniş kategorisine nasıl uyduğunu öğreneceksiniz. Son olarak, makalede sahtekarlık algılama, ses ve yüz tanıma, yaklaşım Analizi ve zaman serisi tahmini gibi gerçek dünyada senaryolar için derin öğrenmenin nasıl uygulanabileceğini açıklanmaktadır.

## <a name="how-do-deep-learning-machine-learning-and-ai-relate-to-one-another"></a>Derin öğrenme, makine öğrenimi ve AI bir birbirleriyle nasıl ilişkilidir?

![İlişki diyagramı: AI ile makine öğrenimi ve derin öğrenme](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Derin öğrenme ve makine öğrenimi ile arasındaki farkı anlamak için AI, bu çalışma alanlarının her biri hakkında bilgi edinin:

- **Derin öğrenme** , bir makinenin kendisini eğmesine izin veren yapay sinir Networks tabanlı makine öğrenimi 'nin bir alt kümesidir. Bu durumda, yapay sinir ağların yapısı birden çok giriş, çıkış ve gizli katmanlardan oluşur, _öğrenme süreci_ _derin_olarak adlandırılır. Her katman, giriş verilerini bir sonraki katmanın belirli bir tahmine dayalı görevi gerçekleştirmek için kullanabileceği bilgilere dönüştüren birimleri içerir. Bu yapı sayesinde bir makine, kendi veri işleme aracılığıyla öğreniyor.

- **Makine öğrenimi** , makinelerin deneyimle görevlerde iyileştirilmesine olanak tanıyan teknikler (derin öğrenme gibi) içeren yapay zekanın bir alt kümesidir. Bu durumda, _öğrenme süreci_ aşağıdaki adımlara dayalıdır:

    1. Daha fazla bilgi (örneğin, özellik ayıklama işlemi gerçekleştirerek) sağlayarak bir algoritmayı verilerle akışa ın.
    2. Bir modeli eğitmek için bu verileri kullanın.
    3. Modeli test edin ve dağıtın.
    4. Dağıtılan modeli, belirli bir otomatik tahmine dayalı görev gerçekleştirmek için kullanın.

- **Yapay zeka (AI)** , bilgisayarların insan zekası taklit etmesini sağlayan bir tekniktir. Machine Learning 'i de içerir. _AI ile Machine Learning ile derin öğrenme_arasındaki ilişkiyi anlamak önemlidir: Makine öğrenimi, yapay zeka sağlamanın bir yoludur. Bu, makine öğrenimi ve derin öğrenme tekniklerinin kullanımı sayesinde, görsel Perception, konuşma tanıma, karar verme ve diller arasında çeviri.

## <a name="comparison-of-deep-learning-vs-machine-learning"></a>Derin öğrenme ve makine öğrenimi karşılaştırması

Machine Learning 'e genel bakış ve derin öğrenime sahip olduğunuza göre, iki tekniği karşılaştırın. Machine Learning söz konusu olduğunda, algoritmanın daha fazla bilgi vererek doğru bir tahmin nasıl yapılacağı konusunda söyleilmesi gerekir; Öte yandan, derinlemesine öğrenmek durumunda algoritma yapay sinir ağ yapısına teşekkürler kendi veri işleme aracılığıyla öğreniyor.

Aşağıdaki tabloda, iki teknik daha ayrıntılı olarak karşılaştırılmaktadır:

| |Tüm makine öğrenimi |Yalnızca derin öğrenme|
|---|---|---|
|  **Veri noktası sayısı** | , Kullanıcılar tarafından sunulan küçük veri miktarlarını kullanabilir | Kısa ekibinizle yapmak için büyük miktarda eğitim verisi gerektirir |
|  **Donanım bağımlılıkları** | , Düşük kaliteli makinelerde çalışabilir. Büyük miktarda hesaplama gücüne gerek yoktur | , Yüksek kaliteli makinelere bağlıdır. Bu, doğal olarak çok sayıda matris çarpma işlemi yapar. Bu işlemler, GPU kullanılarak etkili bir şekilde iyileştirilebilir |
|  **Korturlama işlemi** | Özelliklerin Kullanıcı tarafından doğru şekilde tanımlanması ve oluşturulması gerekir | Verilerin üst düzey özelliklerini öğrenir ve kendi kendine yeni özellikler oluşturuyor |
|  **Öğrenme yaklaşımı** | Görevleri küçük parçalara böler ve ardından alınan sonuçları tek bir sonuç olarak birleştirir | Sorunu, uçtan uca temelinde çözer |
|  **Yürütme süresi** | Karşılaştırmayı, birkaç saniyeden birkaç saat arasında değişen bir şekilde eğitmeniz daha az zaman alır | Derinlemesine bir öğrenme algoritmasında çok sayıda parametre olduğundan, eğitmek uzun zaman alır. |
|  **Çıktı** | Çıktı genellikle bir puan veya sınıflandırma gibi sayısal bir değerdir | Çıktı, bir puandan, metinden, bir öğeden veya bir sesten herhangi bir şey olabilir |

## <a name="deep-learning-use-cases-what-problems-does-it-solve"></a>Derin öğrenme kullanım örnekleri: Hangi sorunları çözüyor?

Yapay sinir ağ yapısı nedeniyle, görüntü, ses, video ve metin gibi yapılandırılmamış verilerde desenleri tanımlamaya göre daha derin öğrenme. Bu nedenle, geleneksel iş süreçlerini yeniden düşünmek için sağlık, enerji, fintech, ulaşım ve diğerleri gibi birçok sektörler hızla dönüştürülüyor. Derin öğrenme için en yaygın uygulamalardan bazıları aşağıdaki paragraflarda açıklanmıştır.

### <a name="named-entity-recognition"></a>Adlandırılmış varlık tanıma

Derin öğrenme ağlarının bir kullanımı, yapılandırılmamış, etiketlenmemiş verilerden kişiler, konumlar, şirketler veya şeyler gibi belirli türde bilgileri ayıklamaya yönelik bir yol olan varlık tanıma olarak adlandırılır. Bu bilgiler daha sonra bir kimlik listesi oluşturmak için yapılandırılmış bir şemada depolanabilir veya bir kimlik doğrulama altyapısı için kıyaslama işlevi görebilir.

### <a name="object-detection"></a>Nesne algılama

Derin öğrenme birçok nesne algılama kullanım durumunda uygulandı. Nesne algılama, aslında iki bölümden oluşan bir işlemdir: görüntü sınıflandırması ve sonra görüntü yerelleştirme. Görüntü sınıflandırması, görüntüdeki nesnelerin bir araba veya kişi gibi ne kadar olduğunu belirler, görüntü yerelleştirme bu nesnelerin belirli konumunu sağlar. Nesne algılama, oyun, perakende, tourma ve kendi kendine çalışan araba sektörlerinde zaten kullanılıyor.

### <a name="image-caption-generation"></a>Resim yazısı oluşturma

Görüntü tanıma görevine benzer şekilde, görüntü resim yazısı, belirli bir görüntü için sistemin, görüntünün içeriğini açıklayan bir resim yazısı oluşturması gereken görevdir. Fotoğraflarındaki nesneleri tespit edebilir ve bu nesneler için Etiketler oluşturduktan sonra, bir sonraki adımın bu etiketleri tutarlı bir cümle açıklamasına dönüştürmek için olduğunu görebilirsiniz. Genellikle sistemler, fotoğraflarda nesne algılaması için çok büyük evsel sinir ağlarının kullanımını ve ardından etiketleri tutarlı bir cümle haline döndürmek için bir yinelenen sinir ağı (RNN) içerir.

### <a name="machine-translation"></a>Makine çevirisi

Makine çevirisi, bir dilden sözcükler, ifadeler veya cümleler alır ve bunları otomatik olarak başka bir dile çevirir. Otomatik makine çevirisi uzun bir süre içinde, ancak derin öğrenme iki belirli alanın en iyi sonucunu elde ediyor: otomatik metin çevirisi (ve konuşmayı metne dönüştürme) ve görüntülerin otomatik çevirisi. Uygun veri dönüşümünde, derin bir ağ metin, ses ve görsel sinyalleri anlayabilme özelliğine sahiptir. Makine çevirisi, daha büyük ses dosyalarındaki ses kod parçacıklarını tanımlamak ve söylenen sözcük veya görüntüyü metin olarak eklemek için kullanılabilir.

### <a name="text-analytics"></a>Metin analizi

Derin öğrenmesinin gerçekleştirebileceği önemli bir görev da e-bulma ' dır. Şirketler, Insider ticaret algılaması ve kamu mevzuatı uyumluluğu için derin öğrenme tabanlı metin analizi kullanıyor. Hedge fon, gelecek yatırım performansı ve Pazar yaklaşımı hakkında öngörüler elde etmek için büyük belge depolarında detaya gitmek üzere metin analizi kullanır. Derin öğrenme tabanlı metin analizi için kullanım örneği, analiz veya yield toplamalar gerçekleştirmek üzere büyük miktarlarda metin verilerini ayrıştırmaya olanak sağlar.

## <a name="what-are-artificial-neural-networks"></a>Yapay sinir Networks nedir?

Yapay sinir ağları bağlı düğümlerin katmanlarına göre biçimlendirilir. Derin öğrenme modelleri, çok fazla sayıda katmanda sinir ağlarını kullanır. En popüler yapay sinir ağı typologies aşağıda ele alınmıştır.

### <a name="feedforward-neural-network"></a>Feedforward sinir ağı

Feedforward sinir ağı, bilgilerin giriş katmanından çıkış katmanına yalnızca bir yönde hareket eden yapay sinir Network 'ün en temel türüdür. Feedforward sinir Networks bir girişi bir dizi gizli katmana yerleştirerek bir girişi dönüştürür. Her katman, her katmanın daha önce katmandaki tüm nelanlar 'a tam olarak bağlandığı bir dizi neurlar kümesinden oluşur. Son olarak, oluşturulan tahminleri temsil eden bir son tam bağlı katman (çıkış katmanı) vardır.

### <a name="recurrent-neural-network"></a>Yinelenen sinir ağı

Yinelenen sinir Networks, bir katmanın çıkışını kaydetme ve katmanın sonucunu tahmin etmeye yardımcı olması için onu giriş katmanına geri kaydeden yapay sinir ağının daha yaygın olarak kullanılan bir türüdür. Bu sinir ağları daha fazla öğrenme yeteneklerine sahiptir ve el yazısı veya dil tanımayı öğrenme gibi daha karmaşık görevler için yaygın olarak kullanılır.

### <a name="convolutional-neural-networks"></a>Evsel sinir ağları

Bir evsel sinir ağı, benzersiz bir mimari sunan, özellikle etkili bir yapay sinir Network türüdür. İlk olarak, Katmanlar üç boyutta düzenlenir: genişlik, yükseklik ve derinlik. Ayrıca, bir katmandaki katlarca, bir sonraki katmandaki tüm neks 'ler, ancak yalnızca küçük bir bölgesine bağlanmayın. Son olarak, son çıktı her bir olasılık puanı vektörüne indirgenecek ve bu da derinlik boyutu üzerinde düzenlenir. Bu sinir ağları görüntü tanıma ve sınıflandırma gibi alanlarda kullanılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde [Azure Machine Learning hizmetinde](/azure/machine-learning/service/)derin öğrenme teknolojisinin nasıl kullanılacağı gösterilmektedir:

- [Bir TensorFlow modeliyle el ile yazılmış rakamları sınıflandır](how-to-train-tensorflow.md)
- [Bir TensorFlow tahmin aracı ve keras ile el ile yazılmış rakamları sınıflandır](how-to-train-keras.md)
- [Bir Pytorch modeliyle görüntüleri sınıflandır](how-to-train-pytorch.md)
- [El ile yazılmış rakamları bir Chainer modeliyle sınıflandırma](how-to-train-pytorch.md)
