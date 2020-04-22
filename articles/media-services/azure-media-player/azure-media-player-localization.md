---
title: Azure Media Player Yerelleştirme
description: İngilizce olmayan yerel kullanıcılar için birden çok dil desteği.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: 34805c4eaae5d969fc2338c24f9f92404e065d15
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727183"
---
# <a name="localization"></a>Yerelleştirme #

Birden çok dil desteği, İngilizce olmayan yerel kullanıcılara oyuncuyla yerel olarak etkileşim de sağlar. Azure Media Player, sayfa diline dayalı hata iletilerini yerelleştiren genel bir dil sözlüğüyle anında anında gerçekleşir. Geliştirici, zorunlu ayarlanmış bir dile sahip bir oyuncu örneği de oluşturabilir. Varsayılan dil İngilizce (tr) 'dir.

> [!NOTE]
> Bu özellik hala bazı testler geçiyor ve bu nedenle hatalara tabidir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" data-setup='{"language":"es"}'>...</video>
```

Azure Media Player şu anda ilgili dil kodlarıyla aşağıdaki dilleri destekler:

| Dil            | Kod | Dil                | Kod   | Dil                | Kod         |
|---------------------|------|-------------------------|--------|-------------------------|--------------|
| İngilizce {default}   | tr   | Hırvatça                | hr     | Rumence                | Ro           |
| Arapça              | Ar   | Macarca               | Hu     | Slovakça                  | sk           |
| Bulgarca           | Bg   | Endonezce              | id     | Slovence                 | Sl           |
| Katalanca             | Ca   | İzlandaca               | is     | Sırpça - Kiril      | sr-cyrl-cs   |
| Çekçe               | Cs   | İtalyanca                 | bu     | Sırpça - Latince         | sr-latn-rs   |
| Danca              | Savcı   | Japonca                | Ja     | Rusça                 | Ru           |
| Almanca              | de   | Kazakça                  | kk     | İsveççe                 | Sv           |
| Yunanca               | El   | Korece                  | ko     | Tayca                    | inci           |
| İspanyolca             | es   | Litvanca              | lt     | Tanrıkulu                 | Tl           |
| Estonya Dili            | et   | Letonca                 | Lv     | Türkçe                 | tr           |
| Baskça              | Ab   | Malezya               | Bayan     | Ukraynaca               | Ingiltere           |
| Farsça               | Fa   | Norveççe - BokmÃ¥l     | Nb     | Urduca                    | sizin           |
| Fince             | ﬁ   | Felemenkçe                   | Nl     | Vietnamca              | Vı           |
| Fransızca              | Fr   | Norveççe - Nynorsk     | nn     | Çince - basitleştirilmiş    | zh-hans      |
| Galiçya Dili            | Gl   | Lehçe                  | Pl     | Çince - geleneksel   | zh-hant      |
| İbranice              | Hge   | Portekizce - Brezilya     | pt-br  |                         |              |
| Hintçe               | Hu   | Portekizce - Portekiz   | pt-pt  |                         |              |


> [!NOTE]
> Herhangi bir yerelleştirmenin gerçekleşmesini istemiyorsanız, dili İngilizce'ye zorlamalısınız

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)