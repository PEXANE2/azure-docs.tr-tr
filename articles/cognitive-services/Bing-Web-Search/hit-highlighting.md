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
ms.openlocfilehash: 32c3279cacddf10e77e8d245ba525ab766efd6fd
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96351887"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Metin vurgulamak için düzenleme işaretçileri kullanma

> [!WARNING]
> Bing Arama API'leri bilişsel hizmetlerden Bing Arama hizmetlere taşınıyor. **30 ekim 2020 ' den** itibaren, [burada](/bing/search-apis/bing-web-search/create-bing-search-service-resource)belgelenen işlem sonrasında Bing arama yeni örneklerin sağlanması gerekir.
> Bilişsel hizmetler kullanılarak sağlanan Bing Arama API'leri, sonraki üç yıl boyunca veya Kurumsal Anlaşma sonuna kadar, hangisi önce gerçekleşene kadar desteklenecektir.
> Geçiş yönergeleri için bkz. [Bing arama Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Bing, bazı yanıtların görüntüleme dizelerinde sorgu terimlerini (veya Bing 'in ilgili bulduğu diğer terimleri) işaretleyen isabet vurgulamasını destekler. Örneğin, bir Web sayfası sonucunun `name` , `displayUrl` ve `snippet` alanları işaretli sorgu terimleri içerebilir. 

Varsayılan olarak, Bing, görüntüleme dizelerindeki vurgulama işaretçilerini içermez. İşaretleyicileri etkinleştirmek için, `textDecorations` isteğinize sorgu parametresini ekleyin ve olarak ayarlayın `true` .

## <a name="hit-highlighting-example"></a>İsabet vurgulama örneği

Aşağıdaki örnek için bir Web sonucu gösterir `Sailing Dinghy` . Bing, E000 ve E001 Unicode karakterlerini kullanarak sorgu teriminin başlangıcını ve bitişini işaretledi.
  
![İsabet vurgulama](./media/cognitive-services-bing-web-api/bing-hit-highlighting.png) 

Sonucu Kullanıcı arabiriminizdeki görüntülemeden önce, Unicode karakterleri, görüntüleme biçiminize uygun olanlarla değiştirin.

## <a name="marker-formatting"></a>İşaret biçimlendirme

Bing, Unicode karakterler veya HTML etiketlerini işaretçiler olarak kullanma seçeneği sunar. Hangi işaretçilerin kullanılacağını belirtmek için [TextFormat](/rest/api/cognitiveservices-bingsearch/bing-web-api-v7-reference#textformat) sorgu parametresini de ekleyin: 

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