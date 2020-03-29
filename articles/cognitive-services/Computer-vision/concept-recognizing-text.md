---
title: Basılı, el yazısı metin tanıma - Bilgisayar Vizyonu
titleSuffix: Azure Cognitive Services
description: Bilgisayar Lı Vizyon API'sini kullanarak resimlerde basılı ve el yazısı yla yazılmış metinlerin tanınmasıile ilgili kavramlar.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: a4c90ed12c8023e0b9ebc509b20d8d9224b49f1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220165"
---
# <a name="recognize-printed-and-handwritten-text"></a>Yazdırılmış ve el yazısı ile yazılan metinleri tanıma

Computer Vision, görüntülerde görünen basılı veya el yazısı metni algılayan ve ayıklayan bir dizi hizmet sağlar. Bu, not alma, tıbbi kayıtlar, güvenlik ve bankacılık gibi çeşitli senaryolarda yararlıdır. Aşağıdaki üç bölümde, her biri farklı kullanım örnekleri için optimize edilmiş üç farklı metin tanıma API'si ayrıntılı olarak açıklanır.

## <a name="read-api"></a>API'yi okuyun

Read API, en son tanıma modellerimizi kullanarak görüntüdeki metin içeriğini algılar ve tanımlanan metni makine tarafından okunabilir bir karakter akışına dönüştürür. Metin ağırlıklı görüntüler (dijital olarak taranmış belgeler gibi) ve çok fazla görsel gürültüye sahip görüntüler için optimize edilmiştir. Her metin satırı için hangi tanıma modelinin kullanılacağını belirleyecek ve görüntüleri hem basılı hem de el yazısıyla yazılmış metinlerle destekler. Daha büyük belgelerin bir sonucu döndürmesi birkaç dakika sürebileceğinden, Read API eş senkronize olarak yürütülür.

Read işlemi, çıktısında tanınan sözcüklerin özgün satır gruplarını tutar. Her satır sınırlayıcı kutu koordinatları ile birlikte gelir ve satır içindeki her sözcüğün de kendi koordinatları vardır. Bir sözcük düşük güvenle tanınıyorsa, bu bilgi de iletilir. Daha fazla bilgi edinmek için [OKUMA API başvuru dokümanlarına](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) bakın ve [API Önizleme başvuru dokümanlarını okuyun.](https://go.microsoft.com/fwlink/?linkid=2118322)

> [!NOTE]
> Bu özellik yalnızca İngilizce ve İspanyolca (önizleme) metinler için kullanılabilir.

### <a name="image-requirements"></a>Görüntü gereksinimleri

Read API, aşağıdaki gereksinimleri karşılayan görüntülerle çalışır:

- Görüntü JPEG, PNG, BMP, PDF veya TIFF formatında sunulmalıdır.
- Görüntünün boyutları 50 x 50 ile 10000 x 10000 piksel arasında olmalıdır. PDF sayfaları 17 x 17 inç veya daha küçük olmalıdır.
- Görüntünün dosya boyutu 20 megabayttan (MB) az olmalıdır.

### <a name="limitations"></a>Sınırlamalar

Ücretsiz katman aboneliği kullanıyorsanız, Okuma API'si yalnızca PDF veya TIFF belgesinin ilk iki sayfasını işler. Ücretli abonelikle birlikte, 200 sayfaya kadar işlenir. Ayrıca, API'nin sayfa başına en fazla 300 satır algıladığını unutmayın.

## <a name="ocr-optical-character-recognition-api"></a>OCR (optik karakter tanıma) API'si

Computer Vision'ın optik karakter tanıma (OCR) API'si Read API'sine benzer, ancak eşzamanlı olarak yürütülür ve büyük belgeler için optimize edilmez. Daha önceki bir tanıma modeli kullanır, ancak daha fazla dille çalışır; desteklenen dillerin tam listesi için [Dil desteğine](language-support.md#text-recognition) bakın.

Gerekirse, OCR yatay görüntü ekseni hakkında derece lerde dönme ofset döndürerek tanınan metnin dönüşünü düzeltir. OCR ayrıca aşağıdaki resimde görüldüğü gibi, her sözcüğün çerçeve koordinatlarını sağlar.

![Döndürülen ve metni okunan ve resmedilen bir görüntü](./Images/vision-overview-ocr.png)

Daha fazla bilgi edinmek için [OCR başvuru dokümanlarına](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) bakın.

### <a name="image-requirements"></a>Görüntü gereksinimleri

OCR API, aşağıdaki gereksinimleri karşılayan görüntüler üzerinde çalışır:

* Görüntü JPEG, PNG, GIF veya BMP biçiminde sunulmalıdır.
* Giriş görüntüsünün boyutu 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır.
* Görüntüdeki metin 90 derecenin herhangi bir katı artı 40 dereceye kadar küçük bir açı ile döndürülebilir.

### <a name="limitations"></a>Sınırlamalar

Metnin baskın olduğu fotoğraflarda, kısmen tanınan sözcüklerden yanlış pozitif sonuçlar gelebilir. Bazı fotoğraflarda, özellikle de metin içermeyen fotoğraflarda, görüntünün türüne bağlı olarak hassasiyet değişebilir.

## <a name="recognize-text-api"></a>Metin API'yi Tanıyın

> [!NOTE]
> Metin API'yi Tanıyın, Okuma API'si lehine amortismana uğruyor. Read API benzer özelliklere sahiptir ve PDF, TIFF ve çok sayfalı dosyaları işlemek için güncelleştirilir.

Metin API'yi TanıOCR'a benzer, ancak eş senkronize olarak yürütür ve güncelleştirilmiş tanıma modellerini kullanır. Daha fazla bilgi edinmek için [Metin API başvuru dokümanlarını tanı](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) yat.

### <a name="image-requirements"></a>Görüntü gereksinimleri

Metin API'yi Tanıyın, aşağıdaki gereksinimleri karşılayan görüntülerle çalışır:

- Görüntü JPEG, PNG veya BMP biçiminde sunulmalıdır.
- Görüntünün boyutları 50 x 50 ile 4200 x 4200 piksel arasında olmalıdır.
- Görüntünün dosya boyutu 4 megabayttan (MB) az olmalıdır.

## <a name="limitations"></a>Sınırlamalar

Metin tanıma işlemlerinin doğruluğu görüntülerin kalitesine bağlıdır. Aşağıdaki etkenler yanlış bir okumaya neden olabilir:

* Bulanık görüntüler.
* El yazısı veya bitişik el yazısı metin.
* Artistik yazı tipi stilleri.
* Küçük metin boyutu.
* Karmaşık arka planlar, gölgeler ya da metnin üzerinde parlama veya perspektif bozukluk.
* Kelimelerin başında büyük harf veya eksik harfler.
* Alt simge, üst simge veya üstü çizili metin.

## <a name="next-steps"></a>Sonraki adımlar

Basit bir C# uygulamasında metin tanımayı uygulamak için [Extract metnini (Oku)](./QuickStarts/CSharp-hand-text.md) hızlı bir şekilde izleyin.
