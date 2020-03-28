---
title: Mürekkep Tanıyıcısı Nedir? - Mürekkep Tanıyıcı API
titleSuffix: Azure Cognitive Services
description: Mürekkep darbesi verilerinin tanımlanıp giriş olarak kullanılmasına izin vermek için Mürekkep Tanıyılayıcı'yı uygulamalarınız, web sitelerine, araçlarınıza ve diğer çözümlerinize entegre edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: overview
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: 2d00a489af35f5960aaeb1dda7add5337d48d57d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75448162"
---
# <a name="what-is-the-ink-recognizer-api"></a>Mürekkep Tanıma API’si nedir?


Mürekkep Algılayıcı Bilişsel Hizmeti, dijital mürekkep içeriğini analiz etmek ve tanımak için bulut tabanlı bir REST API sağlar. Optik Karakter Tanıma (OCR) kullanan hizmetlerin aksine, API giriş olarak dijital mürekkep darbesi verilerini gerektirir. Dijital mürekkep darbeleri, dijital kalemler veya parmaklar gibi giriş araçlarının hareketini temsil eden 2B noktalardan (X,Y koordinatları) zaman sıralı kümelerdir. Daha sonra girişteki şekilleri ve el yazısı içeriği tanır ve tüm tanınan varlıkları içeren bir JSON yanıtı döndürür.

![API'ye mürekkep darbesi girişi göndermeyi açıklayan bir akış şeması](media/ink-recognizer-pen-graph.svg)

## <a name="features"></a>Özellikler

Mürekkep Tanıyıcısı API ile uygulamalarınızda el yazısı içeriği kolayca tanıyabilirsiniz. 

|Özellik  |Açıklama  |
|---------|---------|
| El yazısı tanıma | 63 temel dil ve yerel olarak el yazısı içeriği [tanıyın.](language-support.md) | 
| Düzen tanıma | Dijital mürekkep içeriği hakkında yapısal bilgiler alın. İçeriği yazı bölgelerine, paragraflara, satırlara, sözcüklere, madde işaretli listelere ayırın. Uygulamalarınız daha sonra otomatik liste biçimlendirme ve şekil hizalama gibi ek özellikler oluşturmak için düzen bilgilerini kullanabilir. |
| Şekil tanıma | Not alırken en sık kullanılan [geometrik şekilleri](concepts/send-ink-data.md#shapes-recognized-by-the-ink-recognizer-api) tanıyın. |
| Birleştirilmiş şekiller ve metin tanıma | Hangi mürekkep konturlarının şekillere veya el yazısı içeriğe ait olduğunu tanıyın ve bunları ayrı ayrı sınıflandırın.|

## <a name="workflow"></a>İş akışı

Mürekkep Recognizer API, HTTP isteklerini ve ayrıştırma JSON yapabilirsiniz herhangi bir programlama dilinden aramak kolay hale, bir RESTful web hizmetidir.

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

Kaydolduktan sonra:

1. Mürekkep darbesi verilerinizi alın ve geçerli JSON olarak [biçimlendirin.](concepts/send-ink-data.md#sending-ink-data)
1. Verilerinizle birlikte Mürekkep Tanıyıcısı API'sine bir istek gönderin.
1. Döndürülen JSON iletisini ayrıştırarak API yanıtını işleyin.

## <a name="next-steps"></a>Sonraki adımlar

Mürekkep Tanıyıcısı API'ye çağrı yapmaya başlamak için aşağıdaki dillerde hızlı bir başlangıç yapmayı deneyin.
* [C #](quickstarts/csharp.md)
* [Java](quickstarts/java.md)
* [Javascript](quickstarts/javascript.md)

Mürekkep Tanıma API'sinin dijital mürekkep uygulamasında nasıl çalıştığını görmek için GitHub'daki aşağıdaki örnek uygulamalara göz atın:
* [C# Evrensel Windows Platformu (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [JavaScript web tarayıcı uygulaması](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Java ve Android mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Swift ve iOS mobil uygulaması](https://go.microsoft.com/fwlink/?linkid=2089805)
