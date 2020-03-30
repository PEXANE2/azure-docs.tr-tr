---
title: 'Proprocess Text: Modül Referansı'
titleSuffix: Azure Machine Learning
description: Metni temizlemek ve basitleştirmek için Azure Machine Learning'deki İşlem Öncesi Metin modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/01/2019
ms.openlocfilehash: 6e4d4c8f798418e090caeba091dec33c71f0458f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477502"
---
# <a name="preprocess-text"></a>Metni Ön İşleme

Bu makalede, Azure Machine Learning tasarımcısı (önizleme) bir modül açıklanmaktadır.

Metni temizlemek ve basitleştirmek için **İşlem Öncesi Metin** modüllerini kullanın. Bu yaygın metin işleme işlemlerini destekler:

* Stop-kelimelerin kaldırılması
* Belirli hedef dizeleri aramak ve değiştirmek için normal ifadeleri kullanma
* Birden çok ilişkili sözcüğü tek bir kanonik forma dönüştüren Lemmatizasyon
* Büyük/küçük harf normalleştirme
* Sayılar, özel karakterler ve "aaaa" gibi yinelenen karakterlerin dizileri gibi belirli karakter sınıflarının kaldırılması
* E-postaların ve URL'lerin tanımlanması ve kaldırılması

**Proprocess Text** modülü şu anda yalnızca İngilizce'yi desteklemektedir.

## <a name="configure-text-preprocessing"></a>Metin Önİşleme Yapılandırma  

1.  Azure Machine Learning'de **Ön İşlem Metni** modüllerini ardınıza ekleyin. Bu modülü Text **Analytics**altında bulabilirsiniz.

1. Metin içeren en az bir sütuna sahip bir veri kümesibağlayın.

1. **Dil** açılır listesinden dili seçin.

1. **Temizleyecek metin sütunu**: Önceden işlemek istediğiniz sütunu seçin.

1. **Durdurma sözcüklerini kaldır**: Metin sütununa önceden tanımlanmış bir sözcük listesi uygulamak istiyorsanız bu seçeneği belirleyin. 

    Kronole listeleri dile bağlıdır ve özelleştirilebilir.

1. **Lemmatization**: Sözcüklerin kanonik biçimde temsil edilebilmesini istiyorsanız bu seçeneği seçin. Bu seçenek, aksi takdirde benzer metin belirteçleri benzersiz oluşumlarının sayısını azaltmak için yararlıdır.

    Lemmatizasyon süreci son derece dile bağlıdır...

1. **Cümleleri algılama**: Analiz yaparken modülün bir cümle sınır işareti eklemesini istiyorsanız bu seçeneği seçin.

    Bu modül, cümle sonlandırıcısını `|||` temsil etmek için üç boş¶karakterden oluşan bir dizi kullanır.

1. Normal ifadeleri kullanarak isteğe bağlı bul ve değiştir işlemleri gerçekleştirin.

    * **Özel normal ifade**: Aradığınız metni tanımlayın.
    * **Özel değiştirme dizesi**: Tek bir değiştirme değeri tanımlayın.

1. **Küçük harfe normalleştir**: ASCII büyük harf karakterlerini küçük harf formlarına dönüştürmek istiyorsanız bu seçeneği belirleyin.

    Karakterler normalleştirilemezse, büyük harfli ve küçük harflerdeki aynı sözcük iki farklı sözcük olarak kabul edilir.

1. İşlenen çıktı metninden aşağıdaki karakter veya karakter dizilerini de kaldırabilirsiniz:

    * **Sayıları Kaldır**: Belirtilen dilin tüm sayısal karakterlerini kaldırmak için bu seçeneği belirleyin. Kimlik numaraları etki alanına ve dile bağlıdır. Sayısal karakterler bilinen bir sözcüğün ayrılmaz bir parçasıysa, sayı kaldırılmayabilir.
    
    * **Özel karakterleri kaldır**: Alfasayısal olmayan özel karakterleri kaldırmak için bu seçeneği kullanın.
    
    * **Yinelenen karakterleri kaldır**: İki den fazla yinelenen dizilerde fazladan karakterleri kaldırmak için bu seçeneği belirleyin. Örneğin, "aaaaa" gibi bir dizi "aa" indirgenir.
    
    * **E-posta adreslerini kaldırma**: Biçimin `<string>@<string>`herhangi bir sırasını kaldırmak için bu seçeneği seçin.  
    * **URL'leri Kaldırma**: Aşağıdaki URL önekleri içeren sırayı kaldırmak `http` `https`için `ftp`bu seçeneği seçin: , , ,`www`
    
1. **Expand verb contractions**: Bu seçenek sadece fiil kasılmaları kullanan diller için geçerlidir; şu anda, sadece İngilizce. 

    Örneğin, bu seçeneği seçerek, *"orada kalmazdım" ifadesini "orada kalmazdım"* ifadesini *değiştirebilirsiniz.*

1. **Slashes için backslashes normalleştirin**: Bu seçeneği `\\` `/`seçerek tüm örnekleri eşleme .

1. **Özel karakterlerdeki belirteçleri böl**: `&`, , `-`ve benzeri karakterlerde sözcükleri kırmak istiyorsanız bu seçeneği belirleyin. Bu seçenek, ikiden fazla tekrarladığında özel karakterleri de azaltabilir. 

    Örneğin, dize `MS---WORD` üç belirteçleri `MS`ayrılır, `-`, `WORD`, ve .

1. Boru hattını gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın. 