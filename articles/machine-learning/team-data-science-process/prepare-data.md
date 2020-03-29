---
title: ML Studio için hazırlık verileri (klasik) - Takım Veri Bilimi Süreci
description: Makine öğrenimi için etkili bir şekilde kullanılmak üzere hazırlamak için ön işlem ve temiz veri.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: caedcf313ab809e9607907545f26ca1b62bbeca7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720053"
---
# <a name="tasks-to-prepare-data-for-enhanced-machine-learning"></a>Gelişmiş machine learning için verileri hazırlama görevleri
Veri kümesinin model eğitimi için kullanılabilmesi için önce yapılması gereken ön işleme ve temizleme verileri önemli görevlerdir. Ham veriler genellikle gürültülü ve güvenilmezdir ve eksik değerler olabilir. Bu tür verileri modelleme kullanabilirsiniz yanıltıcı sonuçlar doğurabilir. Bu görevler, Ekip Veri Bilimi Süreci'nin (TDSP) bir parçasıdır ve genellikle gerekli ön işlemeyi keşfetmek ve planlamak için kullanılan bir veri kümesinin ilk keşfini izler. TDSP süreci yle ilgili daha ayrıntılı talimatlar [için, Ekip Veri Bilimi Süreci'nde](overview.md)özetlenen adımlara bakın.

Veri arama görevi gibi ön işleme ve temizleme görevleri, SQL veya Hive veya Azure Machine Learning Studio (klasik) gibi çok çeşitli ortamlarda ve verilerinizin nerede olacağına bağlı olarak R veya Python gibi çeşitli araçlar ve dillerde gerçekleştirilebilir. depolanır ve nasıl biçimlendirilir. TDSP doğada yinelemeli olduğundan, bu görevler işlemin iş akışında çeşitli adımlarda gerçekleşebilir.

Bu makalede, Azure Machine Learning Studio'ya (klasik) veri silmeden önce veya sonra üstlenilen çeşitli veri işleme kavramları ve görevleri tanıtılır.

