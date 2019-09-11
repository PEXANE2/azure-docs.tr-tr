---
title: 'Özellik karma: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Metin verilerini kullanmak için Azure Machine Learning hizmetinde Özellik karma modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 09/01/2019
ms.openlocfilehash: 78d01cf071faed312773ebf12c75e7e6e5596e71
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210851"
---
# <a name="feature-hashing"></a>Özellik karma

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Ingilizce metin akışını bir tamsayı özellikleri kümesine dönüştürmek için **özellik karma** modülünü kullanın. Daha sonra bu karma özellik kümesini bir makine öğrenimi algoritmasına geçirebilirsiniz ve bir metin analizi modelini eğitebilirsiniz.

Bu modülde sunulan özellik karma işlevselliği nimbusml çerçevesini temel alır. Daha fazla bilgi için bkz. [Ngramhash sınıfı](https://docs.microsoft.com/python/api/nimbusml/nimbusml.feature_extraction.text.extractor.ngramhash?view=nimbusml-py-latest).

## <a name="what-is-feature-hashing"></a>Özellik karma nedir

Özellik karması, benzersiz belirteçleri tamsayılara dönüştürerek işe yarar. Giriş olarak sağladığınız tam dizeler üzerinde çalışır ve herhangi bir dil analizi veya ön işleme gerçekleştirmez. 

Örneğin, bunlar gibi bir basit tümceler ve ardından bir yaklaşım puanı alın. Bir model oluşturmak için bu metni kullanmak istediğinizi varsayın.

|KULLANICI METNI|YAKLAŞIM|
|--------------|---------------|
|Bu kitabı sevdim|3|
|Bu kitabı aldım|1\.|
|Bu kitap harika|3|
|Sevdiğim kitaplar|2|

Dahili olarak, **özellik karma** modülü n-gram sözlüğü oluşturur. Örneğin, bu veri kümesi için bigram listesi şuna benzer olacaktır:

|TERIM (bigram)|LEMIYOR|
|------------|---------------|
|Bu kitap|3|
|Sevdim|1\.|
|Ben|1\.|
|Sevdiğim|1\.|

N-gram özelliğini kullanarak n-gram boyutunu kontrol edebilirsiniz. Bigram seçerseniz, tek tek gram da hesaplanır. Sözlük aşağıdaki gibi tek terimleri de içerir:

|Terim (tek gram)|LEMIYOR|
|------------|---------------|
|Kitabın|3|
|I|3|
|kitap|1\.|
|sayfası|1\.|

Sözlük derlendikten sonra, **özellik karma** modülü sözlük koşullarını karma değerlerine dönüştürür. Daha sonra her durumda bir özelliğin kullanılıp kullanılmadığını hesaplar. Her metin verisi satırı için, modül bir sütun kümesi, her karma özellik için bir sütun verir.

Örneğin, karma olduktan sonra Özellik sütunları şuna benzeyebilir:

|Derecelendirme|Karma Özellik 1|Karma özelliği 2|Karma özellik 3|
|-----|-----|-----|-----|
|4|1\.|1|0|
|5|0|0|0|

* Sütundaki değer 0 ise, satır karma özelliği içermiyordu.
* Değer 1 ise, satır özelliği içerir.

Özellik karması, değişken uzunluklu metin belgelerini, eşit uzunlukta bir boyut azaltma elde etmek için eşit uzunlukta bir sayısal Özellik vektörü olarak temsil etmenizi sağlar. Eğitim için metin sütununu olduğu gibi kullanmaya çalıştıysanız, çok sayıda farklı değer içeren kategorik özellik sütunu olarak kabul edilir.

Sayısal çıktılar Ayrıca sınıflandırma, kümeleme ve bilgi alma gibi yaygın makine öğrenimi yöntemlerini kullanmayı mümkün kılar. Arama işlemleri dize karşılaştırmaları yerine tamsayı karmaları kullanabileceğinden, özellik ağırlıklarını elde etmek de çok daha hızlıdır.

## <a name="configure-feature-hashing"></a>Özellik karma Yapılandırma

1.  Visual Interface 'deki denemenize **özellik karma** modülünü ekleyin.

1. Çözümlemek istediğiniz metni içeren veri kümesini bağlayın.

    > [!TIP]
    > Özelliği karma hale getirmek veya kesilme gibi sözcük temelli işlemler gerçekleştirmediğinden, özellik karmacının uygulamadan önce metin ön işlemesi yaparak bazen daha iyi sonuçlar elde edebilirsiniz. 

1. **Hedef sütunları** karma özelliklerine dönüştürmek istediğiniz metin sütunlarına ayarlayın. 

    * Sütunlar dize veri türü olmalıdır.
    
    * Birden çok metin sütununun seçilmesi Özellik boyutalliği üzerinde önemli bir etkiye sahip olabilir. Örneğin, 10 bitlik bir karma için sütun sayısı, iki sütun için tek bir sütun 1024 ' den 2048 ' e gider.

1. Karma tablo oluştururken kullanılacak bit sayısını belirtmek için **karma bit boyutunu** kullanın.
    
    Varsayılan bit boyutu 10 ' dur. Birçok sorun için bu değer yeterlidir. Eğitim metni n-gram sözlüğü boyutuna bağlı olarak çarpışmalardan kaçınmak için daha fazla alana ihtiyacınız olabilir.
    
1. **N-gram**için, eğitim sözlüğüne eklemek üzere en fazla n gram uzunluğunu tanımlayan bir sayı yazın. N-gram, benzersiz birim olarak kabul edilen *n* sözcükten oluşan bir dizidir.

    Örneğin, 3, unigram, bigram ve trigram girerseniz.

1. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

İşlem tamamlandıktan sonra modül, özgün metin sütununun, her biri metindeki bir özelliği temsil eden birden çok sütuna dönüştürüldüğü dönüştürülmüş bir veri kümesi verir. Sözlüğün ne kadar önemli olduğuna bağlı olarak, elde edilen veri kümesi büyük olabilir:

|Sütun adı 1|Sütun türü 2|
|-------------------|-------------------|
|KULLANICI METNI|Özgün veri sütunu|
|YAKLAŞIM|Özgün veri sütunu|
|USERTEXT-karma Özellik 1|Karma özellik sütunu|
|USERTEXT-karma özelliği 2|Karma özellik sütunu|
|USERTEXT-karma özelliği n|Karma özellik sütunu|
|USERTEXT-karma özelliği 1024|Karma özellik sütunu|

Dönüştürülmüş veri kümesini oluşturduktan sonra, bunu eğitme modeli modülüne giriş olarak kullanabilirsiniz.
 
### <a name="best-practices"></a>En iyi uygulamalar

Aşağıdaki en iyi yöntemler, **özellik karma** modülünden en iyi şekilde yararlanmanıza yardımcı olabilir:

* Giriş metnini önceden işlemek için **özellik karma özelliğini** kullanmadan önce bir **ön işlem metin** modülü ekleyin. 

* Metin sütunlarını çıkış veri kümesinden kaldırmak için **özellik karma** modülünden sonra **sütun seçme** modülü ekleyin. Karma Özellikler oluşturulduktan sonra metin sütunlarına gerek yoktur.
    
* Sonuçları basitleştirmek ve doğruluğu artırmak için bu metin ön işleme seçeneklerini kullanmayı göz önünde bulundurun:

    * Sözcük bölme
    * Word kaldırmayı durdur
    * Örnek olay normalleştirme
    * Noktalama işaretleri ve özel karakterlerin kaldırılması
    * Kesintilerinden kaynaklanan  

Herhangi bir çözümde uygulanacak en uygun ön işleme yöntemleri, etki alanı, sözlük ve iş gereksinimlerinize bağlıdır. Hangi metin işleme yöntemlerinin en etkili olduğunu görmek için verilerinize denemeler yapın.

## <a name="next-steps"></a>Sonraki adımlar
            
Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 
