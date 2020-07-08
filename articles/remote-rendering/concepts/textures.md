---
title: Dokular
description: Doku kaynağı iş akışı
author: florianborn71
ms.author: flborn
ms.date: 02/05/2020
ms.topic: conceptual
ms.openlocfilehash: 4b87008f4172e58440c32e4ff92a3ee1f4d5efae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85565869"
---
# <a name="textures"></a>Dokular

Dokular, sabit bir [paylaşılan kaynaktır](../concepts/lifetime.md). Dokular, [öğretici: ortamı ve malzemeleri değiştirme](../tutorials/unity/materials-lighting-effects/materials-lighting-effects.md)bölümünde gösterildiği gibi [BLOB depolamadan](../how-tos/conversion/blob-storage.md) yüklenebilir ve doğrudan modellere uygulanabilir. En yaygın olarak, dokular, kendi [malzemeleri](materials.md)tarafından başvurulduğu [dönüştürülmüş bir modelin](../how-tos/conversion/model-conversion.md)bir parçası olur.

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

* Doku varlığı, SAS URI 'SI tarafından çözülebilir. İlgili yükleme işlevi `LoadTextureFromSASAsync` parametresi ile `LoadTextureFromSASParams` . [Yerleşik dokuları](../overview/features/sky.md#built-in-environment-maps)yüklerken bu değişkeni de kullanın.
* Bu doku, BLOB depolama alanı parametreleri ile doğrudan çözülebilir ve bu da [BLOB depolamanın Hesapla bağlantılı](../how-tos/create-an-account.md#link-storage-accounts)olması durumunda olabilir. Bu durumda ilgili yükleme işlevi `LoadTextureAsync` parametresiyle birlikte `LoadTextureParams` .

Aşağıdaki örnek kod, bir dokunun SAS URI 'SI (veya yerleşik doku) aracılığıyla nasıl yükleneceğini göstermektedir-diğer bir durum için yalnızca yükleme işlevi/parametresinin farklı olduğunu unutmayın:

```cs
LoadTextureAsync _textureLoad = null;
void LoadMyTexture(AzureSession session, string textureUri)
{
    _textureLoad = session.Actions.LoadTextureFromSASAsync(new LoadTextureFromSASParams(textureUri, TextureType.Texture2D));
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

```cpp
void LoadMyTexture(ApiHandle<AzureSession> session, std::string textureUri)
{
    LoadTextureFromSASParams params;
    params.TextureType = TextureType::Texture2D;
    params.TextureUrl = std::move(textureUri);
    ApiHandle<LoadTextureAsync> textureLoad = *session->Actions()->LoadTextureFromSASAsync(params);
    textureLoad->Completed([](ApiHandle<LoadTextureAsync> res)
    {
        if (res->IsRanToCompletion())
        {
            //use res->Result()
        }
        else
        {
            printf("Texture loading failed!");
        }
    });
}
```


Dokuya yönelik olarak kullanılması beklenen değere bağlı olarak, doku türü ve içeriği için kısıtlamalar olabilir. Örneğin, bir [PBR malzemelerinin](../overview/features/pbr-materials.md) kablık eşlemesi gri tonlamalı olmalıdır.

> [!CAUTION]
> ARR 'deki tüm *zaman uyumsuz* işlevler zaman uyumsuz işlem nesneleri döndürüyor. İşlem tamamlanana kadar bu nesnelere bir başvuru depolamanız gerekir. Aksi halde, C# çöp toplayıcı işlemi erken silebilir ve hiçbir şekilde bitmeyebilir. ' _TextureLoad ' üye değişkeninin yukarıdaki örnek kodda, *Tamamlanan* olay gelene kadar bir başvuruyu tutmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

* [Malzemeler](materials.md)
* [Çat](../overview/features/sky.md)