Azure Machine Learning Studio (klasik) içinde yapılan veri arama ve ön işleme örneği için, [Ön işleme veri](https://azure.microsoft.com/documentation/videos/preprocessing-data-in-azure-ml-studio/) videosuna bakın.

## <a name="why-pre-process-and-clean-data"></a>Neden ön işlem ve temiz veri?
Gerçek dünya verileri çeşitli kaynaklardan ve işlemlerden toplanır ve veri kümesinin kalitesinden ödün veren düzensizlikler veya bozuk veriler içerebilir. Ortaya çıkan tipik veri kalitesi sorunları şunlardır:

* **Eksik**: Veri öznitelikleri eksik veya eksik değerleri içeren.
* **Gürültülü**: Veriler hatalı kayıtlar veya aykırılıklar içerir.
* **Tutarsız**: Veriler çakışan kayıtlar veya tutarsızlıklar içerir.

Kalite verileri, kalite tahmine dayalı modeller için bir ön koşuldur. "Çöp içerisini, çöpü dışarı" önlemek ve veri kalitesini artırmak ve bu nedenle performansı modellemek için, veri sorunlarını erken deneve ilgili veri işleme ve temizleme adımlarına karar vermek için bir veri durumu ekranı yürütmek zorunludur.

## <a name="what-are-some-typical-data-health-screens-that-are-employed"></a>Kullanılan bazı tipik veri sağlığı ekranları nelerdir?
Genel veri kalitesini kontrol ederek kontrol edebiliriz:

* **Kayıt**sayısı.
* **Özniteliklerin** (veya **özelliklerin)** sayısı.
* Öznitelik **veri türleri** (nominal, ordinal veya sürekli).
* **Eksik değerlerin**sayısı.
* **İyi biçimlendirilmiş** veriler.
  * Veriler TSV veya CSV'deyse, sütun ayırıcılarının ve çizgi ayırıcılarının her zaman doğru ayrı sütun ve satırları ayırıp ayırmadığını denetleyin.
  * Veriler HTML veya XML biçimindeyse, verilerin kendi standartlarına göre iyi oluşup oluşturulmadığını kontrol edin.
  * Yarı yapılandırılmış veya yapılandırılmamış verilerden yapılandırılmış bilgi elde etmek için ayrıştırma da gerekebilir.
* **Tutarsız veri kayıtları.** İzin verilen değer aralığını denetleyin. Örneğin, veriler öğrenci not ortalamasını (not ortalaması) içeriyorsa, genel not ortalamasının belirlenen aralıkta olup olmadığını kontrol edin, 0~4 deyin.

Verilerle ilgili sorunlar bulduğunuzda, genellikle eksik değerleri temizlemeyi, veri normalleştirmeyi, ayrıştırmayı, veri hizalamasını etkileyebilecek közlediğiniz karakterleri kaldırmak ve/veya değiştirmek için metin işlemeyi, ortak alanlardaki karışık veri türlerini ve diğerlerini içeren **işleme adımları** gereklidir.

**Azure Machine Learning iyi biçimlendirilmiş tabular verileri tüketir.**  Veriler zaten tabular formdaysa, veri ön işleme doğrudan Machine Learning'deki Azure Machine Learning Studio (klasik) ile gerçekleştirilebilir.  Veriler tabular formda değilse, XML'de olduğunu varsayalım, verileri tabular forma dönüştürmek için ayrıştırma gerekebilir.  

## <a name="what-are-some-of-the-major-tasks-in-data-pre-processing"></a>Veri ön işlemedeki başlıca görevler nelerdir?
* **Veri temizleme**: Eksik değerleri doldurun, gürültülü verileri ve aykırılıkları algılave kaldırın.
* **Veri dönüşümü**: Boyutları ve gürültüyü azaltmak için verileri normalleştirin.
* **Veri azaltma**: Daha kolay veri işleme için örnek veri kayıtları veya öznitelikleri.
* **Veri ayrıştırma**: Belirli makine öğrenme yöntemleriyle kullanım kolaylığı için sürekli öznitelikleri kategorik özelliklere dönüştürün.
* **Metin temizleme**: örneğin, sekmeyle ayrılmış bir veri dosyasına katıştırılmış sekmeler, örneğin kayıtları kırabilecek yeni satırlarık lakme, veri hizalanmasına neden olabilecek katıştırılmış karakterleri kaldırın.

Aşağıdaki bölümlerde bu veri işleme adımlarından bazıları ayrıntılı olarak incelenir.

## <a name="how-to-deal-with-missing-values"></a>Eksik değerlerle nasıl başa çıkılalı?
Eksik değerlerle başa çıkmak için, sorunu daha iyi işlemek için önce eksik değerlerin nedenini belirlemek en iyisidir. Tipik eksik değer işleme yöntemleri şunlardır:

* **Silme**: Eksik değerlerle kayıtları kaldırma
* **Kukla ikame**: Eksik değerleri sahte bir değerle değiştirin: örneğin, kategorik olarak *bilinmeyen* veya sayısal değerler için 0.
* **Ortalama değiştirme**: Eksik veriler sayısal ise, eksik değerleri ortalama ile değiştirin.
* **Sık değiştirme**: Eksik veriler kategorikse, eksik değerleri en sık öğeyle değiştirin
* **Regresyon ikamesi**: Eksik değerleri regresse edilmiş değerlerle değiştirmek için bir regresyon yöntemi kullanın.  

## <a name="how-to-normalize-data"></a>Veriler nasıl normale döner?
Veri normalleştirme, sayısal değerleri belirli bir aralıkta yeniden ölçeklendirir. Popüler veri normalleştirme yöntemleri şunlardır:

* **Min-Max Normalizasyon**: Verileri, örneğin 0 ile 1 arasında, min değerinin 0'a, maksimum değerin 1'e ölçeklendirildiği bir aralıkta doğrusal olarak dönüştürün.
* **Z-skoru Normalleştirme**: Veri ortalama ve standart sapmaya göre ölçeklendirin: veriler le ortalama arasındaki farkı standart sapmaya böl.
* **Ondalık ölçeklendirme**: Öznitelik değerinin ondalık noktasını hareket ettirerek verileri ölçeklendirin.  

## <a name="how-to-discretize-data"></a>Veriler nasıl ayrıştırılatır?
Veriler, sürekli değerleri nominal özniteliklere veya aralıklara dönüştürerek ayrıştırılabilir. Bunu yapmanın bazı yolları şunlardır:

* **Eşit Genişlikte Binning**: Bir özniteliğin tüm olası değerlerinin aralığını aynı boyuttaki N gruplarına bölün ve depo gözüne düşen değerleri depo gözü numarasıyla atayın.
* **Eşit Yükseklik Binning**: Her biri aynı sayıda örnek içeren bir öznitelik olası değerlerinin aralığını N gruplarına bölün, ardından depo gözüne düşen değerleri depo gözü numarasıyla atayın.  

## <a name="how-to-reduce-data"></a>Veriler nasıl azaltılı?
Daha kolay veri işleme için veri boyutunu azaltmak için çeşitli yöntemler vardır. Veri boyutuna ve etki alanına bağlı olarak aşağıdaki yöntemler uygulanabilir:

* **Kayıt Örneklemesi**: Veri kayıtlarını örnekle ve yalnızca verilerden temsili alt kümesini seçin.
* **Öznitelik Örneklemesi**: Verilerden yalnızca en önemli özniteliklerin bir alt kümesini seçin.  
* **Toplama**: Verileri gruplara ayırın ve her grup için sayıları depolayın. Örneğin, son 20 yıl içinde bir restoran zincirinin günlük gelir numaraları verilerin boyutunu azaltmak için aylık gelire toplanabilir.  

## <a name="how-to-clean-text-data"></a>Metin verileri nasıl temizlenir?
**Tabular verilerdeki metin alanları,** sütun hizalama ve/veya kayıt sınırlarını etkileyen karakterler içerebilir. Örneğin, sekmeayrılmış bir dosyadaki katıştırılmış sekmeler sütunun yanlış hizalanmasına neden olur ve katıştırılmış yeni satır karakterleri kayıt çizgilerini kırar. Metin yazarken veya okurken yanlış metin kodlama işleme, bilgi kaybına, okunamayan karakterlerin yanlışlıkla tanıtılmasına (nulls gibi) yol açar ve metin ayrıştmasını da etkileyebilir. Uygun hizalama için metin alanlarını temizlemek ve/veya yapılandırılmamış veya yarı yapılandırılmış metin verilerinden yapılandırılmış verileri ayıklamak için dikkatli ayrıştırma ve düzenleme gerekebilir.

**Veri arama,** verilere erken bir görünüm sunar. Bu adım sırasında bir dizi veri sorunu ortaya çıkarılabilir ve bu sorunları gidermek için ilgili yöntemler uygulanabilir.  Sorunun kaynağının ne olduğu ve sorunun nasıl ortaya çıkmış olabileceği gibi sorular sormak önemlidir. Bu işlem, bunları çözmek için atılması gereken veri işleme adımları hakkında karar vermenize de yardımcı olur. Son kullanım örneklerinin ve kişiliklerin belirlenmesi, veri işleme çabasını önceliklendirmek için de kullanılabilir.

## <a name="references"></a>Başvurular
> *Veri Madenciliği: Kavramlar ve Teknikler*, Üçüncü Baskı , Morgan Kaufmann , 2011 , Jiawei Han, Micheline Kamber, ve Jian Pei
> 
> 

