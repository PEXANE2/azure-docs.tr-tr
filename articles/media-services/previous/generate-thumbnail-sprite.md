---
title: Azure Medya Hizmetleri ile küçük resim sprite oluşturma | Microsoft Dokümanlar
description: Bu konu, Azure Medya Hizmetleri ile küçük resim sprite'ı nasıl oluşturacağınızı gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: e5d32d1bc3bd704b03e58c62251a323ed3f4662c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61229059"
---
# <a name="generate-a-thumbnail-sprite"></a>Küçük resim görüntüsü oluşturma  

Bir VTT dosyasıyla birlikte, tek bir (büyük) görüntüde birleştirilmiş birden çok küçük çözünürlüklü küçük resim içeren bir JPEG dosyası olan küçük resim sprite oluşturmak için Media Encoder Standard'ı kullanabilirsiniz. Bu VTT dosyası, her küçük resmin temsil ettiği giriş videosundaki zaman aralığını, büyük JPEG dosyasındaki küçük resmin boyutu ve koordinatlarıyla birlikte belirtir. Video oynatıcılar VTT dosyasını ve sprite görüntüsünü kullanarak bir 'görsel' arama çubuğu gösterir ve görüntüleyiciye video zaman çizelgesi boyunca ileri geri ovma yaparken görsel geri bildirim sağlar.

Thumbnail Sprite, önceden ayarlanmış oluşturmak için Media Encoder Standard kullanmak için:

1. JPG küçük resim görüntü biçimini kullanmalısınız
2. Başlat/Adım/Aralık değerlerini zaman damgası veya % değerleri (çerçeve sayıları değil) olarak belirtmeli 
    
    1. Zaman damgalarını ve % değerlerini karıştırmak sorun değildir

3. Negatif olmayan bir sayı 1'den büyük veya eşit olarak SpriteColumn değerine sahip olmalıdır

    1. SpriteColumn M >= 1 olarak ayarlanmışsa, çıkış görüntüsü M sütunlu bir dikdörtgendir. #2 üzerinden oluşturulan küçük resim sayısı M'nin tam bir katı değilse, son satır eksik olur ve siyah piksellerle bırakılır.  

Örnek aşağıda verilmiştir:

```json
{
    "Version": 1.0,
    "Codecs": [
    {
      "Start": "00:00:01",
      "Type": "JpgImage",
      "Step": "5%",
      "Range": "100%",
      "JpgLayers": [
        {
          "Type": "JpgLayer",
          "Width": "10%",
          "Height": "10%",
          "Quality": 90
        }
      ],
      "SpriteColumn": 10
    }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
   ]
}
```

## <a name="known-issues"></a>Bilinen Sorunlar

1.  Tek bir görüntü satırı içeren sprite görüntü oluşturmak mümkün değildir (SpriteColumn = 1 tek bir sütunlu bir görüntüyle sonuçlanır).
2.  Sprite görüntülerinin orta büyüklükteki JPEG görüntülerine parçalanması henüz desteklenmez. Bu nedenle, dikkat küçük resimlerin sayısını ve boyutlarını sınırlamak için alınmalıdır, böylece ortaya çıkan dikişli Küçük Resim Sprite yaklaşık 8M piksel veya daha az.
3.  Azure Media Player, Microsoft Edge, Chrome ve Firefox tarayıcılarında sprites destekler. VTT ayrıştırma IE11'de desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

[İçerik kodlama](media-services-encode-asset.md)
