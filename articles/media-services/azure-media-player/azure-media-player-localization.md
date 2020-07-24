---
title: Azure Media Player yerelleştirme
description: Ingilizce olmayan yerel ayarların kullanıcıları için birden çok dil desteği.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: 8a459d1fba4353b4b3e092e83a759314cc455ead
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87087297"
---
# <a name="azure-media-player-localization"></a>Azure Media Player yerelleştirme #

Birden çok dil desteği, Ingilizce olmayan yerel ayarların kullanıcılarına Player ile yerel olarak etkileşime geçmesini sağlar. Azure Media Player, sayfa diline bağlı olarak hata iletilerini yerelleştireceğiniz bir dil sözlüğü ile birlikte örneklendirilecektir. Geliştirici, zorunlu ayarlı bir dille bir oynatıcı örneği de oluşturabilir. Varsayılan dil Ingilizce 'dir (en).

> [!NOTE]
> Bu özellik, bazı sınamalara ve bu nedenle hatalara tabi olmaya devam etmektedir.

```html
    <video id="vid1" class="azuremediaplayer amp-default-skin" data-setup='{"language":"es"}'>...</video>
```

Azure Media Player şu anda ilgili dil kodlarıyla aşağıdaki dilleri desteklemektedir:

| Dil            | Kod | Dil                | Kod   | Dil                | Kod         |
|---------------------|------|-------------------------|--------|-------------------------|--------------|
| İngilizce {varsayılan}   | tr   | Hırvatça                | sa     | Rumence                | ro           |
| Arapça              | Ar   | Macarca               | Hu     | Slovakça                  | sor           |
| Bulgarca           | bg   | Endonezce              | kimlik     | Slovene                 | SL           |
| Katalanca             | yetkilisini   | İzlandaca               | is     | Sırpça (Kiril)      | SR-Cyrl-CS   |
| Çekçe               | 'ye   | İtalyanca                 | içerdiği     | Sırpça (Latin)         | sr-latn-rs   |
| Danca              | kapattığımda   | Japonca                | Sofya     | Rusça                 | ru           |
| Almanca              | seçimini   | Kazakça                  | kk     | İsveççe                 | v           |
| Yunanca               | seri   | Korece                  | dili     | Tayca                    | 11           |
| İspanyolca             | es   | Litvanca              | lt     | Tagalog                 | TL           |
| Estonya Dili            | lale   | Letonca                 | aramasını     | Türkçe                 | tr           |
| Baskça              | yapılan   | Malezya               | SWM     | Ukraynaca               | tr           |
| Harfinin               | belirlediğiniz   | Norveççe-BokmÃ ¥ l     | NB     | Urduca                    | metninizi           |
| Fince             | Fi   | Felemenkçe                   | nl     | Vietnamca              | v           |
| Fransızca              | kesir   | Norveççe-Nynorsk     | nn     | Çince-Basitleştirilmiş    | zh-Hans      |
| Galiçya Dili            | g   | Lehçe                  | pl     | Çince-Geleneksel   | zh-Hant      |
| İbranice              | LIP   | Portekizce - Brezilya     | pt-br  |                         |              |
| Hintçe               | n   | Portekizce - Portekiz   | pt-pt  |                         |              |


> [!NOTE]
> Herhangi bir yerelleştirmenin oluşmasını istemiyorsanız, dili Ingilizce 'ye zorlamanız gerekir

## <a name="next-steps"></a>Sonraki adımlar ##

- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)
