---
title: Derin öğrenme ve makine öğrenimi
titleSuffix: Azure
description: Derin öğrenmenin makine öğrenimi ve yapay zeka ile nasıl ilişkili olduğunu öğrenin. Derin öğrenme, sahtekarlık algılama, ses & yüz tanıma, duygu analitiği ve zaman serisi tahmini gibi senaryolarda kullanılır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: lazzeri
author: FrancescaLazzeri
ms.date: 03/05/2020
ms.openlocfilehash: a83f47942e986e882d99528be3f03f616c2a7e04
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257075"
---
# <a name="deep-learning-vs-machine-learning"></a>Derin öğrenme ve makine öğrenimi

Bu makale, makine öğrenimi ile derin öğrenme yi karşılaştırmanıza yardımcı olur. Bu iki kavramın nasıl karşılaştırışta olduğunu ve daha geniş yapay zeka kategorisine nasıl uyduklarını öğreneceksiniz. Makalede ayrıca, derin öğrenmenin dolandırıcılık algılama, ses ve yüz tanıma, duygu analitiği ve zaman serisi tahmini gibi gerçek dünya senaryolarına nasıl uygulanabileceğini de açıklamaktadır.

## <a name="deep-learning-machine-learning-and-ai"></a>Derin öğrenme, makine öğrenimi ve Yapay AI

![İlişki diyagramı: AI vs makine öğrenimi vs derin öğrenme](./media/concept-deep-learning-vs-machine-learning/ai-vs-machine-learning-vs-deep-learning.png)

Derin öğrenme ve makine öğrenimi ve Yapay A'yı anlamak için aşağıdaki tanımları göz önünde bulundurun:

- **Derin öğrenme** yapay sinir ağlarına dayanan makine öğreniminin bir alt kümesidir. Yapay sinir ağlarının yapısı birden fazla giriş, çıktı ve gizli katmanlardan oluştuğu için _öğrenme süreci_ _derindir._ Her katman, giriş verilerini bir sonraki katmanın belirli bir tahmin görevi için kullanabileceği bilgilere dönüştüren birimler içerir. Bu yapı sayesinde, bir makine kendi veri işleme yoluyla öğrenebilirsiniz.

- **Makine öğrenimi,** makinelerin görevlerde geliştirmek için deneyimi kullanmalarını sağlayan teknikleri (derin öğrenme gibi) kullanan yapay zekanın bir alt kümesidir. _Öğrenme süreci_ aşağıdaki adımlara dayanır:

   1. Verileri bir algoritmaya aktarın. (Bu adımda, örneğin özellik ayıklama gerçekleştirerek modele ek bilgiler sağlayabilirsiniz.)
   1. Bir modeli eğitmek için bu verileri kullanın.
   1. Modeli test edin ve dağıtın.
   1. Otomatik bir tahmin görevi yapmak için dağıtılan modeli tüketin. (Başka bir deyişle, model tarafından döndürülen tahminleri almak için dağıtılan modeli arayın ve kullanın.)

- **Yapay zeka (AI),** bilgisayarların insan zekasını taklit etmesini sağlayan bir tekniktir. Makine öğrenimini içerir. 
 
Yapay bilgi, makine öğrenimi ve derin öğrenme arasındaki ilişkiyi anlamak önemlidir. Makine öğrenimi yapay zekaya ulaşmanın bir yoludur. Makine öğrenimi ve derin öğrenme tekniklerini kullanarak, insan zekası ile sık sık ilişkili görevleri yapan bilgisayar sistemleri ve uygulamaları oluşturabilirsiniz. Bu görevler görüntü tanıma, konuşma tanıma ve dil çeviriiçerir.

## <a name="techniques-of-deep-learning-vs-machine-learning"></a>Derin öğrenme teknikleri ve makine öğrenimi 

Artık makine öğrenimine ve derin öğrenmeye genel bir bakışa sahip olduğunuza göre, iki tekniği karşılaştıralım. Makine öğreniminde, algoritmanın daha fazla bilgi tüketerek (örneğin, özellik çıkarma gerçekleştirerek) doğru bir tahminde nasıl yapılacağının söylenmesi gerekir. Derin öğrenmede, algoritma yapay sinir ağ yapısı sayesinde kendi veri işleme yoluyla doğru bir tahmin yapmayı öğrenebilir.

Aşağıdaki tablo da iki tekniği daha ayrıntılı olarak karşılaştırır:

| |Tüm makine öğrenimi |Sadece derin öğrenme|
|---|---|---|
|  **Veri noktalarının sayısı** | Öngörülerde bulunmak için küçük miktarlarda veri kullanabilir. | Öngörülerde bulunmak için büyük miktarda eğitim verisi kullanması gerekir. |
|  **Donanım bağımlılıkları** | Düşük uç makinelerde çalışabilir. Büyük miktarda hesaplama gücüne ihtiyacı yok. | Üst düzey makinelere bağlıdır. Doğal olarak çok sayıda matris çarpma işlemi yapar. Bir GPU bu işlemleri verimli bir şekilde optimize edebilir. |
|  **Featurization süreci** | Özelliklerin kullanıcılar tarafından doğru bir şekilde tanımlanmasını ve oluşturulmasını gerektirir. | Verilerden üst düzey özellikler öğrenir ve kendi başına yeni özellikler oluşturur. |
|  **Öğrenme yaklaşımı** | Öğrenme sürecini daha küçük adımlara böler. Daha sonra her adımdan elde edilen sonuçları bir çıktıda birleştirir. | Sorunu uçtan uca çözerek öğrenme sürecinde hareket eder. |
|  **Yürütme süresi** | Birkaç saniye ile birkaç saat arasında değişen eğitim nispeten az zaman alır. | Derin öğrenme algoritması birçok katman içerdiğinden genellikle eğitmek uzun zaman alır. |
|  **Çıktı** | Çıktı genellikle sayısal bir değerdir, bir puan veya sınıflandırma gibi. | Çıktı, metin, puan veya ses gibi birden çok biçime sahip olabilir. |

## <a name="deep-learning-use-cases"></a>Derin öğrenme kullanım örnekleri

Yapay sinir ağ yapısı nedeniyle, derin öğrenme görüntü, ses, video ve metin gibi yapılandırılmamış verilerdeki desenleri tanımlamada öne çıkmaktadır. Bu nedenle, derin öğrenme hızla sağlık, enerji, finans ve ulaşım da dahil olmak üzere birçok sanayi, dönüştürerek. Bu endüstriler artık geleneksel iş süreçlerini yeniden gözden kalmaktadır. 

Derin öğrenme için en yaygın uygulamalardan bazıları aşağıdaki paragraflarda açıklanmıştır.

### <a name="named-entity-recognition"></a>Adlandırılmış varlık tanıma

Adlandırılmış varlık tanıma, bir metin parçasını girdi olarak alan ve önceden belirlenmiş bir sınıfa dönüştüren derin bir öğrenme yöntemidir. Bu yeni bilgiler bir posta kodu, bir tarih, bir ürün kimliği olabilir. Bilgiler daha sonra adreslerin bir listesini oluşturmak için yapılandırılmış bir şemada saklanabilir veya kimlik doğrulama altyapısı için bir ölçüt görevi görebilir.

### <a name="object-detection"></a>Nesne algılama

Derin öğrenme birçok nesne algılama kullanım durumlarda uygulanmıştır. Nesne algılama iki bölümden oluşur: görüntü sınıflandırma ve daha sonra görüntü yerelleştirme. Görüntü _sınıflandırması,_ görüntünün arabalar veya insanlar gibi nesnelerini tanımlar. Görüntü _yerelleştirme_ bu nesnelerin belirli konumunu sağlar. 

Nesne algılama zaten oyun, perakende, turizm ve kendi kendine sürüş araba gibi sektörlerde kullanılır.

### <a name="image-caption-generation"></a>Resim yazısı oluşturma

Görüntü tanıma gibi, resim altyazı, belirli bir görüntü için, sistem görüntünün içeriğini açıklayan bir başlık oluşturması gerekir. Fotoğraflardaki nesneleri algılayıp etiketleyebildiğinizde, bir sonraki adım bu etiketleri açıklayıcı cümlelere dönüştürmektir. 

Genellikle, görüntü altyazı uygulamaları görüntüdeki nesneleri tanımlamak ve etiketleri tutarlı cümlelere dönüştürmek için yinelenen bir sinir ağı kullanmak için kıvrımlı sinir ağları kullanır.

### <a name="machine-translation"></a>Makine çevirisi

Makine çevirisi bir dilden sözcükleri veya cümleleri alır ve bunları otomatik olarak başka bir dile çevirir. Makine çevirisi uzun zamandır var, ancak derin öğrenme iki özel alanda etkileyici sonuçlar elde eder: metnin otomatik çevirisi (ve konuşmanın metne çevrilme) ve görüntülerin otomatik çevirisi.

