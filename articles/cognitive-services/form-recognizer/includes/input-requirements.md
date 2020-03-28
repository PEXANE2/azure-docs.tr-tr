---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 11783f8fac147e6fb2cf371ee1a588cf318a5e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75379459"
---
Form Tanıyıcısı, bu gereksinimleri karşılayan giriş belgeleri üzerinde çalışır:

* Biçim JPG, PNG, PDF (metin veya taranmış) veya TIFF olmalıdır. Metin lekştirilen PDF'ler en iyisidir, çünkü karakter ayıklama ve konumda hata olasılığı yoktur.
* PDF'leriniz parolayla kilitlenmişse, göndermeden önce kilidi kaldırmanız gerekir.
* PDF ve TIFF belgeleri 200 sayfa veya daha az olmalı ve eğitim veri kümesinin toplam boyutu 500 sayfa veya daha az olmalıdır.
* Görüntüler için boyutlar 600 x 100 piksel ile 4200 x 4200 piksel arasında olmalıdır.
* Kağıt belgelerden taranırsa, formlar yüksek kaliteli taramalar olmalıdır.
* Metin Latin alfabesini (İngilizce karakterler) kullanmalıdır.
* Denetimsiz öğrenme için (etiketli veriler olmadan), verilerin anahtarları ve değerleri içermesi gerekir.
* Denetimsiz öğrenme için (etiketli veriler olmadan), anahtarlar değerlerin üstünde veya solunda görünmelidir; aşağıda veya sağda görüntülenemez.

Form Tanıyıcısı şu anda bu tür giriş verilerini desteklemiyor:

* Karmaşık tablolar (iç içe tablolar, birleştirilmiş üstbilgi veya hücreler vb.)
* Onay kutuları veya radyo düğmeleri.
