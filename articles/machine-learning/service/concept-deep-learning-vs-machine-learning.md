---
title: Derin öğrenme ve makine öğrenimi karşılaştırması
titleSuffix: Azure Machine Learning service
description: Derin öğrenme ve makine öğrenimi hakkında bilgi edinin ve her iki kavramın yapay zeka ile ilişkisi vardır. Derin öğrenme, sahtekarlık algılama, ses ve yüz tanıma, yaklaşım Analizi ve zaman serisi tahmin gibi senaryolarda uygulanabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 08/07/2019
ms.openlocfilehash: 3b0546c42ddd5252b35d1a77d7b152733beec233
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70240950"
---
# <a name="deep-learning-vs-machine-learning"></a>Derin öğrenme ve makine öğrenimi karşılaştırması

Bu makale, derin öğrenme ile makine öğrenimini karşılaştırmanıza yardımcı olur. İki kavramın nasıl karşılaştırılacağını ve yapay zeka 'nın daha geniş kategorisine nasıl uyduğunu öğreneceksiniz. Makalede sahtekarlık algılama, ses ve yüz tanıma, yaklaşım Analizi ve zaman serisi tahmini gibi gerçek dünyada senaryolar için derin öğreninizin nasıl uygulanabileceğini de açıklanmaktadır.

## <a name="deep-learning-machine-learning-and-ai"></a>Derin öğrenme, makine öğrenimi ve AI

