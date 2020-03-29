---
title: Metni vurgulamak için dekorasyon işaretçileri nasıl kullanılır - Bing Web Arama API
titleSuffix: Azure Cognitive Services
description: Bing Web Arama API'sini kullanarak metin süslemelerini ve arama sonuçlarınızda vurgulamayı nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: 5365B568-EA55-4D97-8FBE-0AF60158D4D5
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 07/30/2019
ms.author: scottwhi
ms.openlocfilehash: a6d394fec6e7cf0a230f61ad05c236a1f84dad9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854048"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Metni vurgulamak için süsleme işaretçilerini kullanma

Bing, bazı yanıtların görüntü dizelerinde sorgu terimlerini (veya Bing'in alakalı bulduğu diğer terimleri) işaretleyen isabet vurgulamayı destekler. Örneğin, bir web sayfası `name`sonucunun `displayUrl` `snippet` , ve alanlar işaretli sorgu terimleri içerebilir. 

Varsayılan olarak Bing, görüntü dizelerinde vurgulama işaretçilerini içermez. İşaretçileri etkinleştirmek için, isteğinize sorgu `textDecorations` parametresini `true`ekleyin ve '' olarak ayarlayın.

## <a name="hit-highlighting-example"></a>Hit vurgulama örneği

Aşağıdaki örnekte `Sailing Dinghy`bir web sonucu nu gösterir. Bing, E000 ve E001 Unicode karakterlerini kullanarak sorgu döneminin başlangıcını ve sonunu işaretledi.
  
![Hit Vurgulama](./media/cognitive-services-bing-web-api/bing-hit-highlighting.png) 

Sonucu kullanıcı arabiriminizde görüntülemeden önce, Unicode karakterlerini ekran biçiminize uygun olanlarla değiştirin.

## <a name="marker-formatting"></a>İşaretleyici biçimlendirme

Bing, Unicode karakterlerini veya HTML etiketlerini işaretleyici olarak kullanma seçeneği sunar. Hangi işaretçilerin kullanılacağını belirtmek için [textFormat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#textformat) sorgu parametresini ekleyin: 

| Değer             | Işaretleyici                       |
|-------------------|------------------------------|
| `textFormat=Raw`  | Unicode karakterleri (varsayılan) |
| `textFormat=HTML` | HTML karakterleri              |

## <a name="additional-text-decorations"></a>Ek metin süslemeleri

Bing birkaç farklı metin süslemeleri döndürebilir. Örneğin, bir `Computation` yanıt `log(2)` `expression` alandaki sorgu terimi için alt komut işaretleri içerebilir.

![hesaplama işaretleri](./media/cognitive-services-bing-web-api/bing-markers-computation.png) 

İstek süslemeleri belirtmediyse, `expression` alan . `log10(2)` 

`textDecorations` Varsa, `true`Bing yanıtların görüntü dizelerinde aşağıdaki işaretçileri içerebilir. Eşdeğer HTML etiketi yoksa, tablo hücresi boştur.

|Unicode|HTML|Açıklama
|-|-|-
|U+E000|\<b>|Sorgu teriminin başlangıcını işaretler (isabet vurgulama)
|U+E001|\</b>|Sorgu döneminin sonunu işaretler
|U+E002|\<i>|Italik leştirilmiş içeriğin başlangıcını işaretler 
|U+E003|\</i>|Italikleştirilmiş içeriğin sonunu işaretler
|U+E004|\<br/>|Satır sonu işaretler
|U+E005||Telefon numarasının başlangıcını işaretler
|U+E006||Telefon numarasının sonunu işaretler
|U+E007||Adresin başlangıcını işaretler
|U+E008||Adresin sonunu işaretler
|U+E009|\&nbsp;|Kırılmayan bir alanı işaretler
|U+E00C|\<güçlü>|Kalın içeriğin başlangıcını işaretler
|U+E00D|\</güçlü>|Kalın içeriğin sonunu işaretler
|U+E00E||Arka planı çevresindeki arka plandan daha hafif olması gereken içeriğin başlangıcını işaretler
|U+E00F||Arka planı çevresindeki arka plandan daha hafif olması gereken içeriğin sonunu işaretler
|U+E010||Arka planı çevresindeki arka plandan daha koyu olması gereken içeriğin başlangıcını işaretler
|U+E011||Arka planı çevresindeki arka plandan daha koyu olması gereken içeriğin sonunu işaretler
|U+E012|\<del>|Üzerinden vurulması gereken içeriğin başlangıcını işaretler
|U+E013|\</del>|Üzerinden alınması gereken içeriğin sonunu işaretler
|U+E016|\<alt>|Alt yazı içeriğinin başlangıcını işaretler
|U+E017|\</alt>|Alt yazı içeriğinin sonunu işaretler
|U+E018|\<sup>|Üst yazı içeriğinin başlangıcını işaretler
|U+E019|\</sup>|Üst yazı içeriğinin sonunu işaretler

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Web Arama API'si nedir?](overview.md) 
* [Küçük resimleri yeniden boyutlandırma ve kırpma](resize-and-crop-thumbnails.md)