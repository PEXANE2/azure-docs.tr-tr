---
title: Veri Filtreleme - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Özel bir sistemi eğitmek için kullanılacak belgeleri gönderdiğiniz zaman, belgeler eğitime hazırlanmak için bir dizi işleme ve filtreleme adımından geçer.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 1028443eaaf6c483cd7cd57289b0dcf2a9f11902
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68595913"
---
# <a name="data-filtering"></a>Veri filtreleme

Özel bir sistemi eğitmek için kullanılacak belgeleri gönderdiğiniz zaman, belgeler eğitime hazırlanmak için bir dizi işleme ve filtreleme adımından geçer. Bu adımlar burada açıklanmıştır. Filtreleme bilgisi, özel çevirmende görüntülenen cümle sayısının yanı sıra belgeleri Özel Çevirmen ile eğitime hazırlamak için kendiniz de atabileceğiniz adımları anlamanıza yardımcı olabilir.

## <a name="sentence-alignment"></a>Tümce hizalama
Belgeniz XLIFF, TMX veya ALIGN biçiminde değilse, Özel Çevirmen kaynağınızın cümlelerini hizalar ve belgeleri birbirlerine cümle, cümle ile hizalar. Çevirmen belge hizalama gerçekleştirmez – diğer dilin eşleşen belgesini bulmak için belgelerin adlandırılmasını izler. Belgede, Özel Çevirmen karşılık gelen cümleyi diğer dilde bulmaya çalışır. Hizalama ile yardımcı olmak için gömülü HTML etiketleri gibi belge biçimlendirmekullanır.  

Kaynak ve hedef tarafındaki belgelerdeki cümle sayısı arasında büyük bir tutarsızlık görürseniz, belgeniz ilk etapta paralel olmayabilir veya başka nedenlerden dolayı hizalanamamış olabilir. Belge büyük bir fark (%>10) ile eşler her iki taraftaki cümlelerin gerçekten paralel olduğundan emin olmak için ikinci bir bakış gerektirmesi. Özel Çevirmen, cümle sayısı şüpheli bir şekilde farklıysa belgenin yanında bir uyarı gösterir.  


## <a name="deduplication"></a>Yinelenenleri kaldırma
Özel Çevirmen, test ve tuning belgelerinde bulunan cümleleri eğitim verilerinden kaldırır.Kaldırma, veri işleme adımında değil, eğitim çalışmasının içinde dinamik olarak gerçekleşir. Custom Translator, bu tür kaldırma işleminden önce projeye genel bakışta cümle sayısını size bildirir.  

## <a name="length-filter"></a>Uzunluk filtresi
* Her iki tarafta yalnızca bir sözcük olan cümleleri kaldırın.
* Her iki tarafta 100'den fazla sözcük olan cümleleri kaldırın.Çinliler, Japonca, Koreliler muaftır.
* 3 karakterden az olan cümleleri kaldırın. Çinliler, Japonca, Koreliler muaftır.
* Çince, Japonca, Korece için 2000'den fazla karakteriçeren cümleleri kaldırın.
* %1'den az alfa karaktere sahip cümleleri kaldırın.
* 50'den fazla sözcük içeren sözlük girişlerini kaldırın.

## <a name="white-space"></a>Boşluk
* Sekmeler ve CR/LF dizileri de dahil olmak üzere herhangi bir boşluk karakter dizisini tek bir boşluk karakteriyle değiştirin.
* Cümledeki satır aralığını veya öncü boşluğu kaldırma

## <a name="sentence-end-punctuation"></a>Cümle sonu noktalama işaretleri
Birden çok cümle sonu noktalama işareti karakterlerini tek bir örnekle değiştirin.  

## <a name="japanese-character-normalization"></a>Japonca karakter normalleştirme
Tam genişlikli harfleri ve basamakları yarım genişlikli karakterlere dönüştürün.

## <a name="unescaped-xml-tags"></a>Unescape XML etiketleri
Filtreleme, kaçağı olmayan etiketleri kaçan etiketlere dönüştürür:
* `&lt;`Olur`&amp;lt;`
* `&gt;`Olur`&amp;gt;`
* `&amp;`Olur`&amp;amp;`

## <a name="invalid-characters"></a>Geçersiz karakterler
Custom Translator, Unicode karakteri U+FFFD içeren cümleleri kaldırır. U+FFFD karakteri başarısız bir kodlama dönüşümgösterir.

## <a name="next-steps"></a>Sonraki adımlar

- Özel Çevirmen bir [model eğitin.](how-to-train-model.md)