![İlişki diyagramı: AI ile makine öğrenimi ve derin öğrenme](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Derin öğrenme ve makine öğrenimi hakkında bilgi edinmek için aşağıdaki tanımları göz önünde bulundurun. AI

- **Derin öğrenme** , yapay sinir ağlarını temel alan makine öğrenmesinin bir alt kümesidir. Yapay sinir ağların yapısı birden çok giriş, çıkış ve gizli katmanlardan oluşuyorsa _öğrenme süreci_ _çok önemlidir._ Her katman, giriş verilerini bir sonraki katmanın belirli bir tahmine dayalı görev için kullanabileceği bilgilere dönüştüren birimleri içerir. Bu yapı sayesinde bir makine, kendi veri işleme aracılığıyla bilgi alabilir.

- **Makine öğrenimi** , makinelerin görevleri geliştirmek için deneyim kullanmasını sağlayan tekniklerin (derin öğrenme gibi) kullanıldığı yapay zekanın bir alt kümesidir. _Öğrenme süreci_ aşağıdaki adımlara dayalıdır:

   1. Verileri bir algoritmaya akışın. (Bu adımda, örneğin özellik ayıklama işlemini gerçekleştirerek modele ek bilgi sağlayabilirsiniz.)
   1. Bir modeli eğitmek için bu verileri kullanın.
   1. Modeli test edin ve dağıtın.
   1. Otomatik bir tahmine dayalı görev yapmak için dağıtılan modeli kullanın. (Başka bir deyişle, model tarafından döndürülen tahminleri almak için dağıtılan modeli çağırın ve kullanın.)

- **Yapay zeka (AI)** , bilgisayarların insan zekası taklit etmesini sağlayan bir tekniktir. Machine Learning 'i de içerir. 
 
AI, Machine Learning ve derin öğrenme arasındaki ilişkiyi anlamak önemlidir. Machine Learning, yapay zeka elde etmenin bir yoludur. Makine öğrenimi ve derin öğrenme tekniklerini kullanarak, genellikle insan zekası ile ilişkili görevleri gerçekleştiren bilgisayar sistemleri ve uygulamalar oluşturabilirsiniz. Bu görevler görsel algılama, konuşma tanıma, karar verme ve dil çevirisini içerir.

## <a name="techniques-of-deep-learning-vs-machine-learning"></a>Derin öğrenme ve makine öğrenimi teknikleri 

Machine Learning 'e genel bakış ve derin öğrenime sahip olduğunuza göre, iki tekniği karşılaştıralım. Machine Learning 'de, algoritmanın daha fazla bilgi (örneğin, özellik ayıklama gerçekleştirerek) ile doğru bir tahmin yapma yöntemi olması gerekir. Derin öğreniminde, algoritma yapay sinir ağ yapısı sayesinde kendi veri işleme aracılığıyla doğru bir tahmin yapmayı öğreniyor.

Aşağıdaki tabloda, daha ayrıntılı olarak iki teknik karşılaştırılmaktadır:

| |Tüm makine öğrenimi |Yalnızca derin öğrenme|
|---|---|---|
|  **Veri noktası sayısı** | , Kullanıcılar tarafından sunulan küçük veri miktarlarını kullanabilir. | Kısa ekibinizle yapmak için büyük miktarda eğitim verisi gerektirir. |
|  **Donanım bağımlılıkları** | , Düşük kaliteli makinelerde çalışabilir. Büyük miktarda hesaplama gücüne gerek yoktur. | , Yüksek kaliteli makinelere bağlıdır. Bu, doğal olarak çok sayıda matris çarpma işlemi yapar. GPU, bu işlemleri verimli bir şekilde iyileştirebilir. |
|  **Korturlama işlemi** | Özelliklerin kullanıcılar tarafından doğru şekilde tanımlanması ve oluşturulması gerekir. | Verilerden yüksek düzey özellikleri öğrenir ve kendi kendine yeni özellikler oluşturuyor. |
|  **Öğrenme yaklaşımı** | Görevleri küçük parçalara böler ve ardından alınan sonuçları tek bir sonuç olarak birleştirir. | Sorunu bir uçtan uca temelinde çözer. |
|  **Yürütme süresi** | Birkaç saniye ile birkaç saat arasında uyum sağlamak için karşılaştırmayı daha az zaman alır. | Derin bir öğrenme algoritması birçok parametre içerdiğinden, eğitmek için alışılmadık uzun zaman alır. |
|  **Çıktı** | Çıktı genellikle bir puan veya sınıflandırma gibi sayısal bir değerdir. | Çıktı metin, puan, öğe veya ses olabilir. |

## <a name="deep-learning-use-cases"></a>Derin öğrenme kullanım örnekleri

Yapay sinir ağ yapısı nedeniyle, derinlemesine öğrenme, görüntü, ses, video ve metin gibi yapılandırılmamış verilerde desenleri tanımlamaya. Bu nedenle, derin öğrenme, sağlık, enerji, finans ve taşıma gibi birçok sektörler hızla dönüştürülüyor. Bu endüstriler artık geleneksel iş süreçlerini yeniden düşünürken. 

Derin öğrenme için en yaygın uygulamalardan bazıları aşağıdaki paragraflarda açıklanmıştır.

### <a name="named-entity-recognition"></a>Adlandırılmış varlık tanıma

Ayrıntılı öğrenme ağlarının bir kullanımı, yapılandırılmamış ve etiketsiz verilerden belirli bilgi türlerini ayıklamaya yönelik bir yol olan varlık tanıma olarak adlandırılır. Bu bilgiler kişi, yer, şirket veya şeyler olabilir. Daha sonra bilgiler bir kimlik listesi oluşturmak için yapılandırılmış bir şemada depolanabilir veya bir kimlik doğrulama altyapısı için kıyaslama işlevi görebilir.

### <a name="object-detection"></a>Nesne algılama

Derin öğrenme birçok nesne algılama kullanım durumunda uygulandı. Nesne algılama iki bölümden oluşur: görüntü sınıflandırması ve sonra görüntü yerelleştirme. Görüntü _sınıflandırması_ , görüntünün, otomobiller veya kişiler gibi nesnelerini tanımlar. Görüntü _Yerelleştirme_ bu nesnelerin belirli konumunu sağlar. 

Nesne algılama, oyun, perakende, tourma ve kendi kendine çalışan otomobiller gibi sektörlerde zaten kullanılıyor.

### <a name="image-caption-generation"></a>Resim yazısı oluşturma

Görüntü tanıma gibi, resim yazısı içinde, belirli bir görüntü için sistem, görüntünün içeriğini açıklayan bir resim yazısı oluşturması gerekir. Fotoğraflarındaki nesneleri tespit edebilir ve etiketleyebilir, bir sonraki adım bu etiketlerin açıklayıcı, tutarlı cümleler içine dönüştürülmesi olur. Genellikle, görüntü resim yazısı sistemleri fotoğraflarda nesneleri algılamak için çok büyük evsel sinir ağları kullanır ve ardından etiketleri tutarlı cümlelere dönüştürmek için yinelenen bir sinir ağı (RNN) kullanır.

### <a name="machine-translation"></a>Makine çevirisi

Makine çevirisi, bir dilden sözcükler, ifadeler veya cümleler alır ve bunları otomatik olarak başka bir dile çevirir. Otomatik makine çevirisi uzun bir süre içinde, ancak derin öğrenme iki belirli alana etkileyici sonuçlar elde eder: otomatik metin çevirisi (ve konuşmayı metne çevirme) ve görüntülerin otomatik çevirisi. 

Uygun veri dönüşümünde, derin bir ağ metin, ses ve görsel sinyalleri anlayabilirler. Makine çevirisi, daha büyük ses dosyalarındaki ses kod parçacıklarını tanımlamak ve söylenen sözcük veya görüntüyü metin olarak eklemek için kullanılabilir.

### <a name="text-analytics"></a>Metin analizi

Derin öğrenmesinin önemli bir görevi e-bulma ' dır. Şirketler, Insider ticareti ve kamu yönetmelikleriyle uyumluluğu tespit etmek için derin öğrenmeyi temel alan metin analizi kullanır. Hedge fon, gelecek yatırım performansı ve Pazar yaklaşımı hakkında öngörüler elde etmek için büyük belge depolarında detaya gitmek üzere metin analizi kullanır. Derin öğrenime dayalı metin analizi için kullanım örneği, büyük miktarlarda metin verilerini ayrıştırmaya ve analiz ya da yield toplamaları gerçekleştirmesine olanak sağlar.

## <a name="artificial-neural-networks"></a>Yapay sinir ağları

Yapay sinir ağları bağlı düğümlerin katmanlarına göre biçimlendirilir. Derin öğrenme modelleri, çok sayıda katmana sahip sinir ağlarını kullanır. 

Aşağıdaki bölümlerde en popüler yapay sinir Network typologies vardır.

### <a name="feedforward-neural-network"></a>Feedforward sinir ağı

Feedforward sinir Network, yapay sinir Network 'ün en temel türüdür. Bir feedforward ağında, bilgiler giriş katmanından çıkış katmanına yalnızca bir yönde hareket eder. Feedforward sinir Networks bir girişi bir dizi gizli katmana yerleştirerek bir girişi dönüştürür. Her katman bir dizi değilden oluşur ve her katman, daha önce katmandaki tüm neks 'e tam olarak bağlanır. Son tam bağlı katman (çıkış katmanı) oluşturulan tahminleri temsil eder.

### <a name="recurrent-neural-network"></a>Yinelenen sinir ağı

Yinelenen sinir Networks, yaygın olarak kullanılan yapay bir sinir ağı. Bu ağlar bir katmanın çıkışını kaydeder ve katmanın sonucunun tahmin edilmesine yardımcı olması için giriş katmanına geri akışı sağlar. Yinelenen sinir ağlarında harika öğrenme becerileri vardır. Bunlar, el yazısı öğrenme ve dili tanıma gibi karmaşık görevler için yaygın olarak kullanılır.

### <a name="convolutional-neural-networks"></a>Evsel sinir ağları

Bir evsel sinir ağı, özellikle etkili bir yapay sinir ağı ve benzersiz bir mimari sunar. Katmanlar üç boyutta düzenlenmiştir: genişlik, yükseklik ve derinlik. Bir katmandaki katlara, bir sonraki katmandaki tüm neks 'ler bağlanır, ancak yalnızca katmanın nekatlarındaki küçük bir bölgedir. Son çıktı, derinlik boyutunun tek bir vektörüne indirgenecek ve derinlik boyutu üzerinde düzenlenmiştir. 

Evsel sinir ağları görüntü tanıma ve sınıflandırma gibi alanlarda kullanılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde [Azure Machine Learning hizmetinde](https://docs.microsoft.com/azure/machine-learning/service/?WT.mc_id=docs-article-lazzeri)derin öğrenme teknolojisinin nasıl kullanılacağı gösterilmektedir:

- [Bir TensorFlow modeli kullanarak el ile yazılmış rakamları sınıflandırın](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-tensorflow?WT.mc_id=docs-article-lazzeri)
- [Bir TensorFlow tahmin aracı ve keras kullanarak el ile yazılmış rakamları sınıflandır](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-keras?WT.mc_id=docs-article-lazzeri)
- [Pytorch modeli kullanarak görüntüleri sınıflandırın](https://docs.microsoft.com/azure/machine-learning/service/how-to-train-pytorch?WT.mc_id=docs-article-lazzeri)
- [Bir Chainer modeli kullanarak el ile yazılmış rakamları sınıflandır](https://docs.microsoft.com/en-us/azure/machine-learning/service/how-to-train-chainer?WT.mc_id=docs-article-lazzeri)