Uygun veri dönüşümü yle, sinirağı metin, ses ve görsel sinyalleri anlayabilir. Makine çevirisi, daha büyük ses dosyalarındaki ses parçalarını tanımlamak ve konuşulan sözcüğü veya resmi metin olarak yazıya çevirmek için kullanılabilir.

### <a name="text-analytics"></a>Metin analizi

Derin öğrenme yöntemlerine dayalı metin analitiği, büyük miktarda metin verisini (örneğin, tıbbi belgeler veya gider makbuzları) analiz etmeyi, desenleri tanımayı ve bunun dışında organize ve kısa bilgiler oluşturmayı içerir.

Şirketler, içeriden bilgi sızdırmayı ve devlet yönetmeliklerine uygunluğu tespit etmek için metin analizi yapmak için derin öğrenmeyi kullanırlar. Başka bir yaygın örnek sigorta dolandırıcılığı: metin analitiği genellikle bir sigorta talebi dolandırıcılık olma olasılığını tanımak için belgelerin büyük miktarda analiz etmek için kullanılmıştır. 

## <a name="artificial-neural-networks"></a>Yapay sinir ağları

Yapay sinir ağları bağlı düğüm katmanları oluşur. Derin öğrenme modelleri çok sayıda katmana sahip sinir ağları kullanır. 

Aşağıdaki bölümlerde en popüler yapay sinir ağı tipolojileri inceletir.

### <a name="feedforward-neural-network"></a>Feedforward sinir ağı

Feedforward sinir ağı yapay sinir ağının en temel türüdür. Bir feedforward ağında, bilgi giriş katmanından çıktı katmanına yalnızca bir yönde hareket eder. Feedforward sinir ağları gizli katmanları bir dizi koyarak bir giriş dönüştürmek. Her tabaka bir nöron kümesinden oluşur ve her katman önce katmandaki tüm nöronlara tamamen bağlıdır. Tam olarak bağlanan son katman (çıktı katmanı) oluşturulan öngörüleri temsil eder.

### <a name="recurrent-neural-network"></a>Tekrarlayan sinir ağı

Tekrarlayan sinir ağları yaygın olarak kullanılan yapay sinir ağıdır. Bu ağlar bir katmanın çıktısını kaydeder ve katmanın sonucunu tahmin etmeye yardımcı olmak için giriş katmanına geri besler. Tekrarlayan sinir ağları nın büyük öğrenme yetenekleri var. Zaman serisi tahmini, el yazısını öğrenme ve dili tanıma gibi karmaşık görevler için yaygın olarak kullanılırlar.

### <a name="convolutional-neural-networks"></a>Konvolutional sinir ağları

Bir kvolutional sinir ağı özellikle etkili bir yapay sinir ağı, ve benzersiz bir mimari sunuyor. Katmanlar üç boyutlu olarak düzenlenir: genişlik, yükseklik ve derinlik. Bir katmandaki nöronlar bir sonraki katmandaki tüm nöronlara değil, sadece katmanın nöronlarının küçük bir bölgesine bağlanır. Son çıktı, derinlik boyutu boyunca düzenlenmiş olasılık puanlarının tek bir vektörüne indirgenir. 

Kvolutional sinir ağları video tanıma, görüntü tanıma ve tavsiye sistemleri gibi alanlarda kullanılmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler [Azure Machine Learning'de](https://docs.microsoft.com/azure/machine-learning/?WT.mc_id=docs-article-lazzeri)derin öğrenme teknolojisini nasıl kullanacağınızı gösterir:

- [TensorFlow modelini kullanarak el yazısı yla yazılmış rakamları sınıflandırma](https://docs.microsoft.com/azure/machine-learning/how-to-train-tensorflow?WT.mc_id=docs-article-lazzeri)

- [TensorFlow tahmincisi ve Keras kullanarak el yazısı yla yazılmış rakamları sınıflandırma](https://docs.microsoft.com/azure/machine-learning/how-to-train-keras?WT.mc_id=docs-article-lazzeri)

- [Pytorch modeli kullanarak görüntüleri sınıflandırma](https://docs.microsoft.com/azure/machine-learning/how-to-train-pytorch?WT.mc_id=docs-article-lazzeri)

- [Chainer modelini kullanarak el yazısı yla yazılmış rakamları sınıflandırma](https://docs.microsoft.com/azure/machine-learning/how-to-train-ml-models)

Ayrıca, modeliniz için algoritmaları seçmek için [Makine Öğrenimi Algoritması Hile Sayfası](../synapse-analytics/sql-data-warehouse/cheat-sheet.md) kullanın.
