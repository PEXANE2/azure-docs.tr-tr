---
title: Dil desteği - Computer Vision
titleSuffix: Azure Cognitive Services
description: Bu makalede, Computer Vision özellikleri tarafından desteklenen doğal dillerin bir listesini sağlar; OCR, Metni Tanıyın ve Okuyun.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: a834c68119340d796f87971912a07fc0524a6d21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220143"
---
# <a name="language-support-for-computer-vision"></a>Bilgisayarlı Vizyon için dil desteği

Computer Vision'ın bazı özellikleri birden çok dili destekler; burada belirtilmeyen herhangi bir özellik sadece İngilizce'yi destekler.

## <a name="text-recognition"></a>Metin tanıma

Computer Vision birçok dilde metni tanıyabilir. Özellikle, [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) API çeşitli dilleri desteklerken, [Okuma](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) API'si ve Metin API'sini [Tanıyın](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) yalnızca İngilizce'yi destekler. Bu işlevsellik ve her API'nin avantajları hakkında daha fazla bilgi için [basılı ve el yazısıyla yazılmış metni tanı](concept-recognizing-text.md) yin.

OCR giriş materyalinin dilini otomatik olarak algılar, bu nedenle API çağrısında bir dil kodu belirtmenize gerek yoktur. Ancak, dil kodları her zaman JSON yanıtında `"language"` düğüm değeri olarak döndürülür.

|Dil| Dil kodu | OCR API |
|:-----|:----:|:-----:|
|Arapça | `ar`|✔ |
|Çince (Basitleştirilmiş) | `zh-Hans`|✔ |
|seçenekleri yerine | `zh-Hant`|✔ |
|Çekçe | `cs` |✔ |
|Danca | `da` |✔ |
|Felemenkçe | `nl` |✔ |
|Türkçe | `en` |✔ |
|Fince | `fi` |✔ |
|Fransızca | `fr` |✔ |
|Almanca | `de` |✔ |
|Yunanca | `el` |✔ |
|Macarca | `hu` |✔ |
|İtalyanca | `it` |✔ |
|Japonca | `ja` |✔ |
|Korece | `ko` |✔ |
|Norveççe | `nb` |✔ |
|Lehçe | `pl` |✔ |
|Portekizce | `pt` |✔ |
|Rumence | `ro` |✔ |
|Rusça | `ru` |✔ |
|Sırpça (Kiril) | `sr-Cyrl` |✔ |
|Sırpça (Latin) | `sr-Latn` |✔ |
|Slovakça | `sk` |✔ |
|İspanyolca | `es` |✔ |
|İsveççe | `sw` |✔ |
|Türkçe | `tr` |✔ |

## <a name="image-analysis"></a>Görüntü analizi

Analyze - [Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API'nin bazı eylemleri, `language` sorgu parametresi ile belirtilen diğer dillerde sonuçları döndürebilir. Diğer eylemler, hangi dil inanılsın İngilizce sonuçlar verir ve diğerleri desteklenmeyen diller için bir özel durum oluşturur. Eylemler ve `details` sorgu `visualFeatures` parametreleri ile belirtilir; görüntü analiziyle yapabileceğiniz tüm eylemlerin listesi için [Genel Bakış'a](home.md) bakın.

|Dil | Dil kodu | Kategoriler | Etiketler | Açıklama | Yetişkin | Markalar | Renk | Yüzler | ImageType | Nesneler | Ünlüler | Yer işaretleri |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Çince | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Türkçe | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonca | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portekizce | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|İspanyolca | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda belirtilen Computer Vision özelliklerini kullanmaya başlayın.

* [Yerel bir görüntüyü analiz edin (REST)](./quickstarts/csharp-analyze.md)
* [Yazdırılan metni ayıklama (REST)](./quickstarts/csharp-print-text.md)