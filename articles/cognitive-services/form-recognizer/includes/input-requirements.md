---
author: laujan
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: lajanuar
ms.openlocfilehash: 324fde5198fe3469e3979cf5cd102715118a49aa
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "103467330"
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
