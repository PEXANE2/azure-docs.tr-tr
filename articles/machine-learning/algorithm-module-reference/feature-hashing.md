---
title: Özellik Karma modül başvurusu
titleSuffix: Azure Machine Learning
description: Metin verilerini elde etmek için Azure Machine Learning'deki Özellik Karma modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: 7178417a5c20afe5b1ed02bc526ec174704962df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79456259"
---
# <a name="feature-hashing-module-reference"></a>Özellik Karma modül başvurusu

Bu makalede, Azure Machine Learning tasarımcısına (önizleme) dahil olan bir modül açıklanmaktadır.

Bir İngilizce metin akışını bir tamsayı özelliği kümesine dönüştürmek için Özellik Karma modüllerini kullanın. Daha sonra bu hashed özellik kümesini bir metin analizi modelini eğitmek için bir makine öğrenme algoritmasına geçirebilirsiniz.

Bu modülde sağlanan özellik karma işlevi nimbusml çerçevesine dayanmaktadır. Daha fazla bilgi için Bkz. [NgramHash sınıfı.](https://docs.microsoft.com/python/api/nimbusml/nimbusml.feature_extraction.text.extractor.ngramhash?view=nimbusml-py-latest)

## <a name="what-is-feature-hashing"></a>Özellik karma nedir?

Özellik karma, benzersiz belirteçleri ocaklara dönüştürerek çalışır. Girdi olarak sağladığınız tam dizeleri üzerinde çalışır ve herhangi bir dilsel analiz veya ön işleme gerçekleştirmez. 

Örneğin, bu gibi basit cümleler bir dizi almak, bir duygu puanı takip. Bir model oluşturmak için bu metni kullanmak istediğinizi varsayalım.

|Kullanıcı metni|Yaklaşım|
|--------------|---------------|
|Bu kitaba bayıldım.|3|
|Bu kitaptan nefret ettim.|1|
|Bu kitap harikaydı.|3|
|Kitapları severim.|2|

Dahili olarak, Özellik Karma modülü n-gram bir sözlük oluşturur. Örneğin, bu veri kümesi için bigrams listesi aşağıdaki gibi bir şey olacaktır:

|Dönem (bigrams)|Frequency|
|------------|---------------|
|Bu kitap|3|
|Çok sevdim.|1|
|Nefret ettim.|1|
|Ben seviyorum|1|

**N-gram** özelliğini kullanarak n-gram boyutunu kontrol edebilirsiniz. Bigram'ları seçerseniz, tek gramlar da hesaplanır. Sözlük de bu gibi tek terimler içerecektir:

|Terim (tek gram)|Frequency|
|------------|---------------|
|Kitap|3|
|I|3|
|kitaplar|1|
|oldu|1|

Sözlük oluşturulundan sonra, Özellik Karma modülü sözlük terimlerini karma değerlere dönüştürür. Daha sonra, her durumda bir özelliğin kullanılıp kullanılmadığını hesaplar. Her metin veri satırı için, modül bir sütun kümesi, her hashed özelliği için bir sütun çıkar.

Örneğin, karma sonra, özellik sütunları aşağıdaki gibi görünebilir:

|Derecelendirme|Karma özelliği 1|Karma özelliği 2|Karma özelliği 3|
|-----|-----|-----|-----|
|4|1|1|0|
|5|0|0|0|

* Sütundaki değer 0 ise, satır hashed özelliğini içermiş.
* Değer 1 ise, satır özelliği ni içerdi.

Özellik karma, boyutsallığı azaltmak için değişken uzunluktaki metin belgelerini eşit uzunlukta sayısal özellik vektörleri olarak temsil etmenizi sağlar. Metin sütununu olduğu gibi eğitim için kullanmaya çalıştıysanız, birçok farklı değere sahip kategorik bir özellik sütunu olarak kabul edilir.

Sayısal çıktılar sınıflandırma, kümeleme ve bilgi alma gibi yaygın makine öğrenimi yöntemlerini kullanmayı da mümkün kılar. Arama işlemleri dize karşılaştırmaları yerine tamsayı işbirliği kullanabileceğinden, özellik ağırlıklarını almak da çok daha hızlıdır.

## <a name="configure-the-feature-hashing-module"></a>Özellik Karma modüllerini yapılandırın

1.  Özellik Hashing modüllerini tasarımcıda ardınıza ekleyin.

1. Çözümlemek istediğiniz metni içeren veri kümesini bağlayın.

    > [!TIP]
    > Özellik karmalama, köklenme veya kesilme gibi sözlü işlemler yapmadığından, özellik karmalarını uygulamadan önce metni ön işleme yle bazen daha iyi sonuçlar alabilirsiniz. 

1. **Hedef sütunları,** haşlamet özelliklerine dönüştürmek istediğiniz metin sütunlarına ayarlayın. Unutmayın:

    * Sütunlar dize veri türü olmalıdır.
    
    * Birden çok metin sütunu seçmek, özellik boyutlandırması üzerinde önemli bir etkiye sahip olabilir. Örneğin, 10 bitlik karma sütun sayısı, tek bir sütun için 1.024'ten iki sütun için 2.048'e gider.

1. Karma tabloyu oluştururken kullanılacak bit sayısını belirtmek için **Karma bit boyutunu** kullanın.
    
    Varsayılan bit boyutu 10'dur. Birçok sorun için bu değer yeterlidir. Eğitim metnindeki n-gram kelime nin boyutuna bağlı olarak, çarpışmaları önlemek için daha fazla alana ihtiyacınız olabilir.
    
1. **N-gram**için, eğitim sözlüğüne eklemek için n-gramının maksimum uzunluğunu tanımlayan bir sayı girin. N-gram, benzersiz bir birim olarak kabul edilen *n* sözcükleri dizisidir.

    Örneğin, 3 girerseniz, tek agramlar, bigramlar ve trigrams oluşturulur.

1. Boru hattını gönderin.

## <a name="results"></a>Sonuçlar

İşlem tamamlandıktan sonra, modül, özgün metin sütununbirden çok sütuna dönüştürüldüğü dönüştürülmüş bir veri kümesi ni verir. Her sütun metindeki bir özelliği temsil eder. Sözlüğün ne kadar önemli olduğuna bağlı olarak, ortaya çıkan veri kümesi büyük olabilir:

|Sütun adı 1|Sütun türü 2|
|-------------------|-------------------|
|KULLANICI METNİ|Orijinal veri sütunu|
|Duyguları|Orijinal veri sütunu|
|USERTEXT - Karma özellik 1|Hashed özellik sütunu|
|USERTEXT - Karma özellik 2|Hashed özellik sütunu|
|USERTEXT - Karma özelliği n|Hashed özellik sütunu|
|USERTEXT - Karma özellik 1024|Hashed özellik sütunu|

Dönüştürülmüş veri kümesini oluşturduktan sonra, bunu Tren Modeli modülüne giriş olarak kullanabilirsiniz.
 
## <a name="best-practices"></a>En iyi uygulamalar

Aşağıdaki en iyi uygulamalar, Özellik Karma modülünden en iyi şekilde çıkarmanıza yardımcı olabilir:

* Giriş metnini önceden işlemek için Özellik Hashing'i kullanmadan önce bir İşlem Öncesi Metin modülü ekleyin. 

* Metin sütunlarını çıktı veri kümesinden kaldırmak için Özellik Karma modülünden sonra Sütun seç modülü ekleyin. Karma özellikler oluşturulduktan sonra metin sütunları gerekmez.
    
* Sonuçları basitleştirmek ve doğruluğu artırmak için bu metin önişleme seçeneklerini kullanmayı düşünün:

    * Sözcük kırma
    * Sözcük kaldırmayı durdurma
    * Büyük/küçük harf normalleştirme
    * Noktalama işaretlerinin ve özel karakterlerin kaldırılması
    * Dallanma  

Herhangi bir çözümde uygulanacak en uygun ön işleme yöntemi kümesi etki alanına, kelime dağarcığına ve iş gereksinimine bağlıdır. hangi metin işleme yöntemlerinin en etkili olduğunu görmek için verilerinizle birlikte boru hattı.

## <a name="next-steps"></a>Sonraki adımlar
            
Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın 
