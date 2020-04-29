---
title: İşleme Hatalarını Ayıklama
description: Sunucu tarafı hata ayıklama işleme efektlerine genel bakış
author: jumeder
ms.author: jumeder
ms.date: 04/09/2020
ms.topic: article
ms.openlocfilehash: f10c736cad9322752d5d552d29ef0c63635628a5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81868152"
---
# <a name="debug-rendering"></a>İşleme Hatalarını Ayıklama

Hata ayıklama işleme API 'SI, sunucu tarafı işlemeyi farklı hata ayıklama efektleriyle değiştirecek bir dizi genel seçenek sağlar.

## <a name="available-debug-rendering-effects"></a>Kullanılabilir hata ayıklama işleme etkileri

|Ayar                          | Etki                               |
|---------------------------------|:-------------------------------------|
|Çerçeve sayacı                    | Çerçevenin sol üst köşesine bir metin kaplaması oluşturur. Metin, işleme devam ettikçe sürekli olarak artan geçerli sunucu tarafı çerçeve KIMLIĞINI gösterir. |
|Çokgen sayısı                    | Çerçevenin sol üst köşesine bir metin kaplaması oluşturur. Metin, şu anda işlenmiş poligonları, [sunucu tarafı performans sorgularıyla](performance-queries.md) sorgulanan aynı değeri gösterir| 
|Kafesi                        | Etkinleştirilirse, sunucuda yüklü olan tüm nesne geometrisi tel çerçeve modunda işlenir. Bu modda yalnızca poligonun kenarları rasterleştirilecektir. |

Aşağıdaki kod, bu hata ayıklama efektlerini sağlar:

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
> Tüm hata ayıklama işleme etkileri tüm çerçeveyi etkileyen genel ayarlardır.

## <a name="use-cases"></a>Uygulama alanları

Hata ayıklama işleme API 'si, hizmet bağlantısının gerçekten doğru şekilde çalıştığını doğrulamak gibi basit hata ayıklama görevlerine yöneliktir. Metin işleme seçenekleri, doğrudan akışlı video çerçevelerini doğrudan etkiler. Bunları etkinleştirmek, yeni çerçevelerin alınıp doğru şekilde yeniden çözülerek doğrular.

Ancak, sunulan efektler hiçbir ayrıntılı iç denetim hizmet durumu 'na vermez. Bu kullanım örneği için [sunucu tarafı performans sorguları](performance-queries.md) önerilir.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

* Metin Yerpaylaşımları etkinleştirildiğinde performans yükü yoktur.
* Tel kafes modunun etkinleştirilmesi, sahneye bağlı olarak değişebileceğinden, önemsiz olmayan bir performans yükü doğurur. Karmaşık sahneler için bu mod, çerçeve hızının 60-Hz hedefinin altına alınmasına neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [İşleme modları](../../concepts/rendering-modes.md)
* [Sunucu tarafı performans sorguları](performance-queries.md)
