---
title: Metni vurgulamak için düzenleme işaretçileri kullanma-Bing Web Araması API'si
titleSuffix: Azure Cognitive Services
description: Bing Web Araması API'si kullanarak arama sonuçlarınızda metin süslemelerini nasıl kullanacağınızı ve vurgulamaya nasıl ulaşılacağınızı öğrenin.
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
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "68854048"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Metin vurgulamak için düzenleme işaretçileri kullanma

Bing, bazı yanıtların görüntüleme dizelerinde sorgu terimlerini (veya Bing 'in ilgili bulduğu diğer terimleri) işaretleyen isabet vurgulamasını destekler. Örneğin, bir Web sayfası sonucunun `name` , `displayUrl` ve `snippet` alanları işaretli sorgu terimleri içerebilir. 

Varsayılan olarak, Bing, görüntüleme dizelerindeki vurgulama işaretçilerini içermez. İşaretleyicileri etkinleştirmek için, `textDecorations` isteğinize sorgu parametresini ekleyin ve olarak ayarlayın `true` .

## <a name="hit-highlighting-example"></a>İsabet vurgulama örneği

Aşağıdaki örnek için bir Web sonucu gösterir `Sailing Dinghy` . Bing, E000 ve E001 Unicode karakterlerini kullanarak sorgu teriminin başlangıcını ve bitişini işaretledi.
  
![İsabet vurgulama](./media/cognitive-services-bing-web-api/bing-hit-highlighting.png) 

Sonucu Kullanıcı arabiriminizdeki görüntülemeden önce, Unicode karakterleri, görüntüleme biçiminize uygun olanlarla değiştirin.

## <a name="marker-formatting"></a>İşaret biçimlendirme

Bing, Unicode karakterler veya HTML etiketlerini işaretçiler olarak kullanma seçeneği sunar. Hangi işaretçilerin kullanılacağını belirtmek için [TextFormat](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#textformat) sorgu parametresini de ekleyin: 

| Değer             | İm                       |
|-------------------|------------------------------|
| `textFormat=Raw`  | Unicode karakterler (varsayılan) |
| `textFormat=HTML` | HTML karakterleri              |

## <a name="additional-text-decorations"></a>Ek metin süslemeleri

Bing birkaç farklı metin süslede döndürebilir. Örneğin, bir `Computation` Yanıt alandaki sorgu teriminin alt simge işaretçilerini içerebilir `log(2)` `expression` .

![hesaplama işaretçileri](./media/cognitive-services-bing-web-api/bing-markers-computation.png) 

İstek süslemeleri belirtmediyseniz, `expression` alanı içerir `log10(2)` . 

`textDecorations`İse `true` , Bing yanıtların görüntüleme dizelerinde aşağıdaki işaretçileri içerebilir. Eşdeğer HTML etiketi yoksa tablo hücresi boştur.

|Unicode|HTML|Açıklama
|-|-|-
|U + E000|\<b>|Sorgu teriminin başlangıcını (isabet vurgulama) işaretler
|U + E001|\</b>|Sorgu teriminin sonunu işaretler
|U + E002|\<i>|İtalik içeriğin başlangıcını işaretler 
|U + E003|\</i>|İtalik içeriğin sonunu işaretler
|U + E004|\<br/>|Satır kesmeyi işaretler
|U + E005||Telefon numarasının başlangıcını işaretler
|U + E006||Telefon numarasının sonunu işaretler
|U + E007||Bir adresin başlangıcını işaretler
|U + E008||Bir adresin sonunu işaretler
|U + E009|\&nbsp;|Bölünmez bir boşluk işaretler
|U + E00C|\<strong>|Kalın içeriğin başlangıcını işaretler
|U + E00D|\</strong>|Kalın içeriğin sonunu işaretler
|U + E00E||Arka planının çevreleyen arka planıyla daha açık olması gereken içeriğin başlangıcını işaretler
|U + E00F||Arka planının çevreleyen arka planıyla daha açık olması gereken içeriğin sonunu işaretler
|U + E010||Arka planı çevreleyen arka planıyla daha koyu olması gereken içeriğin başlangıcını işaretler
|U + e011||Arka planı çevreleyen arka planıyla daha koyu olması gereken içeriğin sonunu işaretler
|U + E012|\<del>|Çizili olması gereken içeriğin başlangıcını işaretler
|U + E013|\</del>|Çizili olması gereken içeriğin sonunu işaretler
|U + E016|\<sub>|Alt simge içeriğinin başlangıcını işaretler
|U + E017|\</sub>|Alt indis içeriğinin sonunu işaretler
|U + E018|\<sup>|Üst simge içeriğinin başlangıcını işaretler
|U + E019|\</sup>|Üst simge içeriğinin sonunu işaretler

## <a name="next-steps"></a>Sonraki adımlar

* [Bing Web Araması API'si nedir?](overview.md) 
* [Küçük resimleri yeniden boyutlandırma ve kırpma](resize-and-crop-thumbnails.md)