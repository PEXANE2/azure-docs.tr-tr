---
title: Hata Ayıklama Oluşturma
description: Sunucu tarafı hata ayıklama işleme efektlerine genel bakış
author: jumeder
ms.author: jumeder
ms.date: 04/09/2020
ms.topic: article
ms.openlocfilehash: 675f8d988e64ed7b556f154f681ccb53ed1000c6
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394299"
---
# <a name="debug-rendering"></a>Hata Ayıklama Oluşturma

Hata ayıklama API'si, sunucu tarafı oluşturmayı farklı hata ayıklama efektleriyle değiştirmek için bir dizi genel seçenek sağlar.

## <a name="available-debug-rendering-effects"></a>Kullanılabilir hata ayıklama efektleri

|Ayar                          | Etki                               |
|---------------------------------|:-------------------------------------|
|Çerçeve sayacı                    | Çerçevenin sol üst köşesine bir metin kaplaması işler. Metin, işleme gelirleri olarak sürekli olarak artımlı olan geçerli sunucu tarafı çerçeve kimliğini gösterir. |
|Çokgen sayısı                    | Çerçevenin sol üst köşesine bir metin kaplaması işler. Metin, [sunucu tarafı performans sorguları](performance-queries.md) tarafından sorgulanan aynı değerolan çokgenlerin şu anda işlenen miktarını gösterir| 
|Tel kafes                        | Etkinleştirilirse, sunucuya yüklenen tüm nesne geometrisi tel kafes modunda işlenir. Bu modda sadece çokgenlerin kenarları rasterized olacaktır. |

Aşağıdaki kod bu hata ayıklama efektlerini sağlar:

``` cs
void EnableDebugRenderingEffects(AzureSession session, bool highlight)
{
    DebugRenderingSettings settings = session.Actions.DebugRenderingSettings;

    // Enable frame counter text overlay on the server side rendering
    settings.RenderFrameCount = true;

    // Enable polygon count text overlay on the server side rendering
    settings.RenderPolygonCount = true;

    // Enable wireframe rendering of object geometry on the server
    settings.RenderWireframe = true;
}
```

![Hata ayıklama oluşturma](./media/debug-rendering.png)

> [!NOTE]
> Tüm hata ayıklama efektleri tüm çerçeveyi etkileyen genel ayarlardır.

## <a name="use-cases"></a>Uygulama alanları

Hata ayıklama API'si, servis bağlantısının gerçekte düzgün çalıştığını doğrulamak gibi basit hata ayıklama görevleri için tasarlanmıştır. Metin oluşturma seçenekleri, akış aşağı aktarılan video çerçevelerini doğrudan etkiler. Yeni kareler alınıp alınıp alınıp alınıp alınmamasını ve videonun doğru şekilde çözülüp çözülmemesini etkinleştirmek.

Ancak, sağlanan etkileri hizmet sağlığı içine herhangi bir ayrıntılı içgözlem vermek yok. Bu kullanım örneği için [Sunucu tarafı performans sorguları](performance-queries.md) önerilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

* Metin kaplamalarını etkinleştirmek, çok az performans yüküne neden olmaz.
* Ayrıca bindirme etkinleştirme önemsiz olmayan bir performans yükü ne olur, ancak sahneye bağlı olarak değişebilir. Karmaşık sahneleriçin bu mod, kare hızının 60-Hz hedefinin altına düşmesine neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [İşleme modları](../../concepts/rendering-modes.md)
* [Sunucu tarafı performans sorguları](performance-queries.md)
