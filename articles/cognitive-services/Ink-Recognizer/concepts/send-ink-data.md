---
title: Mürekkep Tanıma API’sine mürekkep verisi gönderme
titleSuffix: Azure Cognitive Services
description: Farklı uygulamalar için Mürekkep Çözümleyici api'sini arama hakkında bilgi edinin
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 0ad961495d44f13522a3c02224a5612aaedaf076
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79221111"
---
# <a name="send-ink-data-to-the-ink-recognizer-api"></a>Mürekkep Tanıma API’sine mürekkep verisi gönderme 

Dijital mürekkep oluşturma, el yazısı ve çizim gibi girdilerin dijital sunumunun yapılmasını sağlayan teknolojilere başvurur. Bu genellikle bir kalem gibi giriş aygıtlarının hareketlerini yakalayan bir sayısallaştırıcı kullanılarak elde edilir. Cihazlar zengin dijital mürekkep oluşturma deneyimleri sunmaya devam ederken yapay zeka ve makine öğrenimi, yazılı metin ve şekillerin tüm bağlamlarda tanınmasını sağlar. Mürekkep Tanıyıcı API'sı mürekkep darbeleri göndermenizi ve bunlar hakkında ayrıntılı bilgi almanızı sağlar. 

## <a name="the-ink-recognizer-api-vs-ocr-services"></a>Mürekkep tanıma API ve OCR hizmetleri

Mürekkep Ayırıcı API Optik Karakter Tanıma (OCR) kullanmaz. OCR hizmetleri, el yazısı ve metin tanıma sağlamak için görüntülerdeki piksel verilerini işler. Bu bazen çevrimdışı tanıma olarak adlandırılır. Bunun yerine, Mürekkep Recognizer API giriş aygıtı kullanılırken yakalanan dijital mürekkep kontur verileri gerektirir. Dijital mürekkep verilerinin bu şekilde işlenmesi, OCR hizmetlerine kıyasla daha doğru tanıma sonuçları üretebilir. 

## <a name="sending-ink-data"></a>Mürekkep verisi gönderme

Mürekkep Ayırıcı API, algılama yüzeyine dokunduğu andan kaldırıldığı zamana kadar bir giriş aygıtı tarafından oluşturulan mürekkep konturlarını temsil eden X ve Y koordinatlarını gerektirir. Her konturun noktaları virgülle ayrılmış değerler dizisi olmalı ve aşağıdaki örnekte olduğu gibi JSON olarak biçimlendirilmelidir. Ayrıca, her mürekkep konturunu her istekte benzersiz bir kimlik olmalıdır. Kimlik aynı istek içinde yinelenirse, API bir hata döndürecektir. En doğru tanıma sonuçları için ondalık noktadan sonra en az sekiz basamak vardır. Tuvalin menşei (0,0) mürekkep tuvalinin sol üst köşesi olarak kabul edilir.

> [!NOTE]
> Aşağıdaki örnek geçerli Değildir JSON. [GitHub'da](https://go.microsoft.com/fwlink/?linkid=2089909)tam mürekkep recognizer JSON isteği bulabilirsiniz.
 
```json
{
  "language": "en-US",
  "strokes": [
   {
    "id": 43,
    "points": 
        "5.1365, 12.3845,
        4.9534, 12.1301,
        4.8618, 12.1199,
        4.7906, 12.2217,
        4.7906, 12.5372,
        4.8211, 12.9849,
        4.9534, 13.6667,
        5.0958, 14.4503,
        5.3299, 15.2441,
        5.6555, 16.0480,
        ..."
   },
    ...
  ]
}
```

## <a name="ink-recognizer-response"></a>Mürekkep Tanıyan yanıt

Mürekkep Tanıyan API, mürekkep içeriğinden tanıdığı nesneler hakkında bir çözüm yanıtı verir. Yanıt, farklı mürekkep konturları arasındaki ilişkileri açıklayan tanıma birimleri içerir. Örneğin, farklı, ayrı şekiller oluşturan konturlar farklı birimlerde bulunur. Her birim, tanınan nesne, koordinatları ve diğer çizim öznitelikleri de dahil olmak üzere mürekkep konturları hakkında ayrıntılı bilgi içerir.

## <a name="shapes-recognized-by-the-ink-recognizer-api"></a>Mürekkep Tanıyıcısı API tarafından tanınan şekiller

Mürekkep Ayırıcı API not alma en sık kullanılan şekilleri belirleyebilir. Aşağıdaki resimde bazı temel örnekler gösterilmektedir. API tarafından tanınan şekillerin ve diğer mürekkep içeriğinin tam listesi için [API başvuru makalesine](https://go.microsoft.com/fwlink/?linkid=2089907)bakın. 

![Mürekkep Tanıyıcısı API tarafından tanınan şekillerin listesi](../media/shapes.png)

## <a name="recommended-calling-patterns"></a>Önerilen arama desenleri

Mürekyp Recognizer REST API'yi uygulamanıza göre farklı desenlerde arayabilirsiniz. 

### <a name="user-initiated-api-calls"></a>Kullanıcı başlatılan API aramaları

Kullanıcı girişi (örneğin, not alma veya ek açıklama uygulaması) alan bir uygulama oluşturuyorsanız, mürekkep tanıma apisine ne zaman ve hangi mürekçenin gönderilemeyeceğini onlara kontrol etmek isteyebilirsiniz. Bu işlevsellik, metin ve şekillerin her ikisi de tuvalde bulunduğunda ve kullanıcılar her biri için farklı eylemler gerçekleştirmek istediğinde özellikle yararlıdır. Kullanıcıların API'ye ne göndereceğini seçmelerini sağlayan seçim özellikleri (kement veya diğer geometrik seçim aracı gibi) eklemeyi düşünün.  

### <a name="app-initiated-api-calls"></a>Uygulama başlatılan API aramaları

Uygulamanızın bir zaman aramadan sonra Mürekkep Tanıyıcısı API'yi aramasını da sağlayabilirsiniz. Geçerli mürekkep konturlarını düzenli olarak API'ye göndererek, API'nin yanıt süresini artırırken tanıma sonuçlarını oluşturuldukları gibi depolayabilirsiniz. Örneğin, kullanıcınızın bunu tamamladığını algıladıktan sonra API'ye el yazısı yla yazılmış bir metin satırı gönderebilirsiniz. 

Tanıma sonuçlarının önceden açıklanması, mürekkep konturlarının birbirleriyle ilişkili olduğu gibi özellikleri hakkında bilgi verir. Örneğin, hangi konturlar aynı sözcük, satır, liste, paragraf veya şekil oluşturmak üzere gruplandırılır. Bu bilgiler, örneğin kontur gruplarını aynı anda seçerek uygulamanızın mürekkep seçimi özelliklerini geliştirebilir.

## <a name="integrate-the-ink-recognizer-api-with-windows-ink"></a>Mürekkep Tanıyıcı API'sini Windows Mürekbiyle tümleştirme

[Windows Mürekktürü,](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions) çeşitli cihazlarda dijital mürekkep deneyimi sağlamak için araçlar ve teknolojiler sağlar. Dijital mürekkep konturlarını görüntüleyen ve yorumlayan uygulamalar oluşturmak için Windows Mürekkep platformunu Mürekkep Tanıyıcı API ile birleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Mürekkep Tanıma API’si nedir?](../overview.md)
* [Mürekkep Tanıyan REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2089907)

* Dijital mürekkep darbesi verilerini şu kullanarak göndermeye başlayın:
    * [C #](../quickstarts/csharp.md)
    * [Java](../quickstarts/java.md)
    * [Javascript](../quickstarts/javascript.md)
