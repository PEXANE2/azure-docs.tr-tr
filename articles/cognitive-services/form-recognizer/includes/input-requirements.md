---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 11783f8fac147e6fb2cf371ee1a588cf318a5e9c
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75379459"
---
Form tanıyıcı, bu gereksinimleri karşılayan giriş belgelerinde çalışmaktadır:

* Biçim JPG, PNG, PDF (metin veya taranan) veya TIFF olmalıdır. Metin katıştırılmış PDF 'Ler en iyisidir çünkü karakter ayıklama ve konumunda hata olasılığı yoktur.
* PDF 'niz parola kilitliyse, göndermeden önce kilidi kaldırmanız gerekir.
* PDF ve TIFF belgelerinin 200 sayfa veya daha az olması ve eğitim veri kümesinin toplam boyutunun 500 sayfa veya daha az olması gerekir.
* Görüntüler için Boyutlar 600 x 100 piksel ve 4200 x 4200 piksel arasında olmalıdır.
* Kağıt belgelerinden tarandığında, formlar yüksek kaliteli taramalar olmalıdır.
* Metin Latin alfabesini (Ingilizce karakterler) kullanmalıdır.
* Denetimsiz öğrenme için (etiketlenmemiş veriler olmadan), verilerin anahtar ve değer içermesi gerekir.
* Denetimsiz öğrenme (etiketli veriler olmadan) için, anahtarların üzerinde veya solunda bir değer görünmelidir; Bunlar sağ tarafta veya doğru görünemez.

Form tanıyıcı Şu anda bu tür giriş verilerini desteklemez:

* Karmaşık tablolar (iç içe tablolar, birleştirilmiş üstbilgiler veya hücreler vb.).
* Onay kutuları veya radyo düğmeleri.
