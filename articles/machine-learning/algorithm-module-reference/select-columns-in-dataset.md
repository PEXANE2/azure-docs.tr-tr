---
title: "Dataset'te Sütun seçin: Modül Başvurusu"
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de Veri Kümesi'ndeki Sütunları Seç modülünü kullanarak akış aşağı işlemlerinde kullanılacak sütun alt kümesini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: 714148815fa4ff543ecbf86ab21e8d51fe775543
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77153783"
---
# <a name="select-columns-in-dataset-module"></a>Dataset modülünde Sütunları Seçin

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Akış aşağı işlemlerinde kullanmak üzere sütunalt kümesini seçmek için bu modülü kullanın. Modül, sütunları kaynak veri kümesinden fiziksel olarak kaldırmaz; bunun yerine, çok bir veritabanı *görünümü* veya *projeksiyon*gibi sütunların bir alt kümesi oluşturur.

Bu modül, bir akış aşağı işlemi için kullanılabilir sütunları sınırlamanız gerektiğinde veya gereksiz sütunları kaldırarak veri kümesinin boyutunu küçültmek istediğinizde yararlıdır.

Veri kümesindeki sütunlar, farklı bir sırada belirtseniz bile, özgün verilerle aynı sırada çıktı lanır.

## <a name="how-to-use"></a>Nasıl kullanılır

Bu modülün parametreleri yok. Sütun seçiciyi, eklemek veya hariç tutmak için sütunları seçmek için kullanırsınız.

### <a name="choose-columns-by-name"></a>Sütunları ada göre seçin

Modülde sütunları ada göre seçmek için birden çok seçenek vardır: 

+ Filtreleme ve arama

    BY **NAME** seçeneğini tıklatın.

    Zaten doldurulmuş bir veri kümesini bağladıysanız, kullanılabilir sütunların bir listesi görünmelidir. Sütun görünmüyorsa, sütun listesini görüntülemek için yukarı akım modüllerini çalıştırmanız gerekebilir.

    Listeye filtre uygulayın, arama kutusuna yazın. Örneğin, arama kutusuna `w` harfi yazarsanız, liste harfi `w`içeren sütun adlarını göstermek için filtrelenir.

    Sütunları seçin ve seçili sütunları sağ bölmedeki listeye taşımak için sağ ok düğmesini tıklatın.

    + Sürekli sütun adları aralığı seçmek için **Shift + Click 'e basın.**
    + Seçime tek tek sütun eklemek için **Ctrl + Tıkla 'ya basın.**

    Kaydetmek ve kapatmak için onay işareti düğmesini tıklatın.

+ Adları diğer kurallarla birlikte kullanma

    With **RULES** seçeneğini tıklatın.
    
    Belirli bir veri türünün sütunlarını göstermek gibi bir kural seçin.

    Ardından, seçim listesine eklemek için bu türdeki sütunları ada göre tek tek tıklatın.

+ Virgülden ayrılmış sütun adlarının listesini yazın veya yapıştırın

    Veri kümeniz genişse, sütunları tek tek seçmek yerine dizinleri veya oluşturulan ad listelerini kullanmak daha kolay olabilir. Listeyi önceden hazırladığınızı varsayarsak:

    1. With **RULES** seçeneğini tıklatın. 
    2. **Sütun Yok'u**seçin, **Ekle'yi**seçin ve ardından kırmızı ünlem işaretiyle metin kutusunun içine tıklayın. 
    3. Daha önce doğrulanmış sütun adlarının virgülle ayrılmış bir listesini yapıştırın veya yazın. Herhangi bir sütunun geçersiz bir adı varsa modülü kaydedemezsiniz, bu nedenle adları önceden kontrol ettiğinizden emin olun.
    
    Dizin değerlerini kullanan sütunların listesini belirtmek için de bu yöntemi kullanabilirsiniz. 

### <a name="choose-by-type"></a>Türüne göre seçin

**WITH RULES** seçeneğini kullanıyorsanız, sütun seçimlerine birden çok koşul uygulayabilirsiniz. Örneğin, yalnızca sayısal veri türüne ait özellik sütunlarını almanız gerekebilir.

**BEGIN WITH** seçeneği başlangıç noktanızı belirler ve sonuçları anlamak için önemlidir. 

+ **TÜM SÜTUNLAR** seçeneğini seçerseniz, tüm sütunlar listeye eklenir. Ardından, belirli koşulları karşılayan sütunları *kaldırmak* için **Dışla** seçeneğini kullanmanız gerekir. 

    Örneğin, tüm sütunlarla başlayıp sütunları ada göre veya türüne göre kaldırabilirsiniz.

+ **SÜTUNYOK** seçeneğini seçerseniz, sütunlar listesi boş başlar. Ardından listeye sütun *eklemek* için koşullar belirtirsiniz. 

    Birden çok kural uygularsanız, her koşul **katkı maddesidir.** Örneğin, sütunsuz olarak başladığınızı ve tüm sayısal sütunları almak için bir kural eklediğinizi varsabilirsiniz. Otomobil fiyat veri kümesinde, bu 16 sütun la sonuçlanır. Ardından, yeni **+** bir koşul eklemek için işareti tıklatın ve **tüm özellikleri ekle'yi**seçin. Elde edilen veri kümesi, tüm sayısal sütunların yanı sıra bazı dize özellik sütunları da dahil olmak üzere tüm özellik sütunlarını içerir.

### <a name="choose-by-column-index"></a>Sütun dizine göre seçin

Sütun dizini, özgün veri kümesi içindeki sütunun sırasını ifade eder.

+ Sütunlar 1'den başlayarak sırayla numaralandırılır.  
+ Bir dizi sütun elde etmek için tire kullanın. 
+ Açık uçlu belirtimler gibi `1-` veya `-3` izin verilmez.
+ Yinelenen dizin değerlerine (veya sütun adlarının) izin verilmez ve bir hataya neden olabilir.

Örneğin, veri kümenizde en az sekiz sütun olduğunu varsayarsak, bitişik olmayan birden çok sütun döndürmek için aşağıdaki örneklerden herhangi birini yapıştırabilirsiniz: 

+ `8,1-4,6`
+ `1,3-8`
+ `1,3-6,4` 

son örnek bir hataya yol lanmaz; ancak, sütunun `4`tek bir örneğini döndürür.



### <a name="change-order-of-columns"></a>Sütunların sırasını değiştirme

Seçimde **sütun sırasını kopyalamaya izin verme ve koruma** seçeneği boş bir listeyle başlar ve ada veya dizine göre belirttiğiniz sütunlar ekler. Sütunları her zaman "doğal sırayla" döndüren diğer seçeneklerin aksine, bu seçenek sütunları adlandırdığınız veya listelediğiniz sırada çıkar. 

Örneğin, Col1, Col2, Col3 ve Col4 sütunlarının yer verdiği bir veri kümesinde, aşağıdaki listelerden birini belirterek sütunların sırasını tersine çevirebilir ve sütun 2'yi dışarıda bırakabilirsiniz:

+ `Col4, Col3, Col1`
+ `4,3,1`


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 