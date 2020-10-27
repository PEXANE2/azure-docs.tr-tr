---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: b3d24ce4dd42ea8122610943379160ca93406c21
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92548104"
---
Form tanıyıcı, bu gereksinimleri karşılayan giriş belgelerinde çalışmaktadır:

* Biçim JPG, PNG, PDF (metin veya taranan) veya TIFF olmalıdır. Metin katıştırılmış PDF 'Ler en iyisidir çünkü karakter ayıklama ve konumunda hata olasılığı yoktur.
* Dosya boyutu 50 MB 'tan az olmalıdır.
* Resim boyutları 50 x 50 piksel ve 10000 x 10000 piksel arasında olmalıdır.
* PDF boyutları, yasal veya A3 kağıt boyutlarına ve daha küçük boyutlara karşılık gelen en fazla 17 x 17 inç olmalıdır.
* PDF ve TIFF için yalnızca ilk 200 sayfa işlenir (ücretsiz bir katman aboneliğiyle yalnızca ilk iki sayfa işlenir).
* Eğitim veri kümesinin toplam boyutu 500 sayfa veya daha az olmalıdır.
* PDF 'niz parola kilitliyse, göndermeden önce kilidi kaldırmanız gerekir.
* Kağıt belgelerinden tarandığında, formlar yüksek kaliteli taramalar olmalıdır.
* Metin Latin alfabesini (Ingilizce karakterler) kullanmalıdır.
* Denetimsiz öğrenme için (etiketlenmemiş veriler olmadan), verilerin anahtar ve değer içermesi gerekir.
* Denetimsiz öğrenme (etiketli veriler olmadan) için, anahtarların üzerinde veya solunda bir değer görünmelidir; Bunlar sağ tarafta veya doğru görünemez.

Form tanıyıcı Şu anda bu tür giriş verilerini desteklemez:

* Karmaşık tablolar (iç içe tablolar, birleştirilmiş üstbilgiler veya hücreler vb.).
* Onay kutuları veya radyo düğmeleri.
