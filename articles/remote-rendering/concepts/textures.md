---
title: Dokular
description: Doku kaynağı iş akışı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 09fa22d33377dfcbafd84f0caeb5f33a575b1bce
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80681668"
---
# <a name="textures"></a>Dokular

Dokular, sabit bir [paylaşılan kaynaktır](../concepts/lifetime.md). Dokular, [öğretici: ortamı ve malzemeleri değiştirme](../tutorials/unity/changing-environment-and-materials.md)bölümünde gösterildiği gibi [BLOB depolamadan](../how-tos/conversion/blob-storage.md) yüklenebilir ve doğrudan modellere uygulanabilir. En yaygın olarak, dokular, kendi [malzemeleri](materials.md)tarafından başvurulduğu [dönüştürülmüş bir modelin](../how-tos/conversion/model-conversion.md)bir parçası olur.

## <a name="texture-types"></a>Doku türleri

Farklı doku türlerinde farklı kullanım durumları vardır:

* **2B dokular** genellikle [malzemelerde](materials.md)kullanılır.
* [Gök](../overview/features/sky.md) **haritaları için cubemaps** kullanılabilir.

## <a name="supported-texture-formats"></a>Desteklenen doku biçimleri

ARR 'ye verilen tüm dokuların [DDS biçiminde](https://en.wikipedia.org/wiki/DirectDraw_Surface)olması gerekir. Tercihen, msunucudan haritalar ve doku sıkıştırması vardır. Dönüştürme işlemini otomatikleştirmek istiyorsanız, bkz. [TexConv komut satırı aracı](../resources/tools/tex-conv.md) .

## <a name="loading-textures"></a>Dokular yükleniyor

Bir doku yüklerken, beklenen türünü belirtmeniz gerekir. Tür uyuşmazlıkları varsa doku yükleme başarısız olur.
Aynı URI 'yi iki kez içeren bir doku yüklemek, [paylaşılan bir kaynak](../concepts/lifetime.md)olduğundan aynı doku nesnesini döndürür.

Model yüklemeye benzer şekilde, kaynak blob depolamada bir doku varlığını ele almak için iki çeşit vardır:

* Doku varlığı, SAS URI 'SI tarafından çözülebilir. İlgili yükleme işlevi parametresi `LoadTextureFromSASAsync` `LoadTextureFromSASParams`ile. [Yerleşik dokuları](../overview/features/sky.md#built-in-environment-maps)yüklerken bu değişkeni de kullanın.
* Bu doku, BLOB depolama alanı parametreleri ile doğrudan çözülebilir ve bu da [BLOB depolamanın Hesapla bağlantılı](../how-tos/create-an-account.md#link-storage-accounts)olması durumunda olabilir. Bu durumda ilgili yükleme işlevi parametresiyle `LoadTextureAsync` `LoadTextureParams`birlikte.

Aşağıdaki örnek kod, bir dokunun SAS URI 'SI (veya yerleşik doku) aracılığıyla nasıl yükleneceğini göstermektedir-diğer bir durum için yalnızca yükleme işlevi/parametresinin farklı olduğunu unutmayın:

``` cs
LoadTextureAsync _textureLoad = null;
void LoadMyTexture(AzureSession session, string textureUri)
{
    _textureLoad = session.Actions.LoadTextureAsync(new LoadTextureParams(textureUri, TextureType.Texture2D));
    _textureLoad.Completed +=
        (LoadTextureAsync res) =>
        {
            if (res.IsRanToCompletion)
            {
                //use res.Result
            }
            else
            {
                System.Console.WriteLine("Texture loading failed!");
            }
            _textureLoad = null;
        };
}
```

Dokuya yönelik olarak kullanılması beklenen değere bağlı olarak, doku türü ve içeriği için kısıtlamalar olabilir. Örneğin, bir [PBR malzemelerinin](../overview/features/pbr-materials.md) kablık eşlemesi gri tonlamalı olmalıdır.

> [!CAUTION]
> ARR 'deki tüm *zaman uyumsuz* işlevler zaman uyumsuz işlem nesneleri döndürüyor. İşlem tamamlanana kadar bu nesnelere bir başvuru depolamanız gerekir. Aksi halde, C# çöp toplayıcı işlemi erken silebilir ve hiçbir şekilde bitmeyebilir. ' _TextureLoad ' üye değişkeninin yukarıdaki örnek kodda, *Tamamlanan* olay gelene kadar bir başvuruyu tutmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
* [Çat](../overview/features/sky.md)
