---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 17dc32f8948387b90229d3c4c07102cff98e3018
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562681"
---
Form tanıyıcı, bu gereksinimleri karşılayan giriş belgelerinde çalışmaktadır:

* Biçim JPG, PNG veya PDF olmalıdır (metin veya taranan). Metin katıştırılmış PDF 'Ler en iyisidir çünkü karakter ayıklama ve konumunda hata olasılığı yoktur.
* PDF 'niz parola kilitliyse, göndermeden önce kilidi kaldırmanız gerekir.
* Dosya boyutu 4 MB 'tan az olmalıdır.
* Görüntüler için Boyutlar 50 x 50 piksel ve 4200 x 4200 piksel arasında olmalıdır.
* Kağıt belgelerinden tarandığında, formlar yüksek kaliteli taramalar olmalıdır.
* Metin Latin alfabesini (Ingilizce karakterler) kullanmalıdır.
* Veriler, anahtar ve değer içermelidir.
* Anahtarlar, değerlerin solunda veya solunda görünebilir, ancak aşağıdan veya sağında yer alabilir.

Form tanıyıcı Şu anda bu tür giriş verilerini desteklemez:

* Karmaşık tablolar (iç içe tablolar, birleştirilmiş üstbilgiler veya hücreler vb.).
* Onay kutuları veya radyo düğmeleri.
* 50 sayfadan daha uzun PDF belgeleri.