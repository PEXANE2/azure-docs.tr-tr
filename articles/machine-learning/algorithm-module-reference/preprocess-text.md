---
title: 'Ön işlem metni: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Metni temizlemek ve basitleştirmek için Azure Machine Learning hizmetinde önceden Işlem metin modülünü nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 09/01/2019
ms.openlocfilehash: 4ee5e90b36f7a8fb3bfb42cad425cbb272553b3a
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210838"
---
# <a name="preprocess-text"></a>Metin ön işleme

Bu makalede Azure Machine Learning hizmeti için görsel arabirim (Önizleme) modülü açıklanır.

Metni temizlemek ve basitleştirmek için **ön Işlem metin** modülünü kullanın. Bu ortak metin işleme işlemlerini destekler:

* Durma sözcüklerini kaldırma
* Belirli hedef dizeleri aramak ve değiştirmek için normal ifadeleri kullanma
* Birden çok ilgili sözcüğü tek bir kurallı forma dönüştüren limon
* Örnek olay normalleştirme
* Sayılar, özel karakterler ve "aaaa" gibi yinelenen karakterlerin dizileri gibi belirli karakter sınıflarının kaldırılması
* E-postaların ve URL 'Lerin tanımlanması ve kaldırılması

**Preprocess metin** modülü şu anda yalnızca İngilizce 'yi destekliyor.

## <a name="configure-text-preprocessing"></a>Metin ön Işleme yapılandırma  

1.  Azure Machine Learning hizmetindeki denemenize **önceden Işlem metin** modülünü ekleyin. Bu modülü **metin analizi**altına bulabilirsiniz.

1. Metin içeren en az bir sütunu olan bir veri kümesini bağlayın.

1. **Dil** açılan listesinden dili seçin.

1. **Temizleyen metin sütunu**: Önceden işlemek istediğiniz sütunu seçin.

1. **Durdurma sözcüklerini kaldır**: Önceden tanımlanmış bir durma sözcüğü listesini metin sütununa uygulamak istiyorsanız bu seçeneği belirleyin. 

    Stopword listeleri dile bağlı ve özelleştirilebilir.

1. **Katileştirme**: Sözcüklerin kendi kurallı biçiminde temsil edilebilmesi istiyorsanız bu seçeneği belirleyin. Bu seçenek, başka türlü benzer metin belirteçlerinin benzersiz oluşum sayısını azaltmak için yararlıdır.

    Katileştirme işlemi yüksek oranda dile bağımlıdır.

1. **Cümleleri Algıla**: Modülün analiz gerçekleştirirken bir cümle sınır işareti eklemesini istiyorsanız bu seçeneği belirleyin.

    Bu modül, cümle sonlandırıcıyı temsil etmek için `|||` üç kanal karakter serisi kullanır.

1. Normal ifadeler kullanarak isteğe bağlı bulma ve değiştirme işlemleri gerçekleştirin.

    * **Özel normal ifade**: Aradığınız metni tanımlayın.
    * **Özel değiştirme dizesi**: Tek bir değiştirme değeri tanımlayın.

1. **Büyük/küçük harf olarak Normalleştir**: ASCII büyük harfli karakterlerini küçük formlarına dönüştürmek istiyorsanız bu seçeneği belirleyin.

    Karakterler normalleştirilmemişse, büyük ve küçük harflerle aynı sözcük iki farklı sözcük olarak değerlendirilir.

1. Ayrıca, işlenen çıkış metninde aşağıdaki karakter veya karakter dizileri türlerini de kaldırabilirsiniz:

    * **Sayıları kaldır**: Belirtilen dile ait tüm sayısal karakterleri kaldırmak için bu seçeneği belirleyin. Kimlik numaraları, etki alanına bağımlıdır ve dile bağımlıdır. Sayısal karakterler bilinen bir sözcüğün integral parçasıysa, sayı kaldırılmayabilir.
    
    * **Özel karakterleri kaldır**: Alfasayısal olmayan özel karakterleri kaldırmak için bu seçeneği kullanın.
    
    * **Yinelenen karakterleri kaldır**: İkiden fazla yineleme için yinelenen ek karakterleri kaldırmak için bu seçeneği belirleyin. Örneğin, "aaaaa" gibi bir sıra "AA" olarak düşürülür.
    
    * **E-posta adreslerini kaldır**: Biçimin `<string>@<string>`herhangi bir dizisini kaldırmak için bu seçeneği belirleyin.  
    * **URL 'Leri kaldır**: Aşağıdaki URL öneklerini içeren herhangi bir sırayı kaldırmak için bu seçeneği belirleyin: `http`, `https`, `ftp`,`www`
    
1. **Fiil karşılamalarını Genişlet**: Bu seçenek yalnızca fiil kodlamalarını kullanan diller için geçerlidir; Şu anda yalnızca Ingilizce. 

    Örneğin, bu seçeneği belirleyerek "hiçbir zaman" kalmazın " olmaz" ifadesini " *Not*edin" olarak değiştirebilirsiniz.

1. **Ters eğik çizgileri eğik çizgiyle Normalleştir**: Tüm örneklerini `\\` öğesine `/`eşlemek için bu seçeneği belirleyin.

1. **Belirteçleri özel karakterlere Böl**: `&` ,`-`Vb. karakterler üzerinde sözcükler kesmek istiyorsanız bu seçeneği belirleyin. Bu seçenek ayrıca, iki kez yinelendiğinde özel karakterleri de azaltabilir. 

    Örneğin, dize `MS---WORD` üç `MS`belirtece `-`ayrılmıştır,,, ve `WORD`.

1. Denemeyi çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning hizmeti için [kullanılabilen modül kümesine](module-reference.md) bakın. 