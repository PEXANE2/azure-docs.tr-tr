---
title: Veri filtreleme-özel çevirici
titleSuffix: Azure Cognitive Services
description: Özel bir sistem eğitimi için kullanılmak üzere belge gönderdiğinizde, belgeler bir dizi işleme ve filtreleme adımlarında eğitim için hazırlanmaya devam edin.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 67807e18559006b7d7eb6089a30370d614aefca3
ms.sourcegitcommit: fc718cc1078594819e8ed640b6ee4bef39e91f7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83992853"
---
# <a name="data-filtering"></a>Veri filtreleme

Özel bir sistem eğitimi için kullanılmak üzere belge gönderdiğinizde, belgeler bir dizi işleme ve filtreleme adımlarında eğitim için hazırlanmaya devam edin. Bu adımlar burada açıklanmıştır. Filtrelemeye ilişkin bilgi, özel çevirmende görünen tümce sayısını anlamanıza yardımcı olabilir ve belgeleri özel çevirmenle eğitmek üzere hazırlamak için kendinize uygulayabileceğiniz adımlar.

## <a name="sentence-alignment"></a>Tümce hizalama
Belgeniz XLıFF, TMX veya HIZALAMA biçiminde değilse, özel çevirmen, kaynak ve hedef belgelerinizin cümlelerini birbirlerine ve tümce cümlelere göre hizalar. Translator belge hizalaması gerçekleştirmez. diğer dilin eşleşen belgesini bulmak için belge adlandırmalarınızın ardından gelir. Belge içinde özel çevirici, ilgili cümleyi diğer dilde bulmaya çalışır. Hizalamayla yardım etmek için katıştırılmış HTML etiketleri gibi belge işaretlemesini kullanır.  

Kaynak ve hedef kenar belgelerindeki Tümcelerin sayısı arasında büyük bir tutarsızlık görürseniz, belgeniz ilk yerde paralel olmayabilir ya da başka nedenlerden dolayı hizalanamaz. Belge çiftleri büyük bir fark (>% 10) Her yandaki cümleler, aslında paralel olduklarından emin olmak için ikinci bir görünüm sağlar. Tümce sayısı suspiciously farklıysa, özel çevirmen belgenin yanında bir uyarı gösterir.  


## <a name="deduplication"></a>Yinelenenleri kaldırma
Özel çevirici, eğitim verilerinden test ve ayarlama belgelerinde bulunan cümleleri kaldırır.Kaldırma işlemi, veri işleme adımında değil, eğitim çalıştırmasının içinde dinamik olarak gerçekleşir. Özel çevirici, bu kaldırma işleminden önce projeye genel bakış bölümünde tümce sayısını raporlar.  

## <a name="length-filter"></a>Uzunluk filtresi
* Cümleleri her iki tarafta yalnızca bir sözcükle kaldırın.
* Her iki tarafta 100 ' ten fazla sözcükten oluşan cümleleri kaldırın.Çince, Japonca, Korece muaf tutulur.
* 3 karakterden kısa bir cümle kaldırın. Çince, Japonca, Korece muaf tutulur.
* Çince, Japonca, Korece için 2000 karakterden uzun cümleler kaldırın.
* %1 ' den az Alfa karakteri olan cümleleri kaldırın.
* 50 'den fazla sözcük içeren sözlük girişlerini kaldırın.

## <a name="white-space"></a>Boşluk
* Sekme ve CR/LF dizileri dahil olmak üzere boşluk karakterlerinden oluşan herhangi bir diziyi tek bir boşluk karakteriyle değiştirin.
* Tümcedeki baştaki veya sondaki boşluğu kaldır

## <a name="sentence-end-punctuation"></a>Tümce sonu noktalama işaretleri
Birden çok cümle sonu noktalama karakterini tek bir örnekle değiştirin.  

## <a name="japanese-character-normalization"></a>Japonca karakter normalleştirme
Tam genişlik ve rakamları yarı genişlik karakterlerine dönüştürür.

## <a name="unescaped-xml-tags"></a>Kaçışsız XML etiketleri
Atlanan olmayan etiketleri kaçış etiketlerine dönüştürür:
* `&lt;`geldiğinde`&amp;lt;`
* `&gt;`geldiğinde`&amp;gt;`
* `&amp;`geldiğinde`&amp;amp;`

## <a name="invalid-characters"></a>Geçersiz karakterler
Özel çevirici, U + FFFD Unicode karakterini içeren cümleleri kaldırır. U + FFFD karakteri, başarısız bir kodlama dönüşümünü gösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Özel çevirmende [bir modeli eğitme](how-to-train-model.md) .
